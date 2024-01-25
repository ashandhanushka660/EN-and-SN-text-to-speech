import tkinter as tk
from gtts import gTTS
import os
import pygame
from io import BytesIO

def play_audio(text, language):
    global audio_stream
    audio_stream = BytesIO()
    text_to_speech = gTTS(text=text, lang=language, slow=False)
    text_to_speech.write_to_fp(audio_stream)
    audio_stream.seek(0)
    pygame.mixer.music.load(audio_stream)
    pygame.mixer.music.play()

def stop_audio():
    pygame.mixer.music.stop()

def on_button_click():
    user_input = entry.get()
    selected_language = language_var.get().lower()

    if selected_language == "english":
        selected_language = "en"

    stop_audio()
    play_audio(user_input, selected_language)

# Initialize the pygame mixer module
pygame.mixer.init()

# Create the window for the Speech App
window = tk.Tk()
window.title("Speech App")

# Label and Entry widget for text input
entry_label = tk.Label(window, text="Enter Text:")
entry_label.pack()

entry = tk.Entry(window, width=30)
entry.pack()

# Menu for language selection
language_label = tk.Label(window, text="Select Language:")
language_label.pack()

languages = ["English", "Sinhala"]
language_var = tk.StringVar(window)
language_var.set(languages[0])

language_dropdown = tk.OptionMenu(window, language_var, *languages)
language_dropdown.pack()

# Button to generate speech
generate_button = tk.Button(window, text="Generate Speech", command=on_button_click)
generate_button.pack()

# Button to stop speech
stop_button = tk.Button(window, text="Stop Speech", command=stop_audio)
stop_button.pack()

# Run the main loop to keep the window active
window.mainloop()
