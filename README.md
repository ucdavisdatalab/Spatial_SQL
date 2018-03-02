# Spatial_SQL
A introductory workshop on Spatial SQL using the QGIS DB Manager.  This workshop was written for QGIS verison 2.18 or 3.0.


# Concepts

## What is a database?
A database is a set of data in tables that are related to each other in some way.  That's it.  It's just a collection of tables.

Generally each table can be connected to another table by a column that both tables have that store the information to match up the rows.  This column is called a **key**.  A key commonly used on campus is your student or employee ID number.

You already use a database if you've done GIS.  A shapefile is essentially just a fancy a table.

## What is a Spatial Database?
A spatial database is just a normal database with a column that holds the geometry information.  The geometry/geography information is sored as a Binary Large Object (BLOB).  The geometry information allows us to relate the tables to each other based on their location and also to perform spatial analysis on our data.

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

## What makes this challenging?
If you're a GIS user, you're probably used to a graphical user interface (GUI) where you can see your data, have tools with guided interfaces, and can see the results of your processing immediately.

These aren't things you get with a typical database manager tool, however, we can connect our database to QGIS so we can see our results and with practice, you will get used to the typical workflow and seeing everything won't be so necessary.

## Databases that support Spatial SQL:
* Oracle
* MySQL
* SQL Server
* Spatial Lite
* PostGRES --> PostGIS
* Others too!

We'll be working with the DB Manager that comes with QGIS,

## Concepts:
The spatial data can be accessed in two ways.  One is Geography, and the other is Geometry.

1. Geography != Geometry

    i. Geography is QGS84
    ii. Geography has limited spatial functions
    iii. Geography is 2D
    iv. Geography is faster for generating visualizations
    v. Geometry is for full analysis
    vi. Geometry can be 3D
    
    

---------------------------------------------
# Hands-On Tutorial

