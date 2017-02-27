---
title: Application Development
---

# Running iHart Applications

First, run the iHart software. A full explanation of the software can be found [here](/software).

To run an application, navigate to the folder containing that application\'s files, 
then run the `.swf` file (OSX or Windows), or the `.app` file (OSX). 
The current applications are in \"client -> apps\".

If the applications won\'t run, you\'re getting errors, or it seems like the application isn\'t
sensing motion, try the [troubleshooting suggestions here](/applications/issues).

# Creating iHart Applications

This guide is also available with images as a [PDF file](Making_Your_Own_iHart_Application.pdf). For sample code checkout the HelloWorld application available on [GitHub](https://github.com/ihart-mhc/ihart/tree/master/client/sample/HelloWorld).

*Note*: throughout this demo, \"DemoApplication\" will be whatever name you 
gave your `.as` and `.fla` files.

<!--To create an iHart program proceed as you would when creating a Flash game or interactive scene. 
Prepare your program by creating an interactive scene or game in flash and saving it in .fla file, 
any action script code needed to make the scene functional should be saved in .as file. 
Just as you would make your game or scene interactive by adding mouse event listeners, 
you will now do the same be creating cvEvent listeners.

First, you will need to import the appropriate iHart libraries along with any other libraries you might need:

    import flash.events.*;
    import ihart.event.*;

Next, you will have to set up some global variables:

    private var hostName:String = "localhost"; //the host name will generally be localhost
    
    //if you are intending to display your application in the hallway the host name will have to be initialized like this:
    //private var hostName:String = "192.168.10.1";

    private var port:uint = 5204; // the port number is 5204

    private var cvManager : CVManager; //cvManager will handle our CVEvents

In your constructor initialize your cvManager and add an event listener to it:

    cvManager = new CVManager(hostName, port);
    cvManager.addEventListener(CVEvent.SHELL, getData);


Now you are done with the initial set up and in you getData function you can write code that will save the data about the users movements
and react to them appropriately. To find out more about the obtained data about the users movement, refer to the cvServer documentation.
If you are having trouble compiling your program, go to the Troubleshooting page to see some of the most common errors and how to deal
with them.-->

## Requirements

* iHart installed on computer (Windows or Mac) 
* Adobe Flash CS6 
* webcam
* speakers (for iHart sounds)
* decent-sized display (for iHart display)
* projector aimed at wall or floor (for iHart projection)

## Setup
Open Adobe Flash CS6.
Create a new `.fla` file by clicking on ActionScript 3.0 under \"Create New\". 
Create a new `.as` file by clicking **File -> New -> ActionScript 3.0 Class** (under Type).
Name your `.as` file whatever you want your application to be called and name your 
`.fla` file with the same name.
For our demo we have named the files `DemoApplication.fla` and `DemoApplication.as`.

### Setup for `.as` file
Now go to your `.as` file in Flash, you will see that Flash has already created some 
things for you. In order for stuff to work you will need to add some stuff to your code 
yourself:

#### Importing Libraries
Copy and paste these import statements:

	import flash.geom.Vector3D;
	import flash.events.*;
	import flash.display.MovieClip;
	import flash.display.Sprite;
	import ihart.event.*;
	
Place them in your code after \"package\" and above the class declaration. 
Next add \"extends Sprite\" after the class declaration and before the opening brace.

#### Defining global variables and constructor
Next we will be adding certain \"terms\" or variables that need to be defined in all 
iHart applications for the code to work.

Copy and paste the global variables below:

	private var hostName:String = "localhost"; //the host name will generally be localhost
	private var port:uint = 5204; // the port number is 5204
	private var cvManager : CVManager; //cvManager will handle our CVEvents

Add them to your code after \"public class DemoApplication { \". 

Copy and paste these lines of code to add to your constructor:

	cvManager = new CVManager(hostName, port);
	cvManager.addEventListener(CVEvent.SHELL, getData);

This will initialize your cvManager and add an event listener to it (all necessary for 
iHart to work!) Add them to your code in your after \"public function DemoApplication() { \" (your 
constructor).

Now notice that we will be defining the function getData to determine what happens 
with user movements. In other words this function will save the data of users\' 
movements and react to them however you would like. 

So you need to declare the getData function like this:

	public function getData (e : CVEvent) : void { } 

Add it after the \"public function DemoApplication()\" last bracket \" } \".

### Setup for `.fla` file

When creating your application think about what kind of background you want for 
your application. You also want to choose graphics that will appear as the user 
interacts with your iHart application. Choose some images save them in the same 
folder that your `.fla` and `.as` are located in!

Choose an image for your `.fla` file. 
To import the image onto the Scene go to **File -> Import -> Import to stage** 
and then select the image you saved. It should appear on top of the white background.

To create your graphic in your .fla file go to **Insert -> New Symbol**. 
Name your symbol anything you like, but remember that you will be referring to in 
your `.as` file.
Choose MovieClip as the type of symbol and name your symbol.

Once you have created the symbol import your image graphic by going to
**File -> Import -> Import to Stage**. The new symbol and background image 
should appear under the Library tab on the right in your `.fla` file.

Be sure to edit AS Linkage by double clicking under \"AS Linkage\" beside the 
symbol and change that to the same name as the Symbol.

