## Series and DataFrame

Table (Dataframe) 
- May have many dimensions
- usually call this dataset

Dataframe: Columns, Rows, Data
Series: One-dimensional ndarray with axis labels 

List (Series)
- One-dimensional
- usually represents a column of a table

pd concat
pd merge

```python
# One dimensional  
series = pd.Series(['BMW', 'Toyota', 'Mazda'])  
  
colours = pd.Series(['red', 'blue', 'white'])  

# Two Dimensional  
car_data = pd.DataFrame({"Car Make": series, "Colour": colours})


# First Five Entrances  
print(car_sales.head())  
  
# Last Five Entrances  
print(car_sales.tail())

print(car_sales.sample(5)) # Five Random Entries

animals = pd.Series(['cat', 'dog', 'snake', 'bird', 'panda', 'human'], index=[0, 3, 9, 8, 3, 2])  

pokemonData.loc["Eevee"] # Single []: RETURNS SERIES
pokemonData.loc[["Eevee"]] # DOUBLE [[]]: RETURNS DATAFRAME

eevolutions = [
    "Eevee",
    "Jolteon",
    "Flareon",
    "Vaporeon",
    "Umbreon",
    "Espeon",
    "Glaceon",
    "Leafeon",
    "Sylveon"
]

pokemonData.loc[eevolutions]

# Returns the index and the position of the given number  
print(pokemonData.loc["Mewtwo"]) # Location

kantoKings = pokemonData.loc[143:151][["name", "attack", "defense", "hp"]]
  
# Returns only the position of the given number  
pokemonData.iloc[[24]] # Pikachu: Index Location
```

## Data Selection (Similar to SQL Queries)

```python
print(car_sales[car_sales['Make'] == 'Toyota'])  
  
print(car_sales[car_sales['Doors'] > 3])

# Returns DataFrame - Like a SELECT query, specifying the columns  
print(accidents[['Miles from Home', '% of Accidents']])


legendaries = pokemonData[pokemonData["is_legendary"] == 1][["name", "japanese_name", "attack", "defense", "hp"]]


# Generation == 1 OR generation == 4
pokedex = pokedex[(pokedex.generation == 1) | (pokedex.generation == 4)]

bestStatsPokemon = pokedex[
    (pokedex["attack"] == pokedex["attack"].max()) |
    (pokedex["defense"] == pokedex["defense"].max()) |
    (pokedex["hp"] == pokedex["hp"].max())
]


# Water type AND legendary AND is from generation 3
pokedex[
	(pokedex["type1"] == "water") & 
	(pokedex["is_legendary"] == 1) & 
	(pokedex["generation"] == 3)
]


# Either defense < 100 Either Attack > 100, BUT NOT BOTH(defense < 100 AND attack > 100)
pokedex[(pokedex["defense"] < 100) ^ (pokedex["attack"] > 100)]

# bitwise NOT: The reverse result
pokedex[~(pokedex["attack"] >= 100)]


pokedex[pokedex["attack"].between(50, 100)]

favouriteLegendaries = [
    "Moltres",
    "Mewtwo",
    "Rayquaza",
    "Groudon",
    "Kyogre",
    "Palkia",
    "Dialga"
]

pokedex[pokedex["name"].isin(favouriteLegendaries)]

# Getting all entries with type2 == NaN
singleTypePokemon = pokedex[pokedex["type2"].isna()]
```

## Sorting 

```python
bitcoinData = pd.read_csv("../csvData/BTC-USD.csv", index_col = 0)

# Sort in ASC order by the Open column
bitcoinData = bitcoinData.sort_values("Open", ascending=True)

# Sort in DESC order by Volume and then by High column
bitcoinData = bitcoinData.sort_values(["Volume", "High"], ascending=False)

# Sort String cols by lowercasing them with a lambda function
pokemonData = pokemonData.sort_values("name", ascending=True, key=lambda col: col.str.lower())


pokemonData = pd.read_csv("../csvData/AllPokemon.csv", index_col=30)  

# Sort by the index 
pokemonData = pokemonData.sort_index(key=lambda col : col.str.lower())
```

## Adding and Removing Columns

