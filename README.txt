==========================================================================
		######                              
		#     #  ####   ####  #    # ###### 
		#     # #    # #    # #    # #      
		######  #    # #      #    # #####  
		#   #   #    # #  ### #    # #      
		#    #  #    # #    # #    # #      
		#     #  ####   ####   ####  ###### 
						    
		######                                     
		#     # ###### #####   ####  #####  #    # 
		#     # #      #    # #    # #    # ##   # 
		######  #####  #####  #    # #    # # #  # 
		#   #   #      #    # #    # #####  #  # # 
		#    #  #      #    # #    # #   #  #   ## 
		#     # ###### #####   ####  #    # #    # 
==========================================================================
  A cross-platorm nostalgic port of the clasic "Rainbow Six: Rogue Spear"
       
		
CONTENTS OF THIS FILE
---------------------
 * Introduction
 * Dependencies 
 * Building
 * Installation
   * Dedicated Server
 * Design Decisions


INTRODUCTION
------------
Rogue Reborn is an open-source cross-platform first-person-shooter. 
It is a port of the classic game by Red Storm Entertainment: "Rainbow
Six: Rogue Spear". The original engine was id Tech 3, aka "The Quake 3
Engine". It was later improved by the community, upon its open-source
release, and became ioQuake3. A team of developers, focused on the renderer,
forked ioQuake3 and developed xReaL. The xReaL project was put on hiatus
after ETCW was open-sourced. 

Rogue Reborn's engine is called Hat and is derived from the team that
developed xReaL.



DEPENDENCIES
============
Rogue Reborn dependencies are platform-specific.

Windows:
	OpenAL - http://goo.gl/0t2P
	V8 - V8 is included for Windows, but you need 7zip (http://goo.gl/E4wo)
	     to unpack the file. Unpack it from:
	     ./engine/dep/v8/lib/v8.7z to ./engine/dep/v8/lib/v8.lib.
	     "Why .7z? Because 7mb vs 2mb is a big difference in terms of
	     a repository. Plus, the lzma is nice."

Mac OS X 10.6:
	OpenAL - It's already natively on your system.
	V8 - You will need to build it. I recommend `brew install v8`
	SDL - You will need to build it. I recommend `brew install sdl`
	CURL - Already on your system

	==== Mac OS X 10.6 Specific ===
	Cocoa
	IOKit
	==== MAC OS X 10.6 Specific ===
	

Linux:
	SDL 1.2 - Use your local package manager
	CURL >= 7.15.5 - Use your local package manager.
	OpenAL - http://goo.gl/TWaz
	V8 - Either use your local package manager or get the latest
             stable branch by running svn:
	     "svn checkout http://v8.googlecode.com/svn/trunk/ v8"

Windows uses SDL, CURL, and V8 as well, but they are pre-built binaries
included with Rogue Reborn. These can be found in ./engine/dep/curl,
./engine/dep/sdl, and ./engine/dep/v8.



BUILDING
--------
Rogue Reborn uses CMake for its build system.

CMake is available for free at:
	http://www.cmake.org/cmake/resources/software.html

Here is a quick build-system setup:
	* Create a new directory outside of the sources called "build"
	- If you are under Windows you can run the CMake gui and then
	  select both the source code and location of where to build the
	  binaries. Ignore the next step if you do this.
	* From that directory run "cmake <path to source>"
	* Done.

Now depending on your build system flavor you will be ready to build
Rogue Reborn. If you're using Makefiles, then just do what is typical and
run make.  If you specified Visual Studio as your build system, then 
you will have project files that you can open and build.

There are a few options to the build system:
	HAT_EXPAND_DEPENDENCIES
		- When enabled, you will get an expanded version of
		  all the dependencies in your IDE. This means that you
		  will see:
			(library) Ogg
			(library) Vorbis
		  And so on in your project file filter list. This does
		  not affect the build in terms of speed or what is built.
	HAT_EXPAND_PLATFORM
		- When enabled, you will get an expanded version of
		  any platform-specific files in your IDE. This mean that
		  you will se::
			(win32-specific) Source Files
			(win32-specific) Header Files
		  And so on in your project file filter list. This does
		  not affect the build in terms of speed or what is built.

The following are generated during the build:
	+ engine/[Debug|Release]/engine[.exe]
		- The actual game; run this to play.
	+ engine/[Debug|Release]/dedicated[.exe] 
		- The dedicated server
	+ client/[Debug|Release]/client[.dll|.so]
		- The client-related game-play
	+ server/[Debug|Release]/server/server[.dll|.so]
		- The server-related game-play
	+ gui/[Debug|Release]/gui[.dll|.so]
		- Anything GUI related



INSTALLATION
============
Rogue Reborn requires its content which is separately maintained.

Windows:
The dependency structure should look like this:
	+ Rogue Reborn/
		- libcurl.dll
		- renderer.dll
		- SDL.dll
		- zlib1.dll
		- engine.exe
		- dedicated.exe
		+ base/
			- server.dll
			- client.dll
			- gui.dll

Dedicated Server:
The dedicated server can be run by doing something like:
	./dedicated +set com_hunkMegs 16 +set dedicated 2  +map k2s

DESIGN DECISIONS
================
Rogue Reborn is designed in the idea of a collection of tools. This means
that each sub-project has its own src and include directories. The include
structure is:
	hat/<project>/files

Any dependency that has too many conditionals per-build-system is
kept out of the project. If it can be built with the project, then it will
be included.