### Linking the graphics to the code in your `.as` file

Back to `.as` file: under your global variables you will be creating another variable to 
link your graphics to your code. 
Create a new instance of your symbol by inserting the following line of code in the 
list of variables:

	private var variable_name: Vector.<Symbol_Name> = new Vector.<Symbol_Name>();
	
Replace variable name with any name you want, but be aware that you will be using it 
again later in the code.

Add these two lines of code to your variables (after \"public class DemoApplication 
extends Sprite\"):

	//the maximum x and y offset of the symbols
	private var yOffsetMax = 50;
	private var xOffsetMax = 50;
	
### Defining the `getData` function

For this function you will be renaming certain variables for your own purposes
Be aware that you have to rename some of the code for the purposes of 
your specific application.

1. For the variable \"currentFlower\", come up with your own name (e.g. \"currentBall\", 
\"currentTriangle\", \"currentHeart\", etc.) for a new variable that will hold your 
Symbol.
2. Make this variable the same type as your Symbol (so whatever name you 
gave your Symbol in the `.fla` file).
3. Add the Symbol to your variable by using `currentName = new SymbolName();` 
using your own variable and Symbol names.
4. Replace \"currentFlower\" with whichever name 
you have for your variable .
5. It is also important to update the comments so that they reflect your specific 
application. All the lines that come after double slashes (`//`) or after a slash 
and two asterisks (`/**`) are comments.

The code is as follows:


	/**
	 * Gets data about a CVEvent and creates flowers based on that data.
	 */
	public function getData (e : CVEvent) : void {
		trace( "Acquiring data... " );
		var numBlobs : int = e.getNumBlobs();
		var blobX : Number;
		var blobY : Number;
		var rot : Number;
		var currentFlower : Bud;

		//for every blob there is on the screen
		for (var i : int = 0; i < numBlobs; i++) {
			currentFlower = new Bud();
			//save the blob's x and y values
			blobX = e.getX(i);
			blobY = e.getY(i);
			//add a random rotation to the flower (between 0 and 60degrees)
			rot = Math.random() * 60;
			currentFlower.rotation = rot;
			//add the new flower to the scene
			addChild(currentFlower);
			//generate the new flower's x and y based on the blob's x and y
			currentFlower.x = generateOffset(blobX, xOffsetMax);
			currentFlower.y = generateOffset(blobY, yOffsetMax);
			//add the new flower to the vector
			flowers.push(currentFlower);
			//remove any old flowers
			removeOld();
		}
	}

### Defining Additional Functions

In `getData()` we made calls or references to two other functions that we have not 
defined yet. 
`removeOld();` removes flowers that have disappeared from view from the screen and 
the vector.
`generateOffSet();` generates a random offset for a flower\'s x or y value.
In order to define these functions we must add these function declarations: 

	public function removeOld () : void { }
	
and

	public function generateOffset (initialNum: Number, maxOffset: Number): Number { }
	

For `removeOld()` insert the following code in between the brackets \"{ }\".
Again you must edit the code slightly to fit your application.
Replace \"flower\" with the name you chose for the variable declaration you 
made earlier in your code (reminder: 
`private var variable_name: Vector.<Symbol_Name> = new Vector.<Symbol_Name>();` ).

	//for every flower in the vector
	for (var i : int = 0; i < flowers.length; i++) {
		//if the reference portion of the flower is not visible
		if (flowers[i].currentFrame == 20) {
			//remove the flower from the screen
			removeChild(flowers[i]); //remove the flower from the vector
			flowers.splice(i, 1);
		}
	}


For `generateOffSet()` insert the following code in between the brackets \"{ }\".

	//set the amount of offset randomly
	var offset : Number = Math.random() * maxOffset;
	//set the sign of the offset (0 is negative, 1 is positive)
	var sign : int = Math.floor(Math.random() * 2);
	if (sign == 0) {
		return initialNum - offset;
	}
	return initialNum + offset;

### You have finished coding your iHart application!

Now just review and make sure you have everything (*Note*: these are modeled after 
the demo application so not everything will be the same):

* make sure your class extends Sprite
* import statements 
* variable declarations
* lines of code in the constructor instantiating and adding the `cvManager` that calls 
`getData()`
* declaration and definition of function `getData()`
* declaration and definition of function `removeOld()`
* declaration and definition of function `generateOffSet()`

### Publish Settings
Finally in your `.fla` file you will need to change the **Publish Settings** for your Flash 
application to use iHart. These steps are also documented under the [issues page](/applications/issues).

**File -> Publish Settings**: Under **Local playback security** change it to **Access network only**.

Then on the top of the Publish Settings page look for a wrench-shaped and click it.
Then click the icon that looks like a folder.

A browsing window will show up and from here you should navigate to the folder 
holding your iHart Application then **Applications -> flash -> ihart**.
Stop here and click Choose.

Still in the Publish Settings page choose **Win projector** or 
**Mac projector** (depending on which system you are 
working on) from the Publish menu on the left side. This 
should create a `.app` file (at least on OSX) with your application name.

Now your application should be ready to use! Run iHart and then click on the `.swf` or `.app` 
file of your application to see your creation working!