```python
# Delete a column
bitcoinData.drop(columns=["Adj Close"], inplace=True)

# Delete a row (by its index)
bitcoinData.drop(labels=[1], axis=0, inplace=True)

# Drop rows by slicing by order
bitcoinData.drop(bitcoinData.index[0:3], inplace=True)


# Create new column at the end
bitcoinData["Change"] = bitcoinData["Close"] - bitcoinData["Open"]

# Create new column at a given position
bitcoinData.insert(1, "Change", bitcoinData["Close"] - bitcoinData["Open"])

bitcoinData.insert(2, "Change %", (bitcoinData["Change"] / (bitcoinData["Open"] / 100)))
```

## Changing the column values

```python
# Renaming a column
pokedex.rename(columns={
    "pokedex_number": "pokedex number",
    "japanese_name": "japanese name",
    "sp_attack": "special attack",
    "sp_defense": "special defense",
    },
    inplace=True
)

# Replacing a column value with another one
pokedex["is_legendary"].replace(0, -1, inplace=True)
# Replacing multiple values at one go
pokedex["is_legendary"].replace([0, 1], ["False", "True"], inplace=True)
# Replacing a value with None
pokedex["is_legendary"].replace([0], [None], inplace=True)

# Incrementing by 10 attack, defense and hp
pokedex.loc[["Charizard", "Venusaur", "Blastoise"], ["attack", "defense", "hp"]] += 10

# Updating defense to be increased by the attack, if defense is <= 50
pokedex.loc[pokedex["defense"] <= 50, "defense"] += pokedex[pokedex["defense"] <= 50]["attack"]

# Casting a string value to a number
car_sales['Price'] = car_sales['Price'].str.replace(r'[\$,]', '', regex=True).astype(float).astype(int)

# Not actually changing the column
print(car_sales['Make'].str.lower())

# Deleting a column
car_sales.drop('Total fuel Used (L)', axis=1, inplace=True)

# Apply functions (either python either numpy)  
car_sales['Odometer (KM)'] = car_sales['Odometer (KM)'].apply(lambda x: x / 1.6)

# Create a column from an existing column data  
car_sales['Total Fuel Used (L)'] = car_sales['Odometer (KM)'] / 100 * car_sales['Fuel per 100KM']  
  
# Create a column by a single value / Default 
car_sales['Number of wheels'] = 4

seats_column = pd.Series([5, 5, 5, 5, 5])  
# Adding a new Column with a DataFrame
car_sales['Seats'] = seats_column  
  
car_sales['Seats'].fillna(5, inplace=True)

# Creating a new column from a Python List  
fuel_economy = [7.6, 3, 5, 123, 1, 2, 3, 4, 12, 4]  
  
car_sales['Fuel per 100KM'] = fuel_economy
```

## Reading files

```python
# Reading and creating a DataFrama simultaneously
car_data = pd.read_csv('car-sales.csv')  

accidents = pd.read_table('./dataScience/accidents.csv', sep=',')

# To save to a csv file
car_data.to_csv('exported-car-sales.csv', index=False)

green_tripdata = pd.read_excel('./dataScience/green_tripdata_2015-09.xls')

url = 'https://jsonplaceholder.typicode.com/users'  
  
users = pd.read_json(url)
```

## Dataframe properties

```python
print(car_sales.dtypes)

print(car_sales.info())
  
car_columns = car_sales.columns  

bitcoinData[["Open", "Close", "High", "Low", "Volume"]].corr()
  
print(car_sales.index)

# Math functions for the value fields
print(car_sales.describe())

print(car_sales.info())

# Comparing two columns  
print(pd.crosstab(car_sales['Make'], car_sales['Doors']))
```

## Math Functions

```python
car_prices = pd.Series([3000, 1500, 11250])  
  
print(car_prices.mean())  
  
print(car_sales.sum())

print(car_sales['Doors'].sum())

print(len(car_sales)) # car_sales.shape

# Calculates the average values for all numeric columns within each group
numeric_columns = car_sales.select_dtypes(include=['number']).columns  
grouped_means = car_sales.groupby(['Make'])[numeric_columns].mean()  

print(grouped_means)

# Randomizing the data order
shuffled_car_sales = (car_sales.sample(frac=1))  
  
print(shuffled_car_sales.reset_index(inplace=False, drop=True))

pokemonData["Exp_Points"].max()

pokemonData["Exp_Points"].min()

pokemonData["Exp_Points"].sum()

pokemonData["Exp_Points"].median()

pokemonData["Height(m)"].count()

pokemonData["Male_Pct"].value_counts() # Show the distribution of values as count

pokemonData["Exp_Points"].describe() # Show all of the above (+ a few more)

allPokemonData.describe(include=["object"]) # Descriptive stats for objects
```

