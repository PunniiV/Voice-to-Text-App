# Voice-to-Text-app

## Installation

1️⃣ Clone this repository:
git clone https://github.com/your-username/voice-to-text-app.git
cd voice-to-text-app
2️⃣ Install required dependencies:
pip install tkinter speechrecognition pyaudio
⚠️ If you face a pyaudio error, install it manually:
pip install pipwin
pipwin install pyaudio
3️⃣ Run the application:
python voice_to_text.py


## Usage

✅ Real-time speech-to-text conversion – Allows users to dictate text instead of typing.
✅ Simple and interactive GUI – A user-friendly interface for easy interaction.
✅ Background noise adjustment – Improves speech recognition accuracy in noisy environments.
✅ Multi-threading for smooth performance – Prevents the application from freezing while processing speech.

## Code Explanation

import tkinter as tk
import threading
import speech_recognition as sr

# Function to capture audio and convert it to text
def transcribe_audio():
    def recognize():
        recognizer = sr.Recognizer()

        # Use the microphone to listen to the user's speech
        with sr.Microphone() as source:
            text_box.insert(tk.END, "Listening...\n")
            root.update_idletasks()  # Update UI
            recognizer.adjust_for_ambient_noise(source)  # Adjust for ambient noise
            audio = recognizer.listen(source)

        try:
            text_box.insert(tk.END, "Processing...\n")
            root.update_idletasks()
            # Recognize the speech using Google's speech-to-text API
            text = recognizer.recognize_google(audio)
            text_box.insert(tk.END, "You said: " + text + "\n")
        except sr.UnknownValueError:
            text_box.insert(tk.END, "Sorry, I couldn't understand that.\n")
        except sr.RequestError as e:
            text_box.insert(tk.END, f"Could not request results from Google Speech API; {e}\n")

    # Run recognition in a separate thread to prevent GUI freezing
    threading.Thread(target=recognize, daemon=True).start()

# Setting up the GUI
root = tk.Tk()
root.title("Voice to Text App")
root.geometry("500x400")

# Create a Text widget for displaying transcribed text
text_box = tk.Text(root, wrap=tk.WORD, height=15, width=50)
text_box.pack(pady=20)

# Create a Button to trigger speech recognition
start_button = tk.Button(root, text="Start Listening", command=transcribe_audio, height=2, width=20)
start_button.pack()

# Run the Tkinter event loop
root.mainloop()

