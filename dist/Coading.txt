from tkinter import *
from tkinter import scrolledtext
import pyttsx3
import pywhatkit
import datetime
import subprocess
import webbrowser
import winsound
import threading
from PIL import ImageTk, Image
import time
from tkinter.ttk import *
import speech_recognition as sr
tk = Tk()
tk.geometry('1400x800')
bg = PhotoImage(file='Background image.png')
my_label = Label(tk , image=bg)
my_label.place(x=0 , y=0 ,relheight=1,relwidth=1)
canvas = Canvas(tk, width=300, height=250)
canvas.pack()
img = ImageTk.PhotoImage(Image.open("AI Image.jpg"))
canvas.create_image(20, 20,image=img)
f_font = ("Comic Sans MS", 13, "bold")
s = scrolledtext.ScrolledText(tk, wrap=WORD, width=80, height=15, font=f_font)
s.pack()
a = 1
bc = 1
fh = open("audios.txt",'r+')
qqp = int(fh.read())
fh.close()
down = PhotoImage(file = "Microphone.png")
indi = PhotoImage(file = "Indicator.png")
engine = pyttsx3.init()
voice = engine.getProperty("voices")
engine.setProperty('voice', voice[qqp].id)
engine.setProperty("rate", 130)
def imageupdate1():
    b.config(image=indi)
    tk.update_idletasks()
def imageupdate2():
    b.config(image=down)
    tk.update_idletasks()
def ttk():
    tk.update_idletasks()
def start():
    try:
        r = sr.Recognizer()
        with sr.Microphone() as source:
            imageupdate1()
            r.energy_threshold = 2600
            audio = r.listen(source ,timeout = 3)
            text = r.recognize_google(audio)
            imageupdate2()
            main(text)
    except:
        imageupdate2()
