---
layout: page
title: Setup 
permalink: /setup/
---

## Software Requirements & Setup Instructions
To fully participate in this boot camp, you will need access to the software described below on your own laptop (N.B. You will likely need Administer privileges/permissions to install some of these). 

<br>

### The Bash Shell
Bash is a commonly-used shell that gives you the power to do simple tasks more quickly.

**Windows:** Install [mobaxterm](http://mobaxterm.mobatek.net), an enhanced terminal for Windows. Once installed, please visit http://mobaxterm.mobatek.net/plugins.html and obtain the Git plugin and put it in the same directory as your MobaXterm executable (Typically ????).

**Mac OS X:** You do not need to install anything. You ca access bash from the **Terminal** (found in **/Applications/Utilities**). You may want to keep Terminal in your dock for this workshop.

**Linux:** There is no need to install anything.

<br>

### Git
Git is a version control system that lets you track who made changes to what and when. To work with GitHub and BitBucket you will need a [supported web browser](https://help.github.com/articles/supported-browsers/) (current versions of Chrome, Firefox or Safari, or Internet Explorer version 9 or above).

**Windows:** Git should be installed as a plugin with mobaxterm](http://mobaxterm.mobatek.net), see above. 

**Mac OS X:** For OS X 10.9 and higher, install Git for Mac by downloading and running the most recent "mavericks" installer from this [list](http://sourceforge.net/projects/git-osx-installer/files/). After installing Git, there will not be anything in your /Applications folder, as Git is a command line program. For older versions of OS X (10.5-10.8) use the most recent available installer labelled "snow-leopard" [available here](http://sourceforge.net/projects/git-osx-installer/files/).

**Linux:** If Git is not already available on your machine you can install it via your distro's package manager. For Debian/Ubuntu run 

    sudo apt-get install git 

and for Fedora run 

    sudo yum install git.

<br>

### Text Editor
When you're writing code, it's nice to have a text editor that is optimized for writing code, with features like automatic color-coding of key words. The default text editor on Mac OS X and Linux is usually set to Vim, which is not famous for being intuitive. if you accidentally find yourself stuck in it, try typing the **escape key**, followed by **:q!** (colon, lower-case 'q', exclamation mark), then hitting Return to return to the shell.

**Windows:** nano is a basic editor and the default that instructors use in the workshop. To install it, download the [Windows installer](http://files.software-carpentry.org/SWCarpentryInstaller.exe) and double click on the file to run it. This installer requires an active internet connection.

**Mac OS X:** nano should be pre-installed.

**Linux:** nano should be pre-installed.


<br>

### Python
Python is a popular language for scientific computing. Installing all of its scientific packages individually can be a bit difficult, so we recommend an all-in-one installer.

Regardless of how you choose to install it, please make sure you install Python version 2.x and not version 3.x (e.g., 2.7 is fine but not 3.4). Python 3 introduced changes that will break some of the code we teach during the workshop.

We will teach Python using the IPython notebook, a programming environment that runs in a web browser. For this to work you will need a reasonably up-to-date browser. The current versions of the Chrome, Safari and Firefox browsers are all supported (some older browsers, including Internet Explorer version 9 and below, are not).

**Windows:** Download and install Anaconda.
Download the default Python 2 installer (do not follow the link to version 3). Use all of the defaults for installation except make sure to check Make Anaconda the default Python.

**Mac OS X:** Download and install Anaconda.
Download the default Python 2 installer (do not follow the link to version 3). Use all of the defaults for installation.

**Linux:** We recommend the all-in-one scientific Python installer Anaconda.

