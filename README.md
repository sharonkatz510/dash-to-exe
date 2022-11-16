# dash-to-exe
A walkthrough for easy conversion of Dash client-side app to an exe file

## Disclaimer
I've only tried this with my dash app which is client-side only (meaning it needs no interface with an external database/site)
I've only tested it on a windows machine with chrome browser

## Quick overview & who is this tutorial good for
This tutorial assumes you already have a nice running dash app, it can either be static or dynamic, that runs well on your local machine.
The goals of this tutorial are:
1. Add a short line of code to open the browser automatically when your dash app is running so you don't need to copy/paste the
address to your browser.
2. Pack the whole thing to a folder with an executable file (.exe) that any user can launch on its machine without python installation
and without any programming knowledge.

## 1st step: open the browser session automatically
To do so, we follow the instructions decribed [here]('https://community.plotly.com/t/auto-open-browser-window-with-dash/31948/2')
Meaning we add two imports to our dash app:
~~~
import webbrowser
from threading import Timer
~~~
We add the following function in our code (I would suggest adding it at the end of the file for readability):
~~~
def open_browser():
	webbrowser.open_new("http://localhost:{}".format(8050))
~~~
**Note that you can change 8050 to other port numer you wish as long as its consistent with the following:**
Finally modify the line that runs the dash app to be:
~~~
if __name__ == '__main__':
    Timer(1, open_browser).start();
    app.run_server(debug=True, port=8050)
~~~

### Now your dash app should be able to open its own browser automatically. You better verify this by running it

Having covered the easy part, let's now pack our app into an exe.
To do so you'll first have to download a python package called **pyinstaller** which basically goes over your app code, finds all the necassery imports and modules
and packs them into a single folder. after doing so it packs your python app as exe to be run on any computer regardless of python installation.
The easiest way to install pyinstaller is using pip (just write the following in your terminal/cmd):
~~~
pip install pyinstaller
~~~
once this is done you can execute the following in your command line (when you are in the directory that contains your app):
~~~
pyinstaller --onedir <your-app-name>.py
~~~
This process might take a while and it will eventually create two directories in your app directory, one is "build" and the other is "dist", another file created is <your-app-name).spec.
The "build" directory can be deleted together with the spec file.
Inside the "dist" folder you'll find a subfolder with the name of your app and inside of it you will find the exe file that contains the same name as your app name.
for example if your python app is named "app.py" you'll find a folder with the name **dist/app** and a **app.exe** file inside of it.

**Before you run to tell your friends** open the command prompt in the folder where your new exe is located and run it inside your CMD.
This way if/when it fails you'll know why. To do this simply open CMD in your desired folder and execute the following:
~~~
<your-app-name>.exe
~~~

## Simple troubleshooting: When your app fails because a file is missing
If you try the above, and your app fails to execute, and in the CMD log you see that the reason is some file missing (for example for me it was **"distributed/distributed.yaml"** that was missing), then try to locate it in your python site-packages directory. Then you need to redo the pyinstaller packing only this time you explicitly tell pyinstaller where to find the specific missing file like so:
~~~
pyinstaller --onedir --add-data "<python_path>/Lib/site-packages/distributed/distributed.yaml;./distributed" <your-app-name>.py
~~~
**Be sure to replace the python_path with your actual python path** for example on my PC it's:
~~~
"C:/Users/User/anaconda3"
~~~

## Final Notes:
1. You can also use the **--onefile** flag of pyinstaller instead of **--onedir**, this will create only a single exe file inside /dist/<your-app-name> and there will be now supplemental files. However, this way the exe file runs much slower.
2. Your exe file needs to stay inside <your-app-name> folder to use all its dependencies. So if you send it to another computer make sure to pack the entire folder and not just the exe file. There are nicer ways to deliver this folder instead of as a ZIP (take a look at [NINS]('https://youtu.be/UZX5kH72Yx4?t=330'))
	
**Hope you found this tutorial usefull and if you have any improvement suggestions feel free to open an issue**

