# Spatial_SQL
A introductory workshop on Spatial SQL using PostGIS


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

Possible sources:
[Mother Jones' US Mass Shooting Dataset](https://www.motherjones.com/politics/2012/12/mass-shootings-mother-jones-full-data/)

## Installation
Installing postgreSQL and PostGIS is a little tricky, but you only have to do it once.  The order of installation is important. 

Download & install these programs in this order:

1. postgreSQL 10: https://www.postgresql.org/download/   = the database program that’s going to make all of this work

    i. The installer should walk you through installation options.  
    ii. Write down the port (keeping the default is fine) and password you choose.  
    iii. Pick a password you like to type because it comes up a lot.

2. PostGIS: https://postgis.net/install/  (windows latest bundle: http://download.osgeo.org/postgis/windows/pg10/)
   
    i. PostGIS makes postgreSQL have spatial capabilities (much like a super power) and install the bundle option because it comes with things you’ll need, saving a few steps
    
    ii. Make sure you’ve installed postgreSQL first
    
    iii. The installer should walk you through the installation options.

## Make a Database
We need to make an empty database to put our files into.

1. Open PGAdmin 4.  This is the graphical user interface for postgreSQL that should have installed with the postgreSQL files. If PGAdmin 4 doesn’t start or gives an error, you may need to start the service that makes it run.  In the Windows search bar, search “services”.  In the Services window, right click postgresql-x64-10 and choose “start”.

1. In the browser panel on the left side of the pgAdmin 4 window, click the + to expand Servers and PostgreSQL 10.  When it asks for your password, give it the password you wrote down during the installation process. (If it gives you an error, start the service... see above.)

1. Right click on PostgreSQL 10 -> Create -> Database

1. In the Database name field, give it a name with lowercase letters.  I named mine maptime.  Leave the owner drop-down on postgres.  Click Save.

1. Expand the Databases entry in the Browser panel to see your new database in the list.

## Add Data


## Enable Extensions
Right now, our database is just a regular database without spatial functions.  We need to enable the PostGIS extension and the extensions that help us geocode.

1. Open the Query Tool: in the Tools menu at the top of the screen, select “Query Tool”.  The top section of the query tool is where you type SQL commands.  Underneath this is where the results of your queries are displayed.

1. Enable the PostGIS extension to give the database spatial capabilities by copying and pasting this code into the query tool and clicking the run button (it looks like a lightning bolt… why?  I don’t know.):

```
CREATE EXTENSION postgis;
```

I know this doesn't make much sense to you right now, but that's ok.  We just need to get the database tool running, and then we can start learning.


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









