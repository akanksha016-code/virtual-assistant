import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import pywhatkit
import pyjokes
import webbrowser

# Initialize the speech engine
engine = pyttsx3.init()
engine.setProperty('rate', 150)  # Speaking speed

def speak(text):
    engine.say(text)
    engine.runAndWait()

def greet_user():
    hour = datetime.datetime.now().hour
    if 0 <= hour < 12:
        speak("Good morning!")
    elif 12 <= hour < 18:
        speak("Good afternoon!")
    else:
        speak("Good evening!")
    speak("I am your assistant. How can I help you today?")

def listen_command():
    listener = sr.Recognizer()
    try:
        with sr.Microphone() as source:
            print("Listening...")
            audio = listener.listen(source)
            command = listener.recognize_google(audio)
            command = command.lower()
            print("You said:", command)
            return command
    except:
        speak("Sorry, I didn't catch that.")
        return ""

def run_assistant():
    greet_user()
    while True:
        command = listen_command()

        if 'time' in command:
            time = datetime.datetime.now().strftime('%I:%M %p')
            speak(f"The time is {time}")

        elif 'date' in command:
            date = datetime.datetime.now().strftime('%B %d, %Y')
            speak(f"Today's date is {date}")

        elif 'who is' in command or 'what is' in command:
            topic = command.replace('who is', '').replace('what is', '')
            try:
                info = wikipedia.summary(topic, sentences=2)
                speak(info)
            except:
                speak("Sorry, I couldn't find information about that.")

        elif 'open youtube' in command:
            webbrowser.open('https://youtube.com')
            speak("Opening YouTube")

        elif 'joke' in command:
            joke = pyjokes.get_joke()
            speak(joke)

        elif 'play' in command:
            song = command.replace('play', '')
            speak(f"Playing {song}")
            pywhatkit.playonyt(song)

        elif 'stop' in command or 'exit' in command or 'quit' in command or 'bye' in command:
            speak("Goodbye! Have a nice day.")
            break

        else:
            speak("I didn't understand. Please say that again.")

# Start the assistant
run_assistant()