## Dealing with missing data

```python
car_sales_missing_data = pd.read_csv('car-sales-missing-data.csv')  
   
# Fill the missing data with the mean value  
car_sales_missing_data['Odometer'].fillna(car_sales_missing_data['Odometer'].mean())

# Reassign the value  
car_sales_missing_data['Odometer'] = car_sales_missing_data['Odometer'].fillna(car_sales_missing_data['Odometer'].mean())

# Automatically reassigns  
car_sales_missing_data['Odometer'].fillna(car_sales_missing_data['Odometer'].mean(), inplace=True)

# Drop the empty data  
car_sales_missing_data.dropna(inplace=True)
```

## Multiplying Matrices

```python
import numpy as np

a = np.array([
    [1, 2, 3],
    [4, 5, 6]
])

b = np.array([
    [1, 2, 3],
    [3, 4, 5],
    [5, 6, 7]
])

c = a.dot(b)

# c[0][0] = 1 (a[0][0]) * 1 (b[0][0]) + 2 (a[0][1]) * 3 (b[1][0]) + 3 (a[0][2]) * 5 (b[2][0])
#         = 1*1 + 2*3 + 3*5 = 1 + 6 + 15 = 22

# c[0][1] = 1 (a[0][0]) * 2 (b[0][1]) + 2 (a[0][1]) * 4 (b[1][1]) + 3 (a[0][2]) * 6 (b[2][1])
#         = 1*2 + 2*4 + 3*6 = 2 + 8 + 18 = 28

# c[0][2] = 1 (a[0][0]) * 3 (b[0][2]) + 2 (a[0][1]) * 5 (b[1][2]) + 3 (a[0][2]) * 7 (b[2][2])
#         = 1*3 + 2*5 + 3*7 = 3 + 10 + 21 = 34

# c[1][0] = 4 (a[1][0]) * 1 (b[0][0]) + 5 (a[1][1]) * 3 (b[1][0]) + 6 (a[1][2]) * 5 (b[2][0])
#         = 4*1 + 5*3 + 6*5 = 4 + 15 + 30 = 49

# c[1][1] = 4 (a[1][0]) * 2 (b[0][1]) + 5 (a[1][1]) * 4 (b[1][1]) + 6 (a[1][2]) * 6 (b[2][1])
#         = 4*2 + 5*4 + 6*6 = 8 + 20 + 36 = 64

# c[1][2] = 4 (a[1][0]) * 3 (b[0][2]) + 5 (a[1][1]) * 5 (b[1][2]) + 6 (a[1][2]) * 7 (b[2][2])
#         = 4*3 + 5*5 + 6*7 = 12 + 25 + 42 = 79

# Final result:
# [
#   [22 28 34]
#   [49 64 79]
# ]

print(c)
```

## Reshaping an Array

```python
import numpy as np  
  
arr = np.array([el + 1 for el in range(10)])  
  
arr = arr.reshape(2, 5)  
# [  
#     [ 1  2  3  4  5]  
#     [ 6  7  8  9 10]  
# ]  

arr = arr.reshape(5, 2)  
# [  
#     [1, 2]  
#     [3, 4]  
#     [5, 6]  
#     [7, 8]  
#     [9, 10]  
# ]  
  
print(arr)
```

## Matrix Transposition

```cpp
import numpy as np  
  
matrix = np.array(  
    [  
        [1, 2, 3, 4, 5],  
        [6, 7, 8, 9, 10]  
    ]  
)  
  
matrix = np.transpose(matrix)  
# [  
#      [ 1  6]  
#      [ 2  7]  
#      [ 3  8]  
#      [ 4  9]  
#      [ 5 10]  
# ]  
  
print(matrix)
```

## Series Methods

