import datetime
import pyttsx3
import speech_recognition as sr

# Initialize the speech engine
engine = pyttsx3.init()

def speak(text):
    engine.say(text)
    engine.runAndWait()

def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)
        try:
            text = recognizer.recognize_google(audio)
            print("You said: " + text)
            return text
        except sr.UnknownValueError:
            speak("Sorry, I did not understand that.")
            return None
        except sr.RequestError:
            speak("Sorry, I'm having trouble connecting to the internet.")
            return None

def respond_to_query(query):
    if 'time' in query:
        now = datetime.datetime.now()
        speak(f"The current time is {now.strftime('%H:%M:%S')}.")
    elif 'hello' in query:
        speak("Hello! How can I assist you today?")
    elif 'stop' in query:
        speak("Goodbye!")
        return False
    else:
        speak("Sorry, I don't understand that command.")
    return True

def main():
    speak("Hello, I am J.A.V.I.S. How can I assist you today?")
    while True:
        query = listen()
        if query:
            if not respond_to_query(query.lower()):
                break

if __name__ == "__main__":
    main()