def main(text):
    global qqp
    r = sr.Recognizer()
    with sr.Microphone() as source:
        text = text.lower()
        while True:
            if 'play' in text:
                song = text.replace("play", '')
                s.insert('insert', "\nPlaying" + song)
                ttk()
                engine.say("playing")
                engine.say(song)
                engine.runAndWait()
                pywhatkit.playonyt(song)
                break
            elif "can you change your voice" in text or "change your voice" in text:
                s.insert('insert', "\nYes i can change my voice")
                ttk()
                engine.say("Yes i can change my voice")
                engine.runAndWait()
                voice = engine.getProperty("voices")
                fh = open("audios.txt",'r+')
                me = fh.read()
                if me =='1' or me == 1:
                    qqp = 0
                else:
                    qqp = 1
                engine.setProperty('voice', voice[qqp].id)
                s.insert('insert', "\nHere is an example of my another voice ,would you like me to use this one?")
                ttk()
                engine.say("Here is an example of my another voice ,would you like me to use this one?")
                engine.runAndWait()
                s.insert('insert', "\nSpeak....")
                imageupdate1()
                r.energy_threshold = 2600
                audio = r.listen(source)
                text = r.recognize_google(audio)
                text = text.lower()
                imageupdate2()
                if text in "lonoKnowknewNOoO":
                    s.insert('insert', "\nOh sorry,i have only two voices,i will change my voice back")
                    ttk()
                    engine.say("Oh sorry,i have only two voices,i will change my voice back")
                    engine.runAndWait()
                    if qqp == 0:
                        qqp = 1
                    else:
                        qqp = 0
                    fh = open("audios.txt", 'w+')
                    fh.write(str(qqp))
                    fh.close()
                    voice = engine.getProperty("voices")
                    engine.setProperty('voice', voice[qqp].id)
                    s.insert('insert', "\nWhat can i do for you?")
                    ttk()
                    engine.say("What can i do for you?")
                    engine.runAndWait()
                    break
                else:
                    fh = open("audios.txt", 'w+')
                    fh.write(str(qqp))
                    fh.close()
                    s.insert('insert', "\nOk Thank you!")
                    ttk()
                    engine.say("ok Thank you!")
                    engine.runAndWait()
                    break
            elif "alarm" in text:
                s.insert('insert', "\nAlright,at which time:")
                ttk()
                engine.say("Alright at which time:")
                engine.runAndWait()
                ttk()
                imageupdate1()
                r.energy_threshold = 2600
                audio = r.listen(source)
                text = r.recognize_google(audio)
                imageupdate2()
                text = text.upper()
                text = text[:-1]
                text = text.replace('.', '')
                if text[0] != '1':
                    text = '0' + text
                def countdown():
                    while True:
                        global tiime
                        tiime = datetime.datetime.now().strftime("%I:%M %p")
                        time.sleep(1.0)
                        if tiime >= text:
                            s.insert('insert', "\nWake up....")
                            for i in range(1, 12):
                                winsound.PlaySound("sound2.wav", winsound.SND_ALIAS)
                                winsound.Beep(3600, 1000)
                            break
                t = threading.Thread(target=countdown)
                t.daemon = True
                t.start()
                s.insert('insert', "\nYour alarm is set for " + text)
                ttk()
                engine.say("Your alarm is set for")
                engine.say(text)
                engine.runAndWait()
                break
            elif "shutdown the laptop" in text or "shutdown laptop" in text or "shut down laptop" in text:
                s.insert('insert', "\n shutting down the Laptop")
                ttk()
                engine.say("shutting down the Laptop")
                engine.runAndWait()
                def closelap():
                    subprocess.Popen("C:\\Windows\\System32\\shutdown.exe /s /t 0")
                tk.destroy()
                closelaptop = threading.Thread(target=closelap)
                closelaptop.start()
            elif "open google" in text or "open chrome" in text or "open Google" in text:
                s.insert('insert', "\nopening Google chrome")
                ttk()
                engine.say("opening Google chrome")
                engine.runAndWait()
                try:
                    subprocess.Popen("C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe")
                except:
                    s.insert('insert',"\nSorry i guess you don't have google chrome")
                    engine.say("Sorry i guess you don't have google chrome")
                    engine.runAndWait()
                break
            elif "open ccleaner" in text or "open c cleaner" in text:
                s.insert('insert', "\nopening CCleaner")
                ttk()
                engine.say("opening CCleaner")
                engine.runAndWait()
                try:
                    subprocess.Popen("C:\\Program Files\\CCleaner\\CCleaner64.exe")
                except:
                    s.insert('insert', "\nSorry i guess you don't have CCleaner")
                    engine.say("Sorry i guess you don't have CCleaner")
                    engine.runAndWait()
                break
            elif "open youtube" in text.lower():
                s.insert('insert', "\nOpening youtube")
                ttk()
                engine.say("Opening youtube")
                engine.runAndWait()
                webbrowser.open("https://www.youtube.com")
                break
            elif "what can you do" in text or "what else can you do" in text:
                s.insert('insert',"\nI can play songs,\nChange my voice,\ntell you the current time,\nset you an alarm,\nI can open Google,\nyoutube,\nCCleaner,\nGmail,\nand I can search whatever you say")
                ttk()
                engine.say("I can play songs, Change my voice , tell you the current time , set you an alarm , I can open Google , youtube , CCleaner , Gmail and I can search whatever you say")
                engine.runAndWait()
                break
            elif "open gmail" in text.lower() or "open mail" in text.lower():
                s.insert('insert', "\nOpening Gmail")
                ttk()
                engine.say("Opening Gmail")
                engine.runAndWait()
                webbrowser.open_new_tab("https://mail.google.com/mail/u/0/?tab=rm&ogbl#inbox")
                break
            elif "search" in text.lower():
                s.insert('insert', "\n" + text.replace("search" , ""))
                ttk()
                taburl = "https://www.google.com/search?q="
                webbrowser.open_new_tab(taburl + text.replace("search" , ''))
                break
b = Button(tk, command=start)
b.config(image=down)
b.pack()
def first():
    global a , qqp , engine
    if a == 1:
        tk.update()
        s.insert('insert', 'Hello This is Tiger AI,What can i do for you?')
        ttk()
        engine.say("Hello This is Tiger AI,What can i do for you?")
        engine.runAndWait()
        a = a + 2
first()
def check():
    while True:
        r = sr.Recognizer()
        with sr.Microphone() as source:
            try:
                r.energy_threshold = 2600
                audio = r.listen(source , timeout=2)
                sample = r.recognize_google(audio)
            except:
                print("None")
                continue
        print(sample)
        if "stop" in sample or "break" in sample or "quit" in sample:
            tk.destroy()
        if (sample is not None) and (sample.count("tiger") > 0):
            s.insert('insert',"\nYes , what can i do for you?")
            ttk()
            engine.say("yes , what can i do for you?")
            engine.runAndWait()
            b.invoke()
loop = threading.Thread(target=check)
loop.daemon = True
loop.start()
tk.mainloop()

