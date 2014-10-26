---
title: Software issues
---

# Troubleshooting the iHart software

### Force quit the camera
Some versions of iHart don\'t stop the camera window when the program is exiting.
This problem has been fixed in a version of iHart that will be released soon. 
For now, use the Activity Monitor or the Task Manager to quit the \"cvServer\" process.

### Help windows do not show up
When sliding the \'help\' trackbar to the right, an empty window might appear instead of a window with help information.
We are working on this; in the meantime, the same help information can be found [here](/software).

### The server can\'t find the camera
Sometimes, the iHart software won\'t be able to detect a camera when starting the main program, 
and the program won\'t run properly. Try restarting your computer.

### iHart won\'t detect faces
There\'s an issue that arises when the application is exported that leads to the exclusion of a file iHart needs to detect faces.
If your application requires face detection to work, try running iHart from the source.


# Other Issues
There may be other issues not listed here that appear on [the GitHub issue queue](https://github.com/ihart-mhc/ihart/issues).


If you have been using iHart and come upon an error not listed here or on GitHub, 
please make use of [the GitHub issue queue on the public iHart repository](https://github.com/ihart-mhc/ihart/issues)
to notify the maintainers of iHart.