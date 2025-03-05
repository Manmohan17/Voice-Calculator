# Voice-Calculator
import speech_recognition as sr
import pyttsx3

# Initialize text-to-speech engine
def speak(text):
    engine = pyttsx3.init()
    engine.say(text)
    engine.runAndWait()

# Capture voice input
def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)

    try:
        command = recognizer.recognize_google(audio)
        print("You said:", command)
        return command.lower()
    except sr.UnknownValueError:
        print("Sorry, I could not understand.")
        speak("Sorry, I could not understand.")
        return None
    except sr.RequestError:
        print("Speech service is unavailable.")
        speak("Speech service is unavailable.")
        return None

# Perform calculation
def calculate(expression):
    try:
        result = eval(expression)
        return result
    except Exception as e:
        return f"Error: {e}"

# Voice Calculator with continuous operation
def voice_calculator():
    speak("Welcome to the Voice Calculator. Say your mathematical expression or 'exit' to stop.")
    while True:
        speak("Please say your mathematical expression.")
        expression = listen()

        if expression:
            # Stop condition
            if "exit" in expression or "stop" in expression:
                speak("Exiting Voice Calculator. Goodbye!")
                break

            # Replace words with math operators
            expression = expression.replace("plus", "+").replace("minus", "-")
            expression = expression.replace("times", "*").replace("into", "*")
            expression = expression.replace("divided by", "/").replace("by", "/")
            expression = expression.replace("x", "*")  # Handling 'x' as multiplication

            # Perform calculation
            result = calculate(expression)
            if isinstance(result, (int, float)):
                speak(f"The result is {result}")
            else:
                speak("There was an error in calculation.")
            
            print("Result:", result)

# Run the calculator
if __name__ == "__main__":
    voice_calculator()


