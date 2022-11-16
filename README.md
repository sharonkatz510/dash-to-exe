# dash-to-exe
A walkthrough for easy conversion of Dash client-side app to an exe file

## Disclaimer
I've only tried this with my dash app which is client-side only (meaning it needs no interface with an external database/site)
I've only tested it on a windows mahine with chrome browser

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
### Note that you can change 8050 to other port numer you wish as long as its consistent with the following:
Finally modify the line that runs the dash app to be:
~~~
if __name__ == '__main__':
    Timer(1, open_browser).start();
    app.run_server(debug=True, port=8050)
~~~

### Now your dash app should be able to open its own browser automatically. You better verify this by running it

Now that we have this covered, let's pack it into an exe.
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

