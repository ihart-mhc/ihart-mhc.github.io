---
title: Autostart the Server
---

# Automatically starting iHart

Note: this guide assumes some familiarity with the project.
Check out [the software page](/software) and [the applications page](/applications) for general overviews.


If you\'re setting up iHart for a long term display, you can use the provided autostart script to reduce maintainence requirements.
The script, `autostart.sh`, and its associated `autostart.config` configuration file, live in the project\'s `bin/` folder.



## Setting preferences

The [`autostart.config` file](https://github.com/ihart-mhc/ihart/blob/master/bin/autostart.config) allows you to set preferences without
modifying the script itself. Each variable is detailed below.

 - `OS_OSX` should be set to `true` if you are running on an OS X operationg system; if you are running on Windows, set this to `false`.
This is used to determine which version of the server should be run.

 - `SERVERWAIT` defines a number of seconds to wait between when the server is started and when the app is started.
 This allows the server to finish setting up before the client attempts to connect. It is recommended that this be at least `5`.
 If you have trouble where it seems like the client is not connecting to the server, you may want to increase this value.

 - `APPRDIR` specifies the location of the `client/apps` directory, relative to where the script and configuration file are located.
 By default, since the script is in `bin/`, this variable is set to `../client/apps/`. If you move the script and configuration file,
 you will need to change this variable to the correct location.

 - `APPFILE` selects which application should be run; more specifically, which exact file to run. On OS X, you should select an `.app` file
 if one is available; simiarly, `.exe` is best for Windows.



## Configuring the script

While you should not need to make any edits to the `autostart.sh` script, there are a few steps required to set up your computer so that
the script runs when the computer boots up. We suggest downloading the iHart project to a guest account that has few permissions in order
to facillitate a secure setup.

*Note: These instructions below are currently only applicable to OS X.*

1. Make sure the `autostart.sh` opens using the Terminal application by default. Navigate to the `bin/` folder, then right-click and select
\"Get Info\", and under \"Open with\" choose \"Terminal\". (Full instructions [here](http://www.imore.com/how-set-mac-app-default-when-opening-file).)

2. Set up a user that will automatically log in when the computer starts up. As an adminstrator, open \"System Preferences\", select \"Users and Groups\", and
click on the lock icon in order to make changes. Click \"Login options\", then \"Automatic login\", and choose an account that has access to the iHart project.
(Full instructions [here](https://support.apple.com/en-us/HT201476).)

3. Set the script to run automatically upon login. Open \"System Preferences\", select \"Users and Groups\", then the user you set up in the previous
step. Click \"Login items\", and use the \"+\" to add the `autostart.sh` script as a login item. (Full instructions [here](https://support.apple.com/kb/PH18881?locale=en_US).)

You\'re done! If you restart your computer you can check that your setup is functioning correctly.

If you\'re working with multiple displays, you can optionally [assign applications to a particular display/space](https://support.apple.com/kb/PH21872?locale=en_US).