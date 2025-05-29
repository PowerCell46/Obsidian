## Series and DataFrame

Table (Dataframe) 
- May have many dimensions
- usually call this dataset

Dataframe: Columns, Rows, Data
Series: One-dimensional ndarray with axis labels 

List (Series)
- One-dimensional
- usually represents a column of a table

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

colNames = ["name", "attack", "defense", "hp", "description", "type1", "type2"]

# Provide column names (in case they are missing)
pokemonData = pd.read_csv("../data/pokedex.csv", names=colNames)

# Ask Jupiter notebook for information
pd?
pd.read_csv?
```

## Show all table entries

```python
# Set it
pd.set_option("display.max_rows", None)
# Reset it
pd.reset_option("display.max_rows")
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

# Getting all entries with type2 == NaN
pokedex[pokedex["type2"].isna()]
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

# Sort first by Artist ASC, then Rank DESC
spotify[
	spotify["Artist"].isin(["Drake", "Post Malone", "Travis Scott", "Ariana Grande"])
].sort_values(["Artist", "All Time Rank"], ascending=[True, False])

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

# Change index of an entry:
row = netflix[netflix.title == "Evil"]
# Detach it from the dataset
netflix.drop(row.index, inplace=True)
# Attach it back with the new index
netflix.loc["s6666"] = row.iloc[0]

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

# Type casting attack from int to float
pokedex["attack"] = pokedex["attack"].astype("float32")

# 'category': ENUM (saves space, safer)
pokedex["is_legendary"] = pokedex["is_legendary"].astype("category")

# Not actually changing the column
print(car_sales['Make'].str.lower())

# Apply functions (either python either numpy)  
car_sales['Odometer (KM)'] = car_sales['Odometer (KM)'].apply(lambda x: x / 1.6)

# Create a column from an existing column data  
car_sales['Total Fuel Used (L)'] = car_sales['Odometer (KM)'] / 100 * car_sales['Fuel per 100KM']  
  
# Create a column by a single value / Default 
car_sales['Number of wheels'] = 4

# Drop all entries with type2 == NaN
pokedex.dropna(subset=["type2"], inplace=True)

# Drop all entries with ANY NaN value
pokedex.dropna(inplace=True)

# Drop all entries where the WHOLE ROW is empty
pokedex.dropna(how="all", inplace=True)

# Set default value for NaN value of a column
pokedex["type2"] = pokedex["type2"].fillna("None")

# Use a dictionary to specify default value for each column
pokedex = pokedex.fillna({"type2": "None", "percentage_male": -1})

# Fill the missing values with another column's values
sales["shipping_zip"].fillna(sales["billing_zip"])
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

pokedex["generation"].mode().values # Most common value/s ([1]) / ([1, 2, 3])

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

# Find columns with NaN values
pokemonData.columns[pokemonData.isna().any()]

# Fill the missing data with the mean value  
car_sales_missing_data['Odometer'].fillna(car_sales_missing_data['Odometer'].mean())

# Reassign the value  
car_sales_missing_data['Odometer'] = car_sales_missing_data['Odometer'].fillna(car_sales_missing_data['Odometer'].mean())

# Automatically reassigns  
car_sales_missing_data['Odometer'].fillna(car_sales_missing_data['Odometer'].mean(), inplace=True)

# Drop the empty data  
car_sales_missing_data.dropna(inplace=True)
```

## Working with Dates

```python
# Directly specify which cols to be parsed to dates
bitcoinData = pd.read_csv("../csvData/BTC-USD.csv", parse_dates=["Date"])

bitcoinData["Date"] = pd.to_datetime(bitcoinData["Date"])

# Specifying the format (Check the documentation for the syntax)
bitcoinData["Date"] = pd.to_datetime(bitcoinData["Date"], format="%Y-%m-%d")

bitcoinData["Date"].dt.year
bitcoinData["Date"].dt.month
bitcoinData["Date"].dt.day
bitcoinData["Date"].dt.dayofweek
bitcoinData["Date"].dt.hour

# Filtering by year month day
bitcoinData[
        (bitcoinData["Date"].dt.year == 2021) &
        (bitcoinData["Date"].dt.month == 6) &
        (bitcoinData["Date"].dt.day == 29)
    ]

bitcoinData = bitcoinData[
    (bitcoinData["Date"].dt.year > 2020) |
    ((bitcoinData["Date"].dt.month == 6) & (bitcoinData["Date"].dt.day == 29))
]

# Filtering date between years
bitcoinData = bitcoinData[bitcoinData["Date"].dt.year.between(2019, 2020)]
```

## Working with Strings

```python
# Removing last chars of a string
pokedex["classification"] = pokedex["classification"].str[:-8]
# Leaving only the last chars
pokedex["classification"] = pokedex["classification"].str[-8:]

# Leaving only the first chars
pokedex["classification"] = pokedex["classification"].str[:8]
# Removing first chars of a string
pokedex["classification"] = pokedex["classification"].str[:8]

# Upper, Lower and Capitalize methods
pokedex["classification"] = pokedex["classification"].str.upper()
pokedex["classification"] = pokedex["classification"].str.lower()
pokedex["classification"] = pokedex["classification"].str.capitalize()

# Strip (lstrip, rstrip)
pokedex["classification"] = pokedex["classification"].str.strip()

# Split
pokedex["pokemon"] = pokedex["classification"].str.split(" ", expand=True)[1]
pokedex["classification"] = pokedex["classification"].str.split(" ", expand=True)[0]

