# Spatial_SQL
A introductory workshop on Spatial SQL using the QGIS DB Manager.  This workshop was written for QGIS verison 2.18 or 3.0.


# Concepts

## What is a database?
A database is a set of data in tables that are related to each other in some way.  That's it.  It's just a collection of tables.

Generally each table can be connected to another table by a column that both tables have that store the information to match up the rows.  This column is called a **key**.  A key commonly used on campus is your student or employee ID number.

You already use a database if you've done GIS.  A shapefile is essentially just a fancy a table.

## What is a Spatial Database?
A spatial database is just a normal database with a column that holds the geometry information.  The geometry/geography information is sored as a Binary Large Object (BLOB).

## What is Spatial SQL?
SQL stands for "structured query language" and it's a language that allows you to ask questions of a database.  Spatial SQL is regular SQL but with some additional functions that perform spatial analysis.

If you've ever written an attribute query in ArcGIS or QGIS, you've worked with SQL.  Example: Hey GIS program, please highlight all the records in my attribute table that have "Yolo" in the "county" column!  In SQL, we would write ```SELECT * FROM county_shapefile WHERE county = 'Yolo'; ```  It's actually quicker to write that query than to fill out the interface in the GIS.

## Why do you want to learn spatial databases and spatial SQL?
* It's a good way to work with large amounts of data
* Faster processing of data compared with working through a graphical user interface.
* Typically faster to run a process in a spatial database than in a GIS program
* Store lots of data (compare with shapefile's 70m row limit)
* One database file stores many, many datasets --> easier data management
* Write a query instead of making a new file (no exporting of results to shapefile necessary!)

## What makes this hard?
If you're a GIS user, you're probably used to a graphical user interface (GUI) where you can see your data, have tools with guided interfaces, and can see the results of your processing immediately.

These aren't things you get with a typical database, however, we can connect our database to QGIS so we can see our results and with practice, you will get used to the typical workflow and seeing everything won't be so necessary.

## Databases that support Spatial SQL:
* Oracle
* MySQL
* SQL Server
* Spatial Lite
* PostGRES --> PostGIS
* Others too!

## Concepts:
The spatial data can be accessed in two ways.  One is Geography, and the other is Geometry.

1. Geography != Geometry
    
    i. Geography is QGS84
    i. Geography has limited spatial functions
    i. Geography is 2D
    i. Geography is faster for generating visualizations
    i. Geometry is for full analysis
    i. Geometry can be 3D

---------------------------------------------
# Hands-On Tutorial

## Data



## Installation
**Install QGIS:** You should have installed QGIS version 2.18 or 3.0 from [QGIS.org](https://qgis.org/en/site/forusers/download.html).  At the time of writing, 3.0 was just released, so I would recommend 2.18 for now unless you are adventurous or fairly familiar with QGIS already.

## Getting Started
Open QGIS. Start a new project by either clicking on the white rectangle icon (with the corner turned down) or selecting "new" under the Project menu. You now should have a blank map canvas ready to go.

We'll be using the DB Manager plugin, so let's make sure that is installed:

1. Click the Database menu at the top of the QGIS window.  Do you have the DB Manager listed as an option in this menu?  If so, click the icon to open the tool and skip Step 2 below.  If not, move on to Step 2 below.
1. If the DB Manager is not in the Database menu, we need to enable the plug in:  

    i. Click the Plugins menu at the top of the window and select "Manage and install plugins".  This will open the Plugins Manager.  
    ii. Make sure you're in the "All" tab on the left side of the tool, and then in the search box, start typing "DB Manager" to narrow down the options.  
    iii. Select "DB Manager" from the list to view the details about this plugin.  
    iv. Click the "Install Plugin" button in the lower right corner to install it.  If this option isn't available, the tool is probably already installed and you just need to make sure the box next to the tool in the list is checked so that is is available.


## Make a Database



## Tour of the interface

## Load some data

## Anatomy of a query = what do they typically look like, caps/no caps, end with ;

## Some non-spatial Queries:
Start simple: 
``` SELECT * FROM table; ```

Add a WHERE clause: 
``` SELECT * FROM table WHERE column = 'something'; ``` 

Add a function: 
``` SELECT COUNT(column) FROM table WHERE column = 'something' ``` 

## Spatial Query Examples:
Let's look at the geometries: ``` SELECT ST_AsText(geom) FROM table;``` 

Getting the lengths of the lines, this doesn't work: ``` SELECT ST_Length(geom), strname FROM street_centerlines;``` 
But this does: ``` SELECT ST_Length(ST_TRANSFORM(geom)), strname FROM street_centerlines;``` 

Get the area of polygons: ``` SELECT objectid, ST_AREA(ST_TRANSFORM(geom)) FROM buildings;``` 

## Projections:
SELECT UpdateGeometrySRID('db_table', 'geometry_field', EPSG);

## Spatial Analysis:




# Additional Resources:

## General Slides & Tutorials:
[Todd Barr's Slides](https://www.slideshare.net/gis_todd/foss4g-2017-spatial-sql-for-rookies)

[Mike Miller's Tutorial](http://millermountain.com/geospatialblog/2017/10/23/qgis-and-spatialite/)

## Geocoding with PostGIS
[How to make a PostGIS geocoder in less than 5 days](https://experimentalcraft.wordpress.com/2017/11/01/how-to-make-a-postgis-tiger-geocoder-in-less-than-5-days/)... yours would only take 4 on your own since your PostGIS is already installed.

[Batch geocoding with PostGIS](https://experimentalcraft.wordpress.com/2017/12/21/batch-geocoding-with-postgis/)

*****Stoped at slide 56 in https://www.slideshare.net/gis_todd/foss4g-2017-spatial-sql-for-rookies









