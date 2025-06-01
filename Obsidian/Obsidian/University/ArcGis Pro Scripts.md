
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