# Replace
pokedex["classification"] = pokedex["classification"].str.replace(" Pokémon", "")

# Filter by checking if a str contains()
pokedex[pokedex["classification"].str.contains("Seed")]
```

## Aggregation Functions

```python
pokedex.groupby("generation")["name"].count()

aggregate = pokedex.groupby("type1")
aggregate.ngroups # Number of groups
aggregate.groups # Dictinonary of the group key and the indices of the values as a list

# Iterating through the grouping
for key in pokedex.groupby("generation").groups:
    currentBufferLst = []
    for val in pokedex.groupby("generation").groups[key]:
        currentBufferLst.append(val)

    pokemonNames = [pokedex.loc[index]["name"] for index in currentBufferLst]
    resultNames = ", ".join(pokemonNames)
    print(f"Generation {key}: {resultNames}")

typeGroup.first() # Show the first entry
typeGroup.get_group("dragon") # Get a particular group


# Using agg
pokedex.groupby("generation")["sp_attack"].agg(["min", "max" ])

# Multiple columns, specifying what you want for each one
pokedex.groupby("generation")[["attack", "defense", "hp"]].agg({"attack": ["min", "max"], "defense": ["min", "max", "mean", "median"], "hp": ["min", "max", "mean", "median"]})

# Defining and using a custom function
def diffRange(s):
    return s.max() - s.min()

pokedex.groupby("type1")["attack"].agg(["min", "max", diffRange])

# Group by generation, show min max mean for all attk def hp
pokedex\
	.groupby(["generation"])[["attack", "defense", "hp"]]\
	.agg(["min", "max", "mean"])

# Group by multiple columns
pokedex.groupby(["generation", "type1"])["attack"].describe()
```

## Multi-Indexing

```python
pokedex.set_index(["generation", "name"], inplace=True)

pokedex.loc[1, "Moltres"]
pokedex.loc[4, "Garchomp"]
pokedex.loc[6, "Greninja"]

pokedex.set_index(["generation", "pokedex_number"], inplace=True)

pokedex.loc[4, 409] # Access Gen 4, pokedex number 409

pokedex.loc[(4, 409):(5, 530)] # Get all entries between these two

pokedex.loc[:, 720, :] # Search only by the second part of the index
pokedex.xs(357, level="pokedex_number")

pokedex.index.levels
pokedex.index.get_level_values(0) # Array of the first indices
pokedex.index.get_level_values(1) # Array of the second indices

df = pokedex\
    .groupby("classification")\
    .agg({
        "pokedex_number": ["count"],
        "attack": ["min", "max", "mean"],
        "defense": ["min", "max", "mean"]
    })
    
df[("attack", "mean")]

 
pokedex.groupby(["generation", "is_legendary"])["attack"]\
    .mean()\
    .unstack()\
    .plot(kind="bar")
```

## Apply & Map

```python
# Override the values
pokedex["attack"] = pokedex["attack"].apply(lambda attk: attk + avgAttack)

# Create a new col with the new results
pokedex["attack_category"] = pokedex["attack"].apply(defineStrength)

def defineStrength(attack, defense, hp):
    statSum = attack + defense + hp
    return "Strong" if statSum > 250 else ("Medium" if statSum > 150 else "Weak")

# Using the values from all rows
pokedex["attack_category"] = pokedex.apply(lambda row: defineStrength(row["attack"], row["defense"], row["hp"]), axis=1)

generationNames = {
    1: "Kanto",
    2: "Johto",
    3: "Hoenn",
    4: "Sinnoh",
    5: "Unova",
    6: "Kalos",
    7: "Alola",
    8: "Galar"
}
# Mapping values
pokedex["generation"] = pokedex["generation"].map(generationNames)
pokedex["generation"] = pokedex["generation"].map(lambda gen: "Generation " + str(gen))
```

## Concatenating & Merging

```python
series1 = pd.Series(["Ivan", "Peter", "Kristian", "Stilian"])
series2 = pd.Series(["Pavel", "Bogdan", "Ilian", "Gabi"])

# Returns a new series [a1, ..., b1, ...]
pd.concat([series1, series2]).reset_index(drop=True)
pd.concat([series2, series1]).reset_index(drop=True)

# Returns a dataframe [[a1, ...], [b1, ...]]
pd.concat([series1, series2], axis=1)

# Joins by index
people = pd.Series(
    data=["Ivan", "Peter", "Kristian", "Stilian", "Gabi"],
    index=["a", "b", "c", "d", "e"]
)

benchPressPrs = pd.Series(
    data=[100, 137.5, 100, 130],
    index=["a", "b", "c", "d"]
)

pd.concat([people, benchPressPrs], axis=1, join="inner")
pd.concat([people, benchPressPrs], axis=1, join="outer")

# Combine two dataframes with the same columns (data sep in multiple files)
pd.concat([pok1, pok2]).reset_index(drop=True)


generations = pd.DataFrame({
    "generation": [num + 1 for num in range(8)], 
    "generation_name": ["Kanto", "Johto", "Hoenn", "Sinnoh", "Unova", "Kalos", "Alola", "Galar"]}
)

# Join by a common column (DB style join)
pokedex.merge(generations, how="left").sample(2)

# Specify join col, put suffices on same col names
pokedex.merge(generations, how="right", on="generation", suffixes=["_pokemon", "_generation"])
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

plt.tight_layout()
