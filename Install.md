# Installation instructions for Spatialite GUI (Graphical User Interface)

## Windows

  * Download and install the [7-zip archive program](https://www.7-zip.org/) 64 bit should work for most people. 7-zip is a more sophisticated tool for zipping and unzipping files than the standard Windows tool.
  * Download the [Spatialite Installer](http://www.gaia-gis.it/gaia-sins/windows-bin-amd64/) file called `spatialite-tools-5.0.1-win-amd64.7z`
  * Download the [Spatialite GUI](https://ucdavis.box.com/s/4vgb1uq6v6bxw94blqoo7ubfv7lvtu4n) file called `spatialite_gui.zip`
  * Use 7-zip to extract the files where you want them by right clicking on the file name in your browser. The unzipped folders should be at the same level in your file browser (i.e. these two folders should be inside the same folder). For Windows 11, you may need to choose "Show more options" first. Select "7-Zip" then "Extract files...", and then make your selections about where to put the files that will be extracted.
  * Click the spatialite_gui.exe to use

  
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
