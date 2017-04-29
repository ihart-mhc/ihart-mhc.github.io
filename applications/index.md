---
title: Applications
---

# Application Development
* [Running iHart Applications](#running-ihart-applications)
* [Current Applications](#examples-of-current-ihart-applications)
* [Application Development](development)
* [Source Code](https://github.com/ihart-mhc/ihart)

# Running iHart Applications

First, run the iHart software. A full explanation of the software can be found [here](/software).

To run an application, navigate to the folder containing that application\'s files.
The current applications are in \"client -> apps\".

**Flash applications:**

From the application\'s folder, run the `.swf` file (OSX or Windows), or the `.app` file (OSX).

*Note: Flash iHart applications will only run with server versions <= 2.3, as the Flash client library is being deprecated.*


**Java applications:**

See the [Java application development guide](/applications/development/java/) section on running Java applications.

If the applications won\'t run, you\'re getting errors, or it seems like the application isn\'t
sensing motion, try the [troubleshooting suggestions here](/applications/issues).

**Python applications:**

From the application\'s folder, run the main Python file as `python ___.py`. Depending on the application, there may be extra libraries to install,
or an exported `.app` or `.exe` file that can be run.

## Examples of Current iHart Applications
Bellow is a partial list of iHart applications. All of these applications (including source code) can be downloaded from [GitHub](https://github.com/ihart-mhc/ihart).

### Butterfly
Butterflies follow the motion of the user and cluster together.
![Screenshot of the Butterfly Application](img/butterflies.png)

### Flowers
Flowers grow around the areas where movement is detected.
![Screenshot of the Flowers Application](img/flowers.png)

### Mario Jump
The user can play as Mario.
![Screenshot of the MarioJump Application](img/marioJump.png)

### Snow Scene
The snowflakes fall around the areas outlined by user\'s motion.
![Screenshot of the Snows Scene Application](img/snowScene.png)

### Piano
The user can play the piano by creating motion in front of the proper keys.
![Screenshot of the Piano Application](img/piano.png)

### Fireworks
Fireworks explode around the areas where movement is detected.
![Screenshot of the Fireworks Application](img/fireworks.png)

