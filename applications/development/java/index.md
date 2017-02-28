---
title: Java Application Development
---

# Creating an iHart Application (Java)

*Contents:*

* [Setup](#setup)
* [Development](#development-using-the-ihart-java-client-library)

The full exported Javadocs for the iHart Java client library are available [here](doc).

# Setup
First, make sure you have downloaded the latest version of the iHart project from [this website](/ihart)
(or [cloned the repository](https://github.com/ihart-mhc/ihart) from GitHub).
You will need to import the `ihart` package and its contents, in order to use classes like `ihart.CVEvent`.


## Eclipse

In order for Eclipse to compile and run your project, it needs to know about the iHart client library and its dependencies.
To link the `.jar` files included with iHart:

1. Open your project in Eclipse. (If you have not already created a Java project, go ahead and do so now.)
1. Under the \"Project\" menu, click on \"Properties\". A popup window should appear.
1. Click on \"Java Build Path\" from the menu on the left.
1. Click on the \"Libraries\" tab on the top.
1. Click on \"Add External JARs\" on the right.
1. Navigate to where you downloaded the iHart project, and then to the \"client/library/java\" folder.
1. Add both the `ihart-library.jar` and `javax.json.jar` files. They are both required for the project to compile and run.
1. Hit \"OK\" at the bottom right.

At this point, you should be able to import the ihart package and its contents.


## Command Line

If you\'re planning on compiling your project from the Terminal/Command Prompt,
the iHart client library and its dependencies need to be available.
Whenever you compile or run your program, you will need to include the necessary `.jar` files in the classpath.

*Note: if you are unfamiliar with compiling and running Java programs from the command line, Eclipse may be a better option.*


### Compile your project

To compile your project, run the following where your source `.java` files are located,
and replace \"\<path to ihart project\>\" with the relative path to where you placed the iHart library.


	javac -cp "<path to ihart project>/client/library/java/*" *.java

For example, to compile the PrintlnProgram example application in \"ihart/client/sample/PrintlnProgram\",
you would first change directories to that folder, and then run:

	javac -cp "../../library/java/*" PrintlnProgram.java


### Run your project

To run your project, run the following where your compiled \".class\" files are located, and again
replace \"\<path to ihart project\>\" with the relative path to where you placed the iHart library.
You should also replace \"\<YourMainClass\>\" with the class containing your `main` method.

	java -cp ".:<path to ihart project>/client/library/java/*" <YourMainClass>

If you are on Windows rather than OS X, you may have to substitute a colon (`:`) for the semicolon (`;`), as below:

	java -cp ".;<path to ihart project>/client/library/java/*" <YourMainClass>

Note that the `.` specifies that Java should refer to your application as well as the iHart libraries when running.

For example, to run the PrintlnProgram example application in \"ihart/client/sample/PrintlnProgram\" on Windows,
you would first change directories to that folder, compile the project as stated above, and then run:

	java -cp ".;../../library/java/*" PrintlnProgram

For more details on the classpath option, see [the official Java documentation](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/classpath.html)
and [this Stack Overflow answer](http://stackoverflow.com/questions/219585/setting-multiple-jars-in-java-classpath).


# Development Using the iHart Java Client Library

The iHart client library is set up with a class that manages connecting to the server and receiving messages,
and a set of classes that allow you to receive and process events. Similarly to Java\'s [ActionListener interface](https://docs.oracle.com/javase/tutorial/uiswing/events/actionlistener.html),
the library provides a `CVEventListener` interface with three methods:

* `public void shellsArrived(CVEvent shellEvent)`
* `public void facesArrived(CVEvent faceEvent)`
* `public void blobsArrived(CVEvent blobEvent)`

Any class that implements the `CVEventListener` interface must implement all of these methods (though the method body may be empty for one or more of them).


## Parsing `CVEvent`s

`CVEvent`s are fired whenever information is received from the server about motion (aka \"shells\") or faces were detected.
A generic \"all blobs\" event will also be dispatched whenever faces, shells, or both were detected.
Every event has a type and a list of blobs associated with it, and you can get the number of blobs from the event object as well.

`CVEvent`s also refer to \"regions of interest\" or \"areas of interest,\" which you can read more about [here](/software).
All blobs occur within the bounds of a single region of interest.
From the `CVEvent` object, you can choose to `getAllBlobs()` regardless of what region of interest they occurred in,
or `getBlobsInRegion(int regionOfInterest)` to get only the blobs that occurred within the specified region.

Both methods return `List<Blob>`; you can iterate over this list to get access to each of the `Blob` objects.
`Blob`s represent either a face or a shell that was detected by the server.
Each blob has an  x coordinate and a y coordinate (representing the top left corner of the blob), a width, a height, a type (face or shell),
and the index of the region of interest it occurred in.

*Note: the x, y, width, and height are stored and returned as `double`s, in order to be as precise as possible.
Many `java.swing` classes and methods require ints as parameters, so you may need to cast these values before using them for graphics.*


## Connecting to the Server

Once you have a class that implements `CVEventListener`, you can start receiving events from the server. There are a few steps involved:

1. Run the server application included in the project in \"server/dist/...\" (choose the Mac or Windows distribution depending on your operating system).
On OS X, you\'ll want the `cvServer.app` file; on Windows, you should run the `cvServer.exe` file.
1. In your code, start the connection to the server by creating a `CVManager` object.
This object needs to know where the server is running; usually, that will be on the same computer (\"localhost\" is the hostname) with the default port (5204).
When the CVManager is created, it will attempt to connect to the server.

	```
	CVManager cvManager = new CVManager(“localhost”, 5204);
	```

1. Next, let the `CVManager` know that your object wants to receive `CVEvent`s:

	```
	cvManager.addCVEventListener(myClass);
	```

	Where `myClass` is an instance of the class that implements `CVEventListener`.


If your application exits with a message about being unable to connect to the server,
make sure you are running the server where the `CVManager` expects it to be.