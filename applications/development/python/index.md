---
title: Python Application Development
---

# Creating an iHart Application (Python)

Generated Python documentation (for the client library) is available [here](doc/ihart.html).

*Contents:*

* [Setup](#setup)
* [Development](#development-using-the-ihart-python-client-library)

# Setup

The suggested library for making client applications is [kivy](https://kivy.org/#home),
which you can use to easily create graphical python applications. The sample python application
uses kivy. Kivy must be [installed separately](https://kivy.org/#download).

Another option is [Tkinter](https://wiki.python.org/moin/TkInter), which comes with most versions of python.

## Importing the `ihart` module

First, make sure you have downloaded the latest version of the iHart project from this website (or cloned the repository from GitHub).

The client library is located in the `client/library/python` folder in the iHart project. When setting up your
application, you need to tell python where this folder is located, related to where you\'re working on your application.

First, copy these lines into your file, underneath your other `import` statements:


	# Regular python imports
	import sys, os

	# Import the iHart client library. This path should be the relative path to the
	# client/library/python folder in the ihart project.
	IHART_PATH = "../../library/python/"

	# We normalize the string for this operating system, make it absolute, and append
	# it to the python path. DON'T TOUCH THIS. Edit IHART_PATH above.
	sys.path.append(os.path.abspath(os.path.normpath(IHART_PATH)) + os.sep)
	import ihart

This code does the following things:

1. Imports the `sys` and `os` modules.
2. Defines an `IHART_PATH` variable. This should be the path to the `ihart/client/library/python` folder.

	**You must change `IHART_PATH` to reflect the path to this folder in your setup!**

3. Adds the `IHART_PATH` to the list of places that python will look for the iHart client library.
4. Imports the `ihart` module, which is the iHart client library.

If you have specified the path correctly, you should be able to run your application without any import errors.


# Development Using the iHart Python Client Library

## Connecting to the server

There are two main steps to using the iHart client library in your application. Once you have completed the setup above
and can import the library, the first step is to create a `CVManager` object:

	cvManager = ihart.CVManager()

Upon creation, the `CVManager` will attempt to connect to the iHart server. If the connection fails, the error will be reported.

*Note: If you are not running the server on the same computer, you can optionally pass in `host` and `port` options to specify where the server is.*


## Retrieving events

You can retrieve events from the `CVManager` via its `getNewEvents()` method. When called, the `CVManager` will check for messages from the server,
and return the parsed event data if there are. If not, the method will return `None`.

	# Get any new events from the server since the last time this method was called.
    eventData = self.cvManager.getNewEvents()

Since most GUI libraries in python have their own main loops, it can be difficult to know where to call this method, and implementations may vary by library:

- For kivy, we suggest using the [Clock.schedule_interval](https://kivy.org/docs/tutorials/pong.html#scheduling-functions-on-the-clock) method (this is what the
sample python application uses). For example, if you have a `processIHartEvents` method:

	```
	Clock.schedule_interval(processIHartEvents, 0)
	```

- For Tkinter, you may wish to use the [after](http://stackoverflow.com/a/459131) method. You can also
[define your own main loop and call update](http://stackoverflow.com/a/4836121).

In all cases, you should make the interval between calls as short as possible, so that events don\'t pile up. Both kivy and Tkinter allow setting the
interval between calls to your scheduled method to 0.

## Parsing events

Events are returned as a `CVEventData` object. Events have `Blob`s associated with them; each one will either be a motion (aka \"shell\") or face type.
Blobs can be retrieved from the `CVEventData` object via the following methods:

- `getAllBlobs()`
- `getAllFaces()`
- `getAllShells()`

`CVEventData` also refers to \"regions of interest\" or \"areas of interest,\" which you can read more about [here](/software).
All blobs occur within the bounds of a single region of interest.
If you\'re only interested in blobs that occurred within a specific region of interest, you can choose to get only those blobs from the `CVEventData` object:

- `getBlobsInRegion(regionOfInterest)`
- `getFacesInRegion(regionOfInterest)`
- `getShellsInRegion(regionOfInterest)`

