# Spanish-translator

import speech_recognition as sr
from googletrans import Translator
import pyttsx3
import tkinter as tk

def recognize_and_translate():
    input_audio_file = "input.mp3"
    output_audio_file = "output.mp3"
    
    recognizer = sr.Recognizer()
    
    # Load the input audio file
    audio = sr.AudioFile(input_audio_file)
    
    with audio as source:
        audio_data = recognizer.record(source)
        
    try:
        # Recognize English speech from the audio
        english_text = recognizer.recognize_google(audio_data)
        
        # Translate English text to Spanish
        translator = Translator()
        translated_text = translator.translate(english_text, src='en', dest='es')
        
        # Use pyttsx3 to convert the translated text to speech
        engine = pyttsx3.init()
        engine.save_to_file(translated_text.text, output_audio_file)
        engine.runAndWait()
        
        result_label.config(text="Translation and audio saved as 'output.mp3'")
        
    except sr.UnknownValueError:
        result_label.config(text="Google Web Speech Recognition could not understand audio.")
    except sr.RequestError as e:
        result_label.config(text=f"Could not request results from Google Web Speech Recognition: {e}")

def translate_button_clicked():
    recognize_and_translate()

# Create a GUI window
root = tk.Tk()
root.title("Audio Translator")

# Create a button to initiate translation
translate_button = tk.Button(root, text="Translate and Save Audio", command=translate_button_clicked)

# Create a label to display the result
result_label = tk.Label(root, text="")

# Place widgets in the window
translate_button.pack()
result_label.pack()

root.mainloop()
