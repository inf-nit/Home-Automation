# Home-Automation
This code will help you in creating an home automation system using your voice in python programming language
import speech_recognition as sr
import pyttsx3
import pywhatkit
import datetime
import random
import wikipedia
import pyfirmata #install pyfirmata library in arduino and in python
import cv2
from time import sleep
import winsound
import pyjokes


listener = sr.Recognizer()
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)


def talk(text):
    engine.say(text)
    print(text)
    engine.runAndWait()


def take_command():

    try:
        with sr.Microphone() as source:
            print("Listening...")
            voice = listener.listen(source)
            command = listener.recognize_google(voice)
            command = command.lower()
            if 'jarvis' in command:
                command = command.replace('jarvis', '')
                # talk(command)
                # print(command)
            else:
                pass

    except:
        pass
    return command


try:

    board = pyfirmata.Arduino('com3')
    led1 = board.get_pin('d:6:o')
    led2 = board.get_pin('d:7:o')
    led3 = board.get_pin('d:8:o')

except Exception as e:
    talk("cannot connect to the main system")


def run_Jarvis():
    command = take_command()
    if 'play' in command:
        song = command.replace('play', '')
        talk('playing ' + song)
        print(song)
        pywhatkit.playonyt(song)
    elif 'wake up' in command:
        option = random.randint(1, 2)
        if option == 1:
            talk("Always there for you sir")
        elif option == 2:
            talk("Beside you sir")
        else:
            pass
    elif 'talk to me' in command:
        talk("You need it , I got it BOSS")
    elif 'check system' in command:
        talk("checking systems...")
        if board == pyfirmata.Arduino('com3') is True:
            talk("system good to go.")
        else:
            talk("connection defect detected.")
    elif 'sarcasm time' in command:
        talk(pyjokes.get_joke())

    elif "time" in command:
        time = datetime.datetime.now().strftime('%H:%M %p')
        talk("current time is " + time)
    elif "search" in command:
        searched = command.replace("search", '')
        info = wikipedia.summary(searched, 2)
        talk(info)
    elif "good morning" in command:
        greetMrng = ("Good Morning Boss")
        talk(greetMrng)
    elif "turn on alpha" in command:
        led1.write(1)
        talk("LED 1 on")
    elif "turn on beta" in command:
        led2.write(1)
        talk("LED 2 on")
    elif "turn on charlie" in command:
        led3.write(1)
        talk("LED 3 on")
    elif "turn off alpha" in command:
        led1.write(0)
        talk("LED 1 off")
    elif "turn off beta" in command:
        led2.write(0)
        talk("LED 2 off")
    elif "turn off charlie" in command:
        led3.write(0)
        talk("LED 3 off")
    elif "all on" in command:
        led1.write(1)
        led2.write(1)
        led3.write(1)
        talk("All LED on..")
    elif "all off" in command:
        led1.write(0)
        led2.write(0)
        led3.write(0)
        talk("All LED off..")
    elif "start the party" in command:
        talk("Starting the party...")
        winsound.PlaySound('a.mp3', winsound.SND_LOOP)  # causing error
        while(True):
            led1.write(1)
            sleep(1)
            led1.write(0)
            sleep(1)
            led2.write(1)
            sleep(1)
            led2.write(0)
            sleep(1)
            led3.write(1)
            sleep(1)
            led3.write(0)
            sleep(1)

    elif "open" in command:
        website = command.replace("open", "")
        pywhatkit.search(website)
        talk("opening" + website)
    elif "take screenshot" in command:
        pywhatkit.take_screenshot()
        talk("Screenshot taken")
    elif "check camera" in command:# under maintanence
        pass
        # talk(" checking camera feeds...")
        # while True:
        #     camera = cv2.VideoCapture(0)
        #     ret, frame = camera.read()
        #     cv2.imshow('Cam feed',frame)
        #     if cv2.waitKey(10)== ord('q'):
        #         break
    else:
        talk("Please Repeat...")
        # pywhatkit.


while(True):

    run_Jarvis()
