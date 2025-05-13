
```python
allPokemonData["generation"].value_counts().plot(kind="pie")

allPokemonData["generation"].value_counts().plot(kind="bar")

allPokemonData["attack"].value_counts().plot(kind="pie")
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