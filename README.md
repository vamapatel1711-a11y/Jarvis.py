import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import webbrowser
import os

# Initialize pyttsx3 (text to speech)
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)  # male = 0, female = 1

def speak(audio):
    print(f"Jarvis: {audio}")   # Console par bhi print karega
    engine.say(audio)
    engine.runAndWait()

# Wish the user
def wishMe():
    hour = int(datetime.datetime.now().hour)
    if 0 <= hour < 12:
        speak("Good Morning!")
    elif 12 <= hour < 18:
        speak("Good Afternoon!")
    else:
        speak("Good Evening!")

    speak("I am Jarvis Sir. Please tell me how may I help you")

# Take command from mic
def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("🎤 Listening...")
        r.adjust_for_ambient_noise(source)  # background noise handle
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language="en-in")
        print(f"✅ You said: {query}")
        speak(f"You said: {query}")   # Repeat karega
        return query
    except Exception as e:
        print("❌ Error:", e)
        speak("Say that again please...")
        return "None"

# Main program
if __name__ == "__main__":
    wishMe()

    while True:
        query = takeCommand().lower()

        if query == "none":
            continue

        # Wikipedia search
        if 'wikipedia' in query:
            speak("Searching Wikipedia...")
            query = query.replace("wikipedia", "").strip()
            try:
                if query:
                    results = wikipedia.summary(query, sentences=2)
                    speak("According to Wikipedia")
                    speak(results)
                else:
                    speak("Please tell me what to search on Wikipedia")
            except wikipedia.exceptions.DisambiguationError:
                speak("There are multiple results, please be more specific.")
            except wikipedia.exceptions.PageError:
                speak("Sorry, I could not find anything on Wikipedia")

        # Time
        elif 'time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")
        
        # YouTube
        elif 'open youtube' in query:
            speak("Opening YouTube")
            webbrowser.open("youtube.com")

        # Google
        elif 'open google' in query:
            speak("Opening Google")
            webbrowser.open("google.com")

        # Play Music
        elif 'play music' in query:
            music_path = r"D:\premika_ne_pyare_veena.mp3" 
            if os.path.exists(music_path):
                speak("Playing your music")
                os.startfile(music_path)
            else:
                speak("Sorry, music file not found.")

        # Calculator (voice expression)
        elif 'calculate' in query:
            try:
                expression = query.replace("calculate", "").strip()
                expression = expression.replace("plus", "+")
                expression = expression.replace("minus", "-")
                expression = expression.replace("multiply", "*")
                expression = expression.replace("divide", "/")

                result = eval(expression)
                speak(f"The result is {result}")
            except Exception as e:
                speak("Sorry, I could not calculate that. Please try again.")
  
        # VS Code
        elif 'open code' in query:
            codepath = r"C:\Users\ADMIN\AppData\Local\Programs\Microsoft VS Code\Code.exe"
            if os.path.exists(codepath):
                speak("Opening Visual Studio Code")
                os.startfile(codepath)
            else:
                speak("VS Code not found on this path.")

        # Open Notepad
        elif 'open notepad' in query:
            speak("Opening Notepad")
            os.startfile("notepad.exe")

        elif 'open excel' in query:
            speak("Opening Excel")
            os.startfile("excel.exe")

        # Open Windows Calculator
        elif 'open calculator' in query:
            speak("Opening Calculator")
            os.startfile("calc.exe")

        # System Shutdown
        elif 'shutdown' in query:
            speak("Shutting down the system, Sir")
            os.system("shutdown /s /t 5")

        # Restart System
        elif 'restart' in query:
            speak("Restarting the system, Sir")
            os.system("shutdown /r /t 5")

        # Log Out
        elif 'log out' in query or 'sign out' in query:
            speak("Signing out, Sir")
            os.system("shutdown -l")

        # Exit
        elif 'exit' in query or 'quit' in query or 'stop' in query:
            speak("Okay Sir, Jarvis shutting down. Have a nice day!")
            break