## Installation
**Install QGIS:** You should have installed QGIS version 2.18 or 3.0 from [QGIS.org](https://qgis.org/en/site/forusers/download.html).  At the time of writing, 3.0 was just released, so I would recommend 2.18 for now unless you are adventurous or fairly familiar with QGIS already.

## Start QGIS
Open QGIS. Start a new project by either clicking on the white rectangle icon (with the corner turned down) or selecting "new" under the Project menu. You now should have a blank map canvas ready to go.

## Data

You'll need to download the following data from the data folder in this repository or from [FigShare](https://figshare.com/s/a59d4d0a8e6d2fadf592):

* Watershed Boundaries (Polygons)
* Watershed Centroids (Points)
* Rivers (Lines)

Add all three files to your map canvas.  Explore their attribute tables to get familiar with the data you'll be using.


## Starting the DB Manager Plugin
We'll be using the DB Manager plugin, so let's make sure that is installed:

1. Click the Database menu at the top of the QGIS window.  Do you have the DB Manager listed as an option in this menu?  If so, click the icon to open the tool and skip Step 2 below.  If not, move on to Step 2 below.
1. If the DB Manager is not in the Database menu, we need to enable the plug in:  

    i. Click the Plugins menu at the top of the window and select "Manage and install plugins".  This will open the Plugins Manager.  
    ii. Make sure you're in the "All" tab on the left side of the tool, and then in the search box, start typing "DB Manager" to narrow down the options.  
    iii. Select "DB Manager" from the list to view the details about this plugin.  
    iv. Click the *Install Plugin* button in the lower right corner to install it.  If this option isn't available, the tool is probably already installed and you just need to make sure the box next to the tool in the list is checked so that is is available.  Now go back to Step 1 above to start the tool.

## Make a Database
You can think of this database as a folder in which you keep files that are related to each other.  You don't want to put data in this database that isn't related to the other data (you could, but that's not the point of a database).

We'll need to make a new database to keep our spatial tables in:

1. Locate your Browser Panel. By default, it usually sits above or below the Layers Panel on the left side of the Map Canvas.  If you've closed it (like your instructor tends to do), you can get it back with View Menu --> Panels --> Browser Panel
1. Right click on the SpatiaLite item (it has the feather icon) and select *Create Database*
1. Navigate to where you would like to keep your database, perhaps in the folder where you downloaded the data for this workshop.  Name your database *sfbay* because we'll be working with San Francisco Bay data.  Yes, it should be all lowercase.

You may have noticed that there are many database format options available to you.  We're working with a SpatiaLite file today, but you can match your future databases to the needs of your project.

Let's connect to our database:

1. Open or locate the DB Manager that we found earier.
1. Right click on the *SpatiaLite* option and select *New Connection*.
1. Navigate to where you saved your *sfbay.sqlite* file, slect the file, and click *Open*..
1. Expand the list (click on the > symbol) to see that your database is now available.
1. If you expand the list next to the *sfbay.sqlite* database, you'll see if has some tables, but none of these contain data yet.


## Load some data to the database

Let's add some data to our database:

1. Click on *sfbay.sqlite* to select it in the list.
1. Click on the *Table* menu, and select *Import Layer/File* to open the Import Vector Layer dialog.
1. For the *Input*, drop down menu. Select the *Flowlines.geojson* file and click *Open*.
1. In the *Table* box, type *flowlines* for the name of our new table.
1. In the *Options* section, check the *Source SRID* box and in the space, type 3310 because the projection for this data is California Albers (NAD38) which has the SRID of 3310.

You can repeat this process to add the other two datasets.  Call the *WBDHU8_Points_SF* table *centroids* and *WBDHU8_SF* table *watersheds*.

Congratulations!  You now have a database with files related to the San Fransisco Bay's watersheds!

## The SQL Window

Now we're just about ready to do some analysis with our database.  Click on the *Open SQL Window* Icon (it looks like a wrench laying on a document).  You may need to expand the window by dragging the lower right corner of the window out so you can see everything.

You will type your queries into the big blank box at the top of the window (next to the 1 line number).  A query is a request for information from the database.

You'll run the query by clicking the *Execute (F5)* button or pressing F5 on your keyboard.

The results of the query will appear in the box below.  Sometimes the results will be a table; sometimes it will be a message.

A query has a structure to it.  The most common one you'll see today is a "select statement".  These start with the SELECT command, followed by the information you want to know, then the name of the table you want the information from, and finally (and optionally) other parameters that limit the results or provide some important caveats.  Queries end with a semicolon.

## Non-spatial Queries
Non-spatial queries are queries that don't involve the geometry column (the spatial information) of our table.

We'll start by investigating our *flowlines* data.  The *flowlines* are linear features that carry water from one place to another.  Some are nature features like rivers or streams, others are man-made like canals.

### Let's look at the whole table:

``` SELECT * FROM flowlines; ```

The asterisk (\*) means "everything" or "give me all the columns".  You could read the query as "Select everything from the flowlines table."

The result should look very much like the attribute table you explored earlier, but with a couple of additional columns.  The import process added an *id* field and a *geom* field.  The *geom* field contains information that allows the database tool to know where that particular object should be located in space, but unfortunately, it doesn't look like anything we understand as humans.  We'll learn to deal with this column more in a little while.

### Add a WHERE clause: 

``` SELECT * FROM flowlines WHERE FTYPE = 460; ``` 

This query limits our results to just the rows where the *FTYPE* column is 460, which corresponds to the natural rivers and streams (not canals).  "Where" in this case does **NOT** indicate location, but rather a condition of the data.

### Add a function: 

``` SELECT COUNT(id) FROM flowlines WHERE FTYPE = 460; ``` 

Here we've added the function COUNT().  So we've asked the database tool to count all of the IDs but only if they have an FYTYPE of 460.

### Summarize Data

What if we wanted to know how many lines there were of each FTYPE?

``` SELECT FTYPE, COUNT(id) FROM flowlines GROUP BY FTYPE; ```

Here, I've asked for a table with the FTYPE and the count of each FTYPE, and finally that it should summarize (group by) the FTYPE.

If I don't like the column name that it automatically generates - COUNT(id) - I can give it an alias with the AS command:

``` SELECT FTYPE, COUNT(id) AS NumberOfLines FROM flowlines GROUP BY FTYPE; ```

This is especially handy if you're making a table for people unfamiliar with your data or SQL.

## Basic Spatial Query Examples:
Let's look at the geometries: 

```SELECT ST_AsText(geom) FROM flowlines;``` 

You can make columns in the results tables larger by placing your mouse cursor over the edge of the column and dragging it out once the expander handle appears (it looks like two arrows pointing different directions).

Getting the lengths of the lines: ```SELECT id, ST_Length(geom) FROM flowlines;``` 

What are the units of the length query?  The units are meters because the units for the projection (California Albers; SRID 3310) are meters.

We just found the length of the individual *flowlines*.  That was not a very informative query.  It would be more useful to know what the total length of the lines are summed by their FCODE.  ```SELECT FCODE, ST_Length(geom) FROM flowlines GROUP BY FCODE;```

Get the area of polygons: ```SELECT NAME, ST_AREA(geom) FROM watersheds;``` 

Remember that if you don't like the column headings, you can alias them with *AS*.

## Projections:
Because we are working with spatial data, we need to know how to handle projections.

### Set the Projection
When you import your data into the DB Manager, it asked you what the SRID (EPSG Code) was for your data.  It's easy to forget to do this or to put in the wrong one if you're in a hurry.  If you discover that you've made a mistake, you don't need to re-import the table; you can set the SRID to the correct projection with an update command.  For our data it would look like this: 

```UPDATE flowlines SET geom = SetSRID(geom, 3310);```

This query replaces the contents of the *geom* column with the results of the SetSRID command.  In our case, it doesn't really do anything new since we had our projection set correctly, but you should know how to do this, so we did.

### Reproject
To change the projection of a dataset, you need to use the *Transform* or *ST_Transform* command.

Let's transform our watershed data into UTM Zone 10 North, the zone that San Fransisco falls into.

First, we'll start with a query that results in a returning information (but doesn't make a new table):

```SELECT id, HUC8, NAME, geom FROM watersheds;```

We have a table with a subset of the columns from the original watersheds table.  Now let's work on transforming the *geom* column.  We'll add a function around the *geom* column to reproject the data.  26910 is the SRID for UTM Zone 10 N.

```SELECT id, HUC8, NAME, ST_Transform(geom, 26910) FROM watersheds;```

It may look like nothing happened, but the column heading on the *geom* column should have changed.  Finally, we'll want do something to keep this data.  Remember that a SELECT statement just returns information, it doesn't save it.  We have two options.  (1) We could overwrite the *geom* column of the *watersheds* table, but that will mean the projection won't match the other data we have.  (2) The other option is to make a new table with a different projection.  We can do this by adding a CREATE TABLE statement to the query we already have:

```CREATE TABLE watershedsUTM AS SELECT id, HUC8, NAME, ST_Transform(geom, 26910) as geom FROM watersheds;```

To see the new table in the list, we'll need to refresh our database.  From the Database menu at the top of the DB Manager window, select *Refresh*.  Now we can see the table, but it's just a table.  The database doesn't seem to know the table is actually polygons.

```SELECT * FROM watershedsUTM;``` Shows that all the columns we asked for, including the *geom* column are there.  What's going on?  We need to recover the geometry column so the database will recognize the table as a spatial table.

```SELECT RecoverGeometryColumn('watershedsUTM', 'geom', 26910, 'MULTIPOLYGON', 'XY');```

This query will return a single column and row.  Now we need to vacuum the database (yes, that sounds a little odd).  From the *Database* menu, select *Run Vacuum*.  Now the icon next to the *watershedsUTM* table should look like a set of polygons.

Now we could add this to our map canvas to see the polygons.  Right click on the *watershedsUTM* table in the tree, and select *add to map canvas*.  Note that we just reprojected the data, so it won't look too much different.


## Spatial Analysis:
Not surprisingly, you can do more than just get lengths and areas, or change projections.

### Intersect

Intersect

### Distance

Distance

### Spatial Join
spatial join

### Buffer & Nesting Functions
One interesting thing about SQL is that you can nest functions to do a series of functions in one query.  For example, maybe I want to find out the area (in square kilometers) within 1 kilometer of all the flowlines.

SELECT sum(ST_Area(ST_Buffer(geom, 1000)))/1000000 FROM flowlines;

Here, we take the sum of the area of the buffer of 1000 meters, then divide the whole thing by 1,000,000 to convert square meters to square kilometers.  Wow.  But I didn't have to make a bunch of intermediate files and add columns to an attribute table, then save a CSV, then sum it all up in Excel.  Now which option sounds crazier?  Perhaps you're starting to see some of the power of spatial SQL.


# Conclusion
Today we've gotten an introduction to Spatial Databases and Spatial SQL.  This is certainly not all you can do with these tools.  To encourage you to keep learning, I've provided more resources below.  Additionally, a well-crafted Google search can return helpful posts and tutorials for learning new skills.



# Additional Resources:

## Spatial SQL Resources:

[SpatiaLite Function Reference List](http://www.gaia-gis.it/gaia-sins/spatialite-sql-4.2.0.html)

## General Slides & Tutorials:

[Todd Barr's Slides](https://www.slideshare.net/gis_todd/foss4g-2017-spatial-sql-for-rookies)

[Mike Miller's Tutorial](http://millermountain.com/geospatialblog/2017/10/23/qgis-and-spatialite/)


*****Stoped at slide 56 in https://www.slideshare.net/gis_todd/foss4g-2017-spatial-sql-for-rookies









