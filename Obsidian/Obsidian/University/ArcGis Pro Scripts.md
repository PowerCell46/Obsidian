
### Copy a layer to the project's GDB

```python
# Path of the shp file, name of the copy
arcpy.management.CopyFeatures("C:\\University\\Udemy\\Data\\ne_10m_admin_1_states_provinces.shp", "StateProvinces")
```

### Get number of entries in a layer

```python
arcpy.management.GetCount("C:\\University\\Udemy\\Data\\ne_10m_admin_0_countries.shp")
```
### Create GDB

```python
arcpy.CreateFileGDB_management("C:\\University\\Udemy\\Test", "SampleGeodatabase")
```

### Create a layer by selecting entries from another one

```python
arcpy.analysis.Select("C:\\University\\Udemy\\Data\\ne_10m_admin_0_countries.shp", "BulgariaPolygon", "NAME ='Bulgaria'")

# Old variant
arcpy.Select_analysis("C:\\University\\Udemy\\Data\\ne_10m_admin_0_countries.shp", "Egypt", "NAME = 'Egypt'")
```

### ENV variables

```python
# Setting a default path
arcpy.env.workspace = "C:\\University\\Udemy\\Test.gdb"

# All functions will overwrite the OG data
arcpy.env.overwriteOutput = True
```

____
# Cursor

### Get Column names as an array

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

districtLayerColumnNames = [col.name for col in arcpy.ListFields(districtsPath)]

districtLayerColumnNames
```

### Using SearchCursor (SELECT projection)

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

with arcpy.da.SearchCursor(
			districtsPath, 
			["Name_bg", "District_code", "EKATTE", "SHAPE_Length", "SHAPE_Area"],
			"Name_bg LIKE 'Б%'"	
		) as cursor:
    for row in cursor:
        print(row)
```

### Print the geometry object for every entry

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

with arcpy.da.SearchCursor(districtsPath, ["Name_bg", "SHAPE@XY", "SHAPE@AREA", "SHAPE@LENGTH", "SHAPE@"]) as cursor:
    for row in cursor:
        print(f'District: {row[0]}\nCentroid X: {row[1][0]} Centroid Y: {row[1][1]}\nArea: {row[2]}, Length: {row[3]}.')
        # print(row[4].partCount, end="\n\n")
        # print(row[4].pointCount, end="\n\n")
```

### Using comprehensions

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

districtsNamesBg = [row[0] for row in arcpy.da.SearchCursor(districtsPath, ["Name_bg"])]
districtsNamesEn = [row[0] for row in arcpy.da.SearchCursor(districtsPath, ["Name_en"])]

districtsProjection = [[row[0], row[1], row[2]] for row in arcpy.da.SearchCursor(districtsPath, ["Name_bg", "EKATTE", "SHAPE_Area"])]


districtsDictionary = {row[0]:row[1] for row in arcpy.da.SearchCursor(districtsPath, ["Name_bg", "District_code"], "OBJECTID < 10")}
```

### Using Update Cursor

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

with arcpy.da.UpdateCursor(districtsPath, ["District_code"], "Name_en = 'Sofia'") as cursor:
    for row in cursor:
        row[0] = row[0].upper()
        cursor.updateRow(row)
        print(row[0])
```

### Add new column to the table

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

arcpy.management.AddField(districtsPath, "NewPythonCol", "TEXT")

with arcpy.da.UpdateCursor(districtsPath, ["District_code", "NewPythonCol"], "Name_en = 'Sofia'") as cursor:
    for row in cursor:
        row[0] = row[0].upper()
        row[1] = "Dynamic Python value"
        cursor.updateRow(row)
        print(row[0])
```

### Delete row by a condition

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

with arcpy.da.UpdateCursor(districtsPath, ["Name_bg"]) as cursor:
    for row in cursor:
        if row[0] == 'Търговище':
            cursor.deleteRow()
```

### Insert cursor

```python
districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

cursor = arcpy.da.InsertCursor(districtsPath, ["Name_bg", "NewPythonCol"])
cursor.insertRow(["Търговище", "Targovishte python val"])
cursor.insertRow(["Без име", "No name python val"])
del cursor

