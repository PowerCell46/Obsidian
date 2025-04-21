## Series and DataFrame

Table (Dataframe) 
- May have many dimensions
- usually call this dataset

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


animals = pd.Series(['cat', 'dog', 'snake', 'bird', 'panda', 'human'], index=[0, 3, 9, 8, 3, 2])  
  
# Returns the index and the position of the given number  
print(animals.loc[3])  
  
# Returns only the position of the given number  
print(animals.iloc[3])
```

## Data Selection (similar to SQL Queries)

```python
print(car_sales[car_sales['Make'] == 'Toyota'])  
  
print(car_sales[car_sales['Doors'] > 3])

# Returns DataFrame - Like a SELECT query, specifying the columns  
print(accidents[['Miles from Home', '% of Accidents']])
```

## Changing the column values

```python
# Castign a string value to a number
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
  
car_columns = car_sales.columns  
  
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

print(len(car_sales))

# Calculates the average values for all numeric columns within each group
numeric_columns = car_sales.select_dtypes(include=['number']).columns  
grouped_means = car_sales.groupby(['Make'])[numeric_columns].mean()  

print(grouped_means)

# Randomizing the data order
shuffled_car_sales = (car_sales.sample(frac=1))  
  
print(shuffled_car_sales.reset_index(inplace=False, drop=True))
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
