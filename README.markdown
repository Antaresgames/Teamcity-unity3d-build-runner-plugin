Teamcity Unity3d Build Runner plugin
====================================

Overview
--------

[Teamcity](http://www.jetbrains.com/teamcity/) is a continuous integration server. Unity3d is a powerful 3d game creation editor and engine.
If you want to easily build [Unity3d](http://unity3d.com/) players as part of a Teamcity installation, you can invoke them
from a Shell runner, but editing parameters and viewing log files is a bit of a pest.

This plugin makes life a bit easier and provides a nice cross-platform way of building Unity3d projects
on the Mac or Windows platforms. (Note that the Unity3d build usually does not output any build progress to standard output
but instead outputs to a log file, whose location is platform-specific)


Using
=====

Installation
------------

### Prebuilt version

You can download a prebuilt version against Teamcity 8.0.3 here: [prebuilt unity runner.zip, 1.5MB](http://tech.mindcandy.com/wp-content/uploads/2013/08/unityRunner.zip)
It _may_ work with other versions of Teamcity (unless there are breaking API changes).

### Compilation

When building the plugin, you need to have downloaded the Teamcity distribution you want to build against, so that the compilation process can grab the necessary libs. It's also useful so that you can test that it will load the plugin correctly.

When running Teamcity locally, any plugins need to be installed by putting them in the .BuildServer folder. On Nix and OSX this defaults to */home/<user>/.BuildServer*.

Before compiling, it's important to update the build.properties file with the paths the Teamcity distribution and to the .BuildServer folder. When the process is complete, ant should automatically copy the unityRunner.zip file to the .BuildServer folder.

#### Ant CLI

To compile with ant directly, just run *ant dist* from the root folder of the repo.

#### IntelliJ

Compile with [IntelliJ IDEA](http://www.jetbrains.com/idea/) -- the Community Edition (free) version will work!

Open up the unity_runner.ipr file, then open the Ant Build window and add build.xml (if not automatically detected).

Double click on 'dist' target to do a full build, which should produce dist/unityRunner.zip

### Installing in Teamcity

Copy the unityRunner.zip file to the .BuildServer/plugins/ folder in your Teamcity installation, then restart Teamcity.


Usage
-----

'Unity' will be available as a Build Step. It will automatically detect if Unity appears to be installed on an Agent, 
by looking in the default install location for each platform (Program Files or Applications).

### Options

Many of these relate to [Unity3d command line arguments](http://unity3d.com/support/documentation/Manual/Command%20Line%20Arguments.html)

* Batch Mode - should be left enabled usually, enables the Unity -batchmode
* No graphics - on Windows only, do not initialize a graphics device during a build to avoid errors when running without a good GPU. Equivalent to -nographics command line option
* Project path - specifies the path (relative to Working directory) of Unity project to open
* Build Player - choose which Unity Player to build, currently supported are Web, Windows or OSX (but it would be easy to add others if required). Equivalent to passing -buildWebPlayer -buildWindowsPlayer or -buildOSXPlayer on command line
* Execute method - specify a method for Unity to execute, to allow you to customise your build process. Equivalent to -executeMethod on the command line
* Build path - specify the output build path for the Player
* Clear output before - ensures output folder exists and is empty, before invoking Unity
* Clean output after - removes any .svn and .meta files found in the output directory, as these are usually not wanted
* Quit - specify if unity should quit after a build. Usually should be left enabled, equivalent to the -quit command line option


### Features

* Automatically locates the Unity3d log file and sends it to Teamcity whilst the build is in progress, giving feedback 
during the build and preventing spurious 'build is hanging' warnings from Teamcity.
* Automatically detects if Unity3d is installed on an Agent, so prevents attempts to run if Unity3d is not installed.
* Processes log file to detect and flag up warnings and errors to Teamcity, making log files easier to read
* Detects compilation, asset refresh and Player statistics blocks in the log file


Contributing
============

Please do! Simply fork the github project as usual.

In particular in the future you may want to

* add support for more log messages
* build different Players
* invoke test suites