# Create a polygon dynamically
cursor = arcpy.da.InsertCursor(districtsPath, ["SHAPE@", "Name_bg"])

mercatorProjection = arcpy.Describe(districtsPath).spatialReference
nullIslandPoint = arcpy.Point(0, 0)
nullIslandGeometry = arcpy.PointGeometry(nullIslandPoint, mercatorProjection).buffer(5)
cursor.insertRow([nullIslandGeometry, "Null Island"])
del cursor
```

___
# Statistics

### Create stats table from a layer

```python
# Input layer, Table name, [field, aggFunc], groupByField (optional)
arcpy.Statistics_analysis(
	"Countries",
	"ContinentsStats", 
	[["POP_EST", "MEAN"], ["POP_EST", "SUM"]], 
	"CONTINENT"
)

arcpy.Statistics_analysis(
	"Countries", 
	"WorldStats", 
	[["POP_EST", "MEAN"], ["POP_EST", "SUM"]]
)

# Create a new layer with sorted values
arcpy.Sort_management(
	"Countries", 
	"CountriesSorted", 
	[["POP_EST", "DESCENDING"]]
)
```


```python
import arcpy
import pandas as pd
import matplotlib.pyplot as plt

layerName = "RasterT_Classif1"
columnName = "gridcode"

data = arcpy.da.SearchCursor(layerName, [columnName])
pandasData = pd.Series(data)

# pandasData.value_counts()

plt.title("Percentage for the RasterT_Classif1 of Burned and Unburned entries")
plt.bar(["Unurned", "Burned"], pandasData.value_counts())
plt.xlabel("Type")
plt.ylabel("Number of entries")
plt.show()
```


----

# [[Pandas]]

### ArcPy SearchCursor result to a Dataframe

```python
import arcpy
import pandas as pd

districtsPath = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse\\ArcpyUdemyCourse.gdb\\BulgarianDistricts"

columns = [column.name for column in arcpy.ListFields(districtsPath)]

districtsData = [row for row in arcpy.da.SearchCursor(districtsPath, columns)]
districtsDataframe = pd.DataFrame(districtsData, columns=columns)
districtsDataframe

# Better way (faster)
array = arcpy.da.FeatureClassToNumPyArray(districtsPath, columns)

districtsDataframe = pd.DataFrame(array.tolist(), columns=columns)
districtsDataframe

```


----

# Performing GIS inventory

```python
import arcpy

arcpy.env.workspace = "C:\\Users\\HP ZBook 17 G5\\Documents\\ArcGIS\\Projects\\ArcpyUdemyCourse"

workList = arcpy.ListWorkspaces()

geodatabases = []
for workspace in workList:
    if workspace.endswith(".gdb"):
        geodatabases.append(workspace)
    # arcpy.env.workspace = workspace
    # gdbList = arcpy.ListWorkspaces("", "FileGDB")
    # print(f'{workspace} constains {gdbList}')
    # geodatabases.extend(gdbList)

for gdb in geodatabases:
    arcpy.env.workspace = gdb
    print(f'Geodatabase: {gdb}\nFeature classes: {", ".join(arcpy.ListFeatureClasses())}')
    print(f'Geodatabase: {gdb}\nDatasets: {", ".join(arcpy.ListDatasets("", "Feature"))}')
    fdList = arcpy.ListDatasets("", "Feature")
    for fd in fdList:
        arcpy.env.workspace = r"{0}/{1}".format(gdb, fd)
        print(f'Feature classes in feature dataset {arcpy.env.workspace}:\n{arcpy.ListFeatureClasses()}')

for gdb in geodatabases:
    arcpy.env.workspace = gdb
    # print(f'Tables in {gdb}:\n {", ".join(arcpy.ListTables())}')
    for table in arcpy.ListTables():
        tablePath = arcpy.env.workspace + "\\\\" + table
        print(f'{table}: {[[f.name, f.type] for f in arcpy.ListFields(tablePath)]}')
```