```python
import pandas as pd

allPokemonData = pd.read_csv("../AllPokemon.csv")

allPokemonData.abilities.dtype # Method for a Series

allPokemonData.type1.unique() # Unique Values

allPokemonData.type1.nunique() # Number of Unique

pokemonData.nlargest(5, columns=["attack", "weight_kg"]) # Top 5 pokemon with largest attack, then ordered by weight

pokemonData.nsmallest(5, columns=["defense", "hp"]) # Top 5 pokemon with smallest defense, then ordered by hp
```

## Indexes

```python
bitcoinData = pd.read_csv("../csvData/BTC-USD.csv", index_col = 0)

bitcoinData.set_index("Date")

```

----

```python
import pandas as pd

  

spotify2024Data = pd.read_csv("../Most Streamed Spotify Songs 2024.csv", encoding="latin1")

  

# spotify2024Data[["Track", "Artist", "Album Name", "Spotify Streams", "Release Date", "Explicit Track"]].head(100) # First 100 entries

meaningfullSpotifyData = spotify2024Data[["Track", "Artist", "Album Name", "Spotify Streams", "Release Date", "Explicit Track"]]

meaningfullSpotifyData = meaningfullSpotifyData.rename(columns={"Track": "Track Name", "Artist": "Artist Name", "Explicit Track": "Is Track Explicit"})

  

import matplotlib.pyplot as plt

  

# spotify2024Data.shape

# spotify2024Data.columns

# spotify2024Data.dtypes # Types of each col

  

# spotify2024Data["Artist"].unique() # Unique col entries

  

spotify2024Data["Track"] = spotify2024Data["Track"].astype("string") # Cast the Track col to String

  

# spotify2024Data["Track"].str.isnumeric() # returns Bool column

# spotify2024Data[spotify2024Data["Track"].str.isnumeric()] # Filter the entries by that query

# spotify2024Data["Track"].str.isnumeric().value_counts() # returns count of true and count of false

  

spotify2024Data["Artist"] = spotify2024Data["Artist"].astype("string")

  

artist_counts = spotify2024Data["Artist"].value_counts().head(25)

  

plt.figure(figsize=(12, 6))

plt.bar(artist_counts.index, artist_counts.values)

plt.yticks(range(0, 70, 5))

plt.xlabel("Artist Name")

plt.ylabel("Top Songs Count")

plt.grid()

plt.title("Top 25 Artists by Number of Songs")

plt.xticks(rotation=45, ha="right")

plt.tight_layout()

plt.show()
```


```python
import pandas as pd

  

spotify2024Data = pd.read_csv("../Most Streamed Spotify Songs 2024.csv", encoding="latin1")

  

# spotify2024Data[["Track", "Artist", "Album Name", "Spotify Streams", "Release Date", "Explicit Track"]].head(100) # First 100 entries

meaningfullSpotifyData = spotify2024Data[["Track", "Artist", "Album Name", "Spotify Streams", "Release Date", "Explicit Track"]]

meaningfullSpotifyData = meaningfullSpotifyData.rename(columns={"Track": "Track Name", "Artist": "Artist Name", "Explicit Track": "Is Track Explicit"})

  

import matplotlib.pyplot as plt

  

# spotify2024Data.shape

# spotify2024Data.columns

# spotify2024Data.dtypes # Types of each col

  

# spotify2024Data["Artist"].unique() # Unique col entries

  

spotify2024Data["Track"] = spotify2024Data["Track"].astype("string") # Cast the Track col to String

  

# spotify2024Data["Track"].str.isnumeric() # returns Bool column

# spotify2024Data[spotify2024Data["Track"].str.isnumeric()] # Filter the entries by that query

# spotify2024Data["Track"].str.isnumeric().value_counts() # returns count of true and count of false

  

spotify2024Data["Artist"] = spotify2024Data["Artist"].astype("string")

  

artist_counts = spotify2024Data["Artist"].value_counts().head(25)

  

plt.figure(figsize=(12, 6))

plt.bar(artist_counts.index, artist_counts.values)

plt.yticks(range(0, 70, 5))

plt.xlabel("Artist Name")

plt.ylabel("Top Songs Count")

plt.grid()

plt.title("Top 25 Artists by Number of Songs")

plt.xticks(rotation=45, ha="right")

plt.tight_layout()

plt.show()
```