## Using Pandas

```python
allPokemonData["generation"].value_counts().plot(kind="pie")

allPokemonData["generation"].value_counts().plot(kind="bar")

allPokemonData["attack"].value_counts().plot(kind="pie")
```

## Matplotlib

```python
arr = np.arange(1, 11, 2)

plt.plot(arr, arr)
plt.figure()

plt.plot(arr, arr**2)
plt.figure()

plt.plot(arr, arr**3)
```

## Styling Matplotlib

```python
# Displaying available plot styles
plt.style.available

# Using a plot style
plt.style.use("fivethirtyeight")
plt.style.use("dark_background")

# Custom Coloring of Lines 
plt.plot(arr, arr ** 1.5, color="#FF00FF")
plt.plot(arr, arr ** 2, color="#0096FF")
plt.plot(arr, arr ** 2.5, color="#FFAC1C")
plt.plot(arr, arr ** 3, color="#4CBB17")

# Specifying Line width
plt.plot(arr, arr ** 2, linewidth=2)
plt.plot(arr, arr ** 2.5, linewidth=3)
plt.plot(arr, arr ** 3, linewidth=4)

# Changing the style of the line
plt.plot(arr, arr ** 2.5, linestyle="dotted")
plt.plot(arr, arr ** 3, linestyle="dashed")

# Setting title to a plot
plt.title("Array Representation", loc="left", fontsize=18, pad=15, color="darkblue")
# X and Y labels
plt.xlabel("X value", labelpad=25)
plt.ylabel("Array value", labelpad=25)

# Specifying the X and Y ticks
plt.xticks([1, 2, 3, 4, 6, 8])
# Adding labels
plt.yticks([25, 100, 200, 300, 450, 600, 750], labels=["25 000", "100k", "200k", "300k", "450k", "600k", "750k"])

# Limit the graph
plt.xlim(2, 6)
plt.ylim(100, 750)

# Adding Legend (USE labels) 
plt.plot(arr, arr ** 1.5, color="grey", label="arr ** 1.5")
plt.plot(arr, arr ** 2, color="olive", label="arr ** 2")
plt.plot(arr, arr ** 3, color="#FF00FF", label="arr ** 3")

plt.grid()
plt.legend(loc="upper left", shadow=True, frameon=True)

# Bar Horizontal Lines 
# ---
# -- 
# -
plt.barh(pokedex["generation"].value_counts().index, pokedex["generation"].value_counts(), color="#C70039")

# Bar Vertical Lines
# |
# | |
# | | |
plt.bar(pokedex["generation"].value_counts().index, pokedex["generation"].value_counts(), color="#C70039")

# Histogram (Distribution of values)
year2016 = bitcoin[bitcoin["Date"].dt.year == 2016]["Close"]
year2017 = bitcoin[bitcoin["Date"].dt.year == 2017]["Close"]
year2018 = bitcoin[bitcoin["Date"].dt.year == 2018]["Close"]

plt.hist(year2016, color="red", label="2016", alpha=0.6)
plt.hist(year2017, color="blue", label="2017", alpha=0.6)
plt.hist(year2018, color="green", label="2018", alpha=0.6)

# Scatter Plot
plt.scatter(pokedex["attack"].tail(10), pokedex["defense"].tail(10), color="purple", marker=".", label="Attack&Defense")

plt.scatter(pokedex["defense"].tail(10), pokedex["hp"].tail(10), color="blue", marker=".", label="Defense&Hp")

# Pie chart
generations = pokedex["generation"].value_counts()
labels = ["Gen " + str(val) for val in generations.index]

max_value = generations.values.max()
explode = [0.1 if val == max_value else 0 for val in generations]

plt.pie(
    generations,
    labels=labels,
    autopct="%1.2f%%",
    explode=explode
)
```

## Seaborn

```python

```


```python
import pandas as pd
import matplotlib.pyplot as plt

pokedex = pd.read_csv("../pokedex.csv", index_col = 0)

femaleLegendaries = pokedex[(pokedex["is_legendary"] == 1) & (pokedex["percentage_male"] == 0)]
maleLegendaries = pokedex[(pokedex["is_legendary"] == 1) & (pokedex["percentage_male"] == 100)]
nonGenderLegendaries = pokedex[(pokedex["is_legendary"] == 1) & (pokedex["percentage_male"].isna())]

genderDistribution = {
    "male": maleLegendaries.shape[0],
    "female": femaleLegendaries.shape[0],
    "non-gender": nonGenderLegendaries.shape[0]
}

plt.xlabel("Legendary gender")
plt.ylabel("Count")

plt.bar(genderDistribution.keys(), genderDistribution.values())
```