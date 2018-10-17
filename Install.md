# Installation instructions for Spatialite GUI (Graphical User Interface)

## Windows

  * Get the [7-zip archive program](https://www.7-zip.org/) 64 bit should work for most people.
  * Install 7-zip (You need this to "unzip a 7z format")
  * Get the [Spatialite Installer](http://www.gaia-gis.it/gaia-sins/windows-bin-x86/spatialite_gui-4.3.0a-win-x86.7z) 
  * Use 7-zip to extract the files where you want them.
  * Click the exe to use
  
  *We've direct linked as it can be very hard to find the pre-made ready to use program.*

## Mac

  * Open a Terminal
  * Install [Homebrew](https://brew.sh/)
  
    ``` /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" ```
  
  * Install Spatialite and Spatialite GUI
  
   ```
       brew install spatialite-tools
       brew install gdal
       brew install spatialite-gui
   ```
   *Instructions here courtesy of Ryan Peek, see the [notes](https://github.com/ryanpeek/spatiallite_demo) for more details*

## Linux

 * If you have a package manager look for spatialite-gui
 * To install from source, get it [here](https://www.gaia-gis.it/fossil/spatialite_gui/index).


**Now Back to the [workshop tutorial](README.md)**
