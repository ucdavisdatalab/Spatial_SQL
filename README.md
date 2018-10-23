# Spatial SQL with SpatiaLite
An introductory workshop on Spatial SQL using SpatiaLite.  

## Expected Learning Outcomes
This workshop is intended to give you an introduction to spatial SQL through working with a graphical user interface (GUI) with some examples of common analysis processes as well as present you with resources for continued learning.  This workshop cannot teach you everything you could possibly need to know about databases, but rather is an introduction.


# Concepts

## What is a database?
A database is a set of data in tables that are related to each other in some way.  That's it.  It's just a collection of tables.

Generally each table can be connected to another table by a column that both tables have that store the information to match up the rows.  This column is called a **key**.  A key commonly used on campus is your student or employee ID number.

You already use a database if you've done GIS.  A shapefile is essentially just a fancy a table.

Below is an example of a database diagram for the files involved in a Marxan analysis.  At least one column in each table relates to a column in another table (indicated in this diagram by a line drawn between the two columns).  Documenting a database with a diagram like this is common practice.  It provides a quick visual reference to the data contained in each table and how the tables relate to each other.

![image](https://experimentalcraft.files.wordpress.com/2013/08/marxantables.png)

Image Credit: [Michele Tobias](https://experimentalcraft.wordpress.com/2013/08/23/marxan-table-relationships/)

[Schmierer, et al. 2007](https://www.joe.org/joe/2007august/tt7.php) have a couple of diagrams of the relationships between the SSURGO tables.  Unfortunately, the journal posted their images in .gif format so I can't make them visible here.  Check out the link though!

## What is a Spatial Database?
A spatial database is just a normal database with a column in the tables that holds the spatial information commonly called the "geometry".  The geometry information is stored as a Binary Large Object (BLOB).  The geometry information allows us to relate the tables to each other based on their location and also to perform spatial analysis on our data.

## What is Spatial SQL?
SQL stands for "structured query language" and it's a language that allows you to ask questions of a database.  Spatial SQL is regular SQL but with some additional functions that perform spatial analysis.

If you've ever written an attribute query in ArcGIS or QGIS, you've worked with SQL.  Example: Hey GIS program, please highlight all the records in my attribute table that have "Yolo" in the "county" column!  In SQL, we would write ```SELECT * FROM county_shapefile WHERE county = 'Yolo'; ```  It's actually quicker to write that query than to fill out the interface in the GIS.

## Why do you want to learn to work with spatial databases and spatial SQL?
* It's a good way to work with large amounts of data
* Typically faster to run a process in a spatial database than in a desktop GIS program
* Store lots of data (compare with shapefile's 70m row limit)
* One database file stores many, many tables --> easier data management
* Write a query instead of making a new file (no exporting of results to shapefile necessary!)

## What makes this challenging?
If you're a GIS user, you're probably used to a graphical user interface (GUI) where you can see your data, have tools with guided interfaces, and can see the results of your processing immediately.  These aren't things you get with a typical database manager tool, however, we can connect our database to QGIS so we can see our results and with practice, you will get used to the typical workflow and seeing everything won't be so necessary.

## Databases that support Spatial SQL:
* Oracle
* MySQL
* SQL Server
* SpatiaLite
* PostGRES --> PostGIS
* Others too!

We'll be working with SpatiaLite, because it works on all the common computer operating systems and is fairly easy to install.  Once you learn the basics, you can choose the database program that best fits your needs.


---------------------------------------------
# Hands-On Tutorial

## Installation

**Install SpatiaLite:**  The instructions for intalling SpatiaLite varies depending on your operating sytsem.  We've provided a [guide to installing the software](https://github.com/MicheleTobias/Spatial_SQL/blob/master/Install.md).

**Install QGIS:** You should have installed QGIS version 2.18 or 3.0 from [QGIS.org](https://qgis.org/en/site/forusers/download.html).  This workshop is written for version 2.18; the instructions may also work for 3.0 but haven't been tested yet.  We'll be using QGIS to visualize our spatial data tables that we import into our SpatiaLite database.


## Data

You'll need to download the following data from the data folder in this repository or from [FigShare](https://figshare.com/articles/SpatiaLite_Workshop_Data/7227581):

* Watershed Boundaries (Polygons)
* Watershed Centroids (Points)
* Rivers (Lines)

If you downloaded a .zip file, be sure to unzip it.  

![alt text](https://github.com/MicheleTobias/Spatial_SQL/blob/master/images/DataPreview.png)


## Starting SpatiaLite
Let's start by opening Spatialite:

1. In your computer's file explorer, navigate to the Spatialite folder you recently unzipped.
1. Start Spatialite by running the Spatialite_gui.exe file in this folder.

## Make a Database
You can think of a database as a folder in which you keep tables that are related to each other.  You don't want to put data in this database that isn't related to the other data (you could, but that's not the point of a database).

We'll need to make a new empty database to keep our spatial tables in:

1. Click the "Creating a new (empty) SQLite Database" ![alt text](https://github.com/MicheleTobias/Spatial_SQL/blob/master/images/SpatiaLite_Button_NewDB.PNG)
1. Navigate to where you would like to keep your database, perhaps in the folder where you downloaded the data for this workshop.  Name your database *sfbay.sqlite* because we'll be working with San Francisco Bay data.  Yes, it should be all lowercase. Click OK.

![alt text](https://github.com/MicheleTobias/Spatial_SQL/blob/master/images/SpatiaLite_Inteface_ConnectedDB.PNG)

Note that Spatialite cna only connect to one database at a time, so if you want to work with another database, disconnect this one.

## Load some data to the database

Let's add some data to our database:

1. Click the "Load Shapefile" button. ![alt text](https://github.com/MicheleTobias/Spatial_SQL/blob/master/images/SpatiaLite_Button_LoadShapefile.PNG)
1. Navigate to your workshop data folder.
1. Select the *Flowlines.shp* file and click *Open*.
1. In the *Table Name* box, type *flowlines* for the name of our new table.
1. In the *SRID* box, type 3310 because the projection for this data is California Albers (NAD38) which has the SRID of 3310.
1. Review the other options, but you can leave the defaults as they are.  Click *OK* when you're done.

You can repeat this process to add the other two datasets.  Call the *WBDHU8_Points_SF* table *centroids* and *WBDHU8_SF* table *watersheds*.

You should see your three tables listed in the panel on the left.  Congratulations!  You now have a database with files related to the San Francisco Bay's watersheds!

## The SQL Window

Now we're just about ready to do some analysis with our database.  You may need to expand the Spatialite window by dragging the lower right corner of the window out so you can see everything.

You will type your queries into the big blank box at the top right of the window.  A query is a request for information from the database.

You'll run the query by clicking the *Execute SQL Statement* button to the right of the query box.

The results of the query will appear in the box below.  Sometimes the results will be a table; sometimes it will be a message.

A query has a structure.  The most common one you'll see today is a "select statement".  These start with the SELECT command, followed by the information you want to know, then the name of the table you want the information from, and finally (and optionally) other parameters that limit the results or provide some important caveats.  Queries end with a semicolon.

## Non-spatial Queries
Non-spatial queries are queries that don't involve the geometry column (the spatial information) of our table.

We'll start by investigating our *flowlines* data.  The *flowlines* are linear features that carry water from one place to another.  Some are nature features like rivers or streams, others are man-made like canals.

### Let's look at the whole table:

```SQL 
SELECT * FROM flowlines; 
```

The asterisk (\*) means "everything" or "give me all the columns".  You could read the query as "Select everything from the *flowlines* table."

The result should look very much like an attribute table you might expect to see in a graphical GIS, but with one additional column.  The import process added *geometry* field.  The *geom* field contains information that allows the database tool to know where that particular object should be located in space, but unfortunately, it doesn't look like anything we understand as humans.  We'll learn to deal with this column more in a little while.

### Add a WHERE clause: 

```SQL
SELECT * FROM flowlines WHERE FTYPE = 460; 
``` 

This query limits our results to just the rows where the number in the *FTYPE* column is 460, which corresponds to the natural rivers and streams (not canals).  "Where" in this case does **NOT** indicate location, but rather a condition of the data.

### Add a function: 

```SQL
SELECT COUNT(PK_UID) FROM flowlines WHERE FTYPE = 460; 
``` 

Here we've added the function COUNT().  So we've asked the database tool to count all of the IDs but only if they have an FYTYPE of 460.

### Summarize Data

What if we wanted to know how many lines there were of each *FTYPE*?

```SQL
SELECT FTYPE, COUNT(PK_UID) FROM flowlines GROUP BY FTYPE;
```

Here, I've asked for a table with the *FTYPE* and the count of each *PK_UID*, and finally that it should summarize (group by) the *FTYPE*.

If I don't like the column name that it automatically generates - ```COUNT(PK_UID)``` - I can give it an alias with the AS command:

```SQL 
SELECT FTYPE, COUNT(PK_UID) AS NumberOfLines FROM flowlines GROUP BY FTYPE; 
```

This is especially handy if you're making a table for people unfamiliar with your data or SQL or if you need the column name to be something specific.

## Basic Spatial Query Examples:

### View Geometry
Let's start understanding spatial queries by looking at the geometries column: 

```SQL 
SELECT ST_AsText(geometry) FROM flowlines;
``` 

*ST_AsText()* lets us see the geometry string in human-readable form.  This isn't very useful most of the time, but perhaps it's comforting to know it's there.  You can make columns in the results tables larger by placing your mouse cursor over the edge of the column and dragging it out once the expander handle appears (it looks like two arrows pointing different directions).

### Length
Let's do an analysis that you might come across.  Let's get the lengths of each of the *flowlines*: 
```SQL
SELECT PK_UID, ST_Length(geometry) FROM flowlines;
``` 

What are the units of the length query?  The units are meters because the units for the projection (California Albers; SRID 3310) are meters.

We just found the length of the individual *flowlines*.  That was not a very informative query.  It would be more useful to know what the total length of the lines are summed by their FCODE.  

```SQL
SELECT FCODE, SUM(ST_Length(geometry)) FROM flowlines GROUP BY FCODE;
```

### Area
Let's look at an example to get the area of the *watershed* polygons: 

```SQL
SELECT NAME, ST_AREA(geometry) FROM watersheds;
``` 

Remember that if you don't like the column headings, you can alias them with *as*.

## Projections:
Because we are working with spatial data, we need to know how to handle projections.

### Set the Projection
When you import your data into Spatialite, it asked you what the SRID (EPSG Code) was for your data.  It's easy to forget to do this or to put in the wrong one if you're in a hurry.  If you discover that you've made a mistake, you don't need to re-import the table; you can set the SRID to the correct projection with an update command.  For our data it would look like this: 

```SQL
UPDATE flowlines SET geom = SetSRID(geometry, 3310);
```

This query replaces the contents of the *geom* column with the results of the SetSRID command.  In our case, it doesn't really do anything new since we had our projection set correctly, but you should know how to do this, so we did.

### Reproject
To change the projection of a dataset, you need to use the *Transform* or *ST_Transform* command.

Let's transform our watershed data into UTM Zone 10 North, the zone that San Francisco falls into.

First, we'll start with a query that results in a returning information (but doesn't make a new table):

```SQL
SELECT PK_UID, HUC8, NAME, geometry FROM watersheds;
```

We have a table with a subset of the columns from the original watersheds table.  Now let's work on transforming the *geom* column.  We'll add a function around the *geometry* column to reproject the data.  26910 is the SRID for UTM Zone 10 N.

```SQL
SELECT PK_UID, HUC8, NAME, ST_Transform(geometry, 26910) FROM watersheds;
```

It may look like nothing happened, but the column heading on the *geom* column should have changed.  Finally, we'll want do something to keep this data.  Remember that a SELECT statement just returns information, it doesn't save it.  We have two options.  (1) We could overwrite the *geometry* column of the *watersheds* table, but that will mean the projection won't match the other data we have.  (2) The other option is to make a new table with a different projection.  We can do this by adding a CREATE TABLE statement to the query we already have:

```SQL
CREATE TABLE watershedsUTM AS 
SELECT PK_UID, HUC8, NAME, ST_Transform(geometry, 26910) as geometry 
FROM watersheds;
```

To see the new table in the list, we'll need to refresh our database.  From the Database menu at the top of the DB Manager window, select *Refresh*.  Now we can see the table, but it's just a table.  The database doesn't seem to know the table is actually polygons.

```SELECT * FROM watershedsUTM;``` Shows that all the columns we asked for, including the *geometry* column are there.  What's going on?  We need to recover the geometry column so the database will recognize the table as a spatial table.

```SQL
SELECT RecoverGeometryColumn('watershedsUTM', 'geometry', 26910, 'MULTIPOLYGON', 'XY');
```

This query will return a single column and row.  Now we need to vacuum the database (yes, that sounds a little odd).  Run the command ```VACUUM;``` in the SQL Query panel.  This will clean up any issues created by all the changes we just made to the database.


## Spatial Join
Spatial joins allow us to combine information from one table with another based on the location associated with each record.  Let's see if we can figure out which watershed each of our flowlines is in:

```SQL
SELECT flowlines.*, watersheds.NAME as Watershed_Name
FROM flowlines, watersheds
WHERE ST_Contains(watersheds.geometry, flowlines.geometry);
```

Your table should look just like your *flowlines* table, but we've added the *NAME* column from our *watersheds* table (but called it "Watershed_Name" because this will make more sense if we needed to use the this data later and didn't remember where this information came from). 

ST_Contains tells us if a line is completely within a particular watersheds polygon.  How would you change this query to identify which watershed each line *intersects* rather than is *contained by*? Hint: [SpatiaLite Function Reference List](http://www.gaia-gis.it/gaia-sins/spatialite-sql-4.2.0.html) 

## Views vs. Tables
We've discussed that one of the big reasons to use a database is not needing to save a bunch of intermediate data products.  You just keep your data in tables and you write queries to combine them in various ways.  But what if you find yourself needing the results a particular query quite often?  Do you save a table?  There's another option: the view.  A view is like a saved query.  It behaves like a table so you can call it in other queries or view it in QGIS, but the records in the table are generated by running a query rather than storing the same data over again.

Let's make a view!  Let's say we want to have a view that contains all the rivers (so, FTYPE = 460, like before) that are in the Tomales-Drake Bays watershed and let's put the watershed name into the table as well.

First, let's make a query to make sure we're getting the information we want.

```SQL
SELECT flowlines.*, watersheds.name as watershed_name
FROM flowlines, watersheds
WHERE ST_Contains(watersheds.geometry, flowlines.geometry)
AND FTYPE = 460
AND watersheds.name LIKE 'Tomales-Drake Bays';
```

That query looks like what we want.  Let's make it into a view.  View creation sytax is really similar to the way you make a table:

```SQL
CREATE VIEW rivers_tomales AS
SELECT flowlines.*, watersheds.name as watershed_name
FROM flowlines, watersheds
WHERE ST_Contains(watersheds.geometry, flowlines.geometry)
AND FTYPE = 460
AND watersheds.name LIKE 'Tomales-Drake Bays';
```

This is a non-spatial view.  To have the database recognize it as a spatial view, we need one more step that registers the view in the table that keeps track of which views are spatial.  (If this sounds complicated, just file this away as a step that needs to get done.)

```SQL
INSERT INTO views_geometry_columns
(view_name, view_geometry, view_rowid, f_table_name, f_geometry_column, read_only)
VALUES ('rivers_tomales', 'geometry', lower('PK_UID'), 'flowlines', 'geometry', 0);
```

What does this query do?  I'll break it down.

```INSERT INTO views_geometry_columns``` tells the databse to put data into the *views_geometry_columns* table.  This is a table that comes standard with Spatialite databases.  It was there as soon as we made the database.  It tells the rest of the databse which views are spatial.  There's a similar table for listing spatial tables.  

```(view_name, view_geometry, view_rowid, f_table_name, f_geometry_column, read_only)``` is the list of columns we want to put information into in the *views_geometry_columns* table.  We'll be making a new row of data in this table, and these are the columns where that data is going to go.

```VALUES ('rivers_tomales', 'geometry', lower('PK_UID'), 'flowlines', 'geometry', 0)``` *VALUES* says to the database, "here is the list of items to put in the columns I told you about in the last line", then we put the list of things in parentheses.  So 'rivers_tomales' (the new table name) will go into the 'view_name' field. 'geometry' is the geometry column for the view so it goes into the 'view_geometry' field of the table. I wrapped 'PK_UID' (the ID column) in the lower() function because it complained that the input had to be lowercase.  Lower() just turns all the input characters into their lower case counterparts.  'flowlines' is the table that our 'rivers_tomales' view inherits its spatial data from and it has a geometry column called 'geometry' as well.  Finally, 'read_only' takes either a 0 to make the table read-only or 1 to make it writable.  Read-only is a good choice here since we won't be adding or changing the view's contents.



## Viewing Tables in QGIS

Tables and views are usefull, but this is spatial data so it might be nice to look at the data in map form.  We can look at our spatial tables and views directly in QGIS.  Let's look at some of our tables and views:

1. Open QGIS (2.18 or 3.0)
1. In the Browser Panel (usually on the left by default... add it if it's missing: *View* menu -> *Panels* -> *Browser Panel*).  Click to expand the Spatialite list.  There's no *sfbay* database yet!
1. Right click on the Spatialite section in the Browser Panel and select *New connection*
1. Navigate to and select your *sfbay.sqlite* database.  Click *ok*.
1. In the Browser Panel, expand your *sfbay*.sqlite database to see the tables and views
1. Double click the tables or views to add them to the map canvas.  **Note:** Tables and (especially) views with a lot of records will take a while to load.  I advise adding one at a time and saving after each table addition just in case it crashes.
1. Now you can access the symbology, labels, and other standard tools in the *Layer Properties* for each layer, just as you would any other spatial dataset in QGIS.


## More Spatial Analysis:
Not surprisingly, you can use a spatial database to do more than just get lengths and areas of existing geometries, or change projections.  Let's go back to Spatialite and explore some more spatial analysis possibilities.


### Distance
Let's find out which watershed is closest to the city of San Francisco.  We could go about this a number of ways, but let's find the distance from the city's center point to the centroid of each watershed:

```SQL
SELECT ST_Distance(MakePoint(37.7749, -122.4194), centroids.geometry) 
FROM centroids;
```

Here we used *MakePoint()* to turn a set of latitude/longitude coordinates into a format that  the database tool understands, then put the results into the *Distance()* function.

How could you make this table more informative?  Could you add or rename some columns?
 

### Buffer & Nesting Functions
One interesting thing about SQL is that you can nest functions to do a series of functions in one query like you just saw above, but it can get more complex.  For example, maybe I want to find out the area (in square kilometers) within 1 kilometer of all the *flowlines*.

```SQL
SELECT sum(ST_Area(ST_Buffer(geom, 1000)))/1000000 FROM flowlines;
```

Here, we take the sum of the area of the buffer of 1000 meters, then divide the whole thing by 1,000,000 to convert square meters to square kilometers.  Wow.  That's pretty complicated.  But I didn't have to make a bunch of intermediate files and add columns to an attribute table, then save a CSV, then sum it all up in Excel.  Now which option sounds crazier?  Perhaps you're starting to see some of the power of spatial SQL.


# Conclusion
Today we've gotten an introduction to Spatial Databases and Spatial SQL.  This is certainly not all you can do with these tools; we've barely scratched the surface!  To encourage you to keep learning, I've provided more resources below.  Additionally, a well-crafted Google search can return helpful posts and tutorials for learning new skills.



# Additional Resources:

## Spatial SQL Resources:

[SpatiaLite Function Reference List](http://www.gaia-gis.it/gaia-sins/spatialite-sql-4.2.0.html)

[SpatiaLite Cookbook](https://www.gaia-gis.it/spatialite-3.0.0-BETA/spatialite-cookbook/index.html)

## General Slides & Tutorials:

[Todd Barr's Slides](https://www.slideshare.net/gis_todd/foss4g-2017-spatial-sql-for-rookies)

[Mike Miller's Tutorial](http://millermountain.com/geospatialblog/2017/10/23/qgis-and-spatialite/)
