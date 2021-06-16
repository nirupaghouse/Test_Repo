import pywinauto
from pywinauto import Desktop, Application
from pywinauto.controls.win32_controls import ButtonWrapper
from pandas.io import clipboard
#import clipboard

import win32clipboard
#########################To verify if PING ID window is already Opened ################################################


# kills pING ID  applications if already opened
import os

def kill_by_process_name_shell(name):
    os.system("taskkill /f /im " + name)
    os.system('cls')


kill_by_process_name_shell("PingID.exe") 

###################################################################################################################

# Launch PING ID from Program files

app = Application(backend="uia").start("C:\Program Files (x86)\Ping Identity\PingID\PingID.exe",timeout=20)

# To verify of PING iD window is displayed successfully

existFlag  = app.window(title ="PingID").exists(timeout =20)
print(existFlag)
if(existFlag):
    print("PingID window  exists")
pingid_dlg = app.window(title='PingID')

pingid_dlg.wait('ready')
# Click on Refresh Button

pingid_dlg.Refresh.click()

pingid_dlg.wait('ready')

# Copy the token

pingid_dlg.Copy.click()

pingid_dlg.wait('ready')

# Paste the clipboard content
clipboard_content = clipboard.paste()


print(clipboard_content)

pingid_dlg.Close.click()

# If directory doesnot exists, python creates "MFA" directory

if not os.path.exists('MFA'):
    os.makedirs('MFA')


f = open("MFA\pingid.txt", "w", encoding='utf-8')

# Writes the clipboard content to the a notepad file
f.write(clipboard_content)

# Closed the notepad file

f.close() 



