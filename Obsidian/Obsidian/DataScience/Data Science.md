## Setting up the environment

1. Open the **Anaconda prompt**
2. Create a directory and enter it using the cd ***name***
3. run **conda create --prefix ./env pandas numpy matplotlib scikit-learn** 
4. run **conda activate "C:\Users\HP ZBook 17 G5\Desktop\sample_project_1\env"**

Now the environment is setup and is running, and you can use **jupyter notebook***
##### <span style="color:rgb(107, 255, 174)">or</span>

use jupiter notebook in VsCode

__________________________________________________________________________

# Reading from different sources of data

```python
import pandas as pd

pd.read_csv("../accidents.csv")

pd.read_csv("../accidents.csv").columns # name of the columns

pd.read_csv("../accidents.csv").index   # (start, stop, step)

pd.read_csv("../accidents.csv").shape   # shape of the table

#--------------------------------------------------------------#

# %pip install openpyxl  

green_tripdata = pd.read_excel('../BG_Example_ImportA.xlsx')

green_tripdata

#--------------------------------------------------------------#

pd.read_json("../cigarConsumption.json")

pd.read_json("../cigarConsumption.json")[["country", "annual_consumption_per_capita"]]

#--------------------------------------------------------------#

import sqlalchemy

engine = sqlalchemy.create_engine(
	"postgresql+psycopg2://postgres:PowerCell46@localhost/santa_db_evn"
)

# pd.read_sql_table("import_value", engine)

pd.read_sql_query(""
"SELECT"
"   id, "
"   reporting_point_own, "
"   loi_document_type_id, "
"   period_from, "
"   period_to, "
"   price_in_levs, "
"   agreement_type_id, "
"   power_plant_id, "
"   is_valid "
"FROM"
"   import_value"
"", engine
)

#--------------------------------------------------------------#

%pip install requests
%pip install bs4
import requests
from bs4 import BeautifulSoup

emag = requests.get("https://www.emag.bg/")
  
parser = BeautifulSoup(emag.content, "html.parser")

main_container = parser.find(id="main-container")

if main_container:
    print(main_container.prettify())
else:
    print("Element not found!")
```

## Pandas melt
	
```python
data_tidy = data.melt(

    id_vars=["religion"],

    var_name="income",

    value_name="frequency"

) # leave out only the religion column and transform all other cols to a single column

data_tidy
```