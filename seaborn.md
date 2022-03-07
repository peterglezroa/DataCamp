# Data visualization with seaborn

### Use Pandas DataFrame
```python
# PANDAS =====================================================================
# pd as universal convetion
import pandas as pd

# Read csv
df = pd.read_csv("masculinity.csv")
# See first 5 rows
df.head()

# Do countplot of the column how_masculine
sns.countplot(x="how_masculine", data=df)
#plt.show()
```

### Relational Plots and Subplots
```python
import seaborn as sns
import matplotlib.pyplot as plt

# From:
#   sns.scatterplot(x="total_bill", y="tip", data=tips)
# To:
# Creates 2 graphs according to the different values in smoker col yes/no
# Displays both in the same row with a max of 3 per row
sns.relplot(x="total_bill", y="tip", data=tips,
  kind="scatter", col="smoker", col_wrap=3)

# Displays both in the same col with a max of 3 per col
sns.relplot(x="total_bill", y="tip", data=tips,
  kind="scatter", row="smoker", row_wrap=3)

# Displays a mixed according to also time of the day
sns.relplot(x="total_bill", y="tip", data=tips,
  kind="scatter", col="smoker", row="time")

plt.show()
```

#### Scatterplot
Each plot point is an independent observation.
```python
height = [62, 64, 69, 75, 66, 68, 65, 71, 76, 73]
weight = [120, 136, 148, 175, 137, 165, 154, 172, 200, 187]

sns.scatterplot(x=height, y=weight)
```

#### Lineplot
Each plot point represents the same "thing" in a range of values, typically
tracker over time.

```python
sns.relplot(x="hour", y="NO_2_mean", data=air_df_mean, kind="line")

# To set markers on the points:
sns.relplot(x="hour", y="NO_2_mean", data=air_df_mean, kind="line",
  markers=True)

# To remove pointed lines
sns.relplot(x="hour", y="NO_2_mean", data=air_df_mean, kind="line",
  dashes=False)

# If multiple values per intervarl, then it will shade it according to the
# confidence interval (95%).
# But also it can be replaced with standard deviation:
sns.relplot(x="hour", y="NO_2_mean", data=air_df, kind="line",
  ci="sd")
```



### Catplot
Used for categorical plots
```python
category_order = ["No answer", "Not at all", "Not very", "Somewhat", "Very"]

sns.catplot(data=masculinity_data, kind="count", x="how_masculine",
  order=category_order)
```

#### Count plots
Involve categorical variables.
```python
sns.catplot(data=masculinity_data, x="how_masculine", kind="count")
```

#### Bar plots
Displays mean of quantitative variable per category
```python
sns.catplot(data=tips, kind="bar", x="day", y="total_bill")

# To remove confidence interval:
sns.catplot(data=tips, kind="bar", x="day", y="total_bill", ci=None)
```

#### Box plot
* Shows the distribution of quantitive data.
* See median, spread, skewness and outliers
```python
sns.catplot(data=tips, kind="box", x="time", y="total_bill")

# By default, the whiskers extend to 1.5* the interquartile range.
# To change the whiskers: whis=[first percentile, second percentile]
sns.catplot(data=tips, kind="box", x="time", y="total_bill", whis=[5, 95])

# To omit outliers: sym=""
sns.catplot(data=tips, kind="box", x="time", y="total_bill", sym="")
```

#### Point plot
* Mean of a quantitative variable
* Vertical lines show 95% confidence intervals.
* Like line plots but work for quantitative values.

```python
sns.catplot(data=masculinity_data, kind="point", x="age",
  y="masculinity_important")

# To use median instead of mean (Better when we have alot of outliers)
from numpy import median

sns.catplot(data=masculinity_data, kind="point", x="age",
  y="masculinity_important", estimator=median)

# To remove the joints between points:
sns.catplot(data=masculinity_data, kind="point", x="age",
  y="masculinity_important", joints=False)
```

### Adding titles and labels

```python
# First learn if the plot is a FacetGrid or a AxesSubplot
g = sns.scatterplot (x="height", y="weight", data=df)
type(g)
# > matplotlib.axes._subplots.AxesSubplot
```

#### FacetGrid
Used for `relplot()` and `catplot()`
```python
g = sns.catplot(x="Region", y="Birthrate", data=df, kind="box")

# Set title
g.fig.suptitle("New Title", y=1.01)

# Set titles to each subplot
g.set_titles("This is {col_name}")

# Set labels
g.set(xlabel="New X Label", ylabel="New Y Label")

# Rotate tick labels 90
plt.xticks(rotation=90)
```

#### AxesSubplot
Used for `scatterplot()`, `countplot()`, etc.
```python
g = sns.boxplot(x="Region", y="Birthrate", data=gdp_data)

# Set title
g.set_title("New Title", y=1.01)
```

### Change plot style
Change the style according to the values of the dataset
```python
# CHANGE FIGURE STYLE
# Preset options: "white", "dark", "whitegrid", "darkgrid", "ticks"
sns.set_style("whitegrid")

# CHANGE PALETTE
# Preset options:
# - "RdBu", red to blue
# - "PRGn", purple to green
# - "RdBu_r", blue to red
# - "PRGn_r", green to purple
# - "Greys", grayscale
# - "Blues", bluescale
sns.set_palette("RdBu")

# CHANGE SCALE
# Preset options (smallest to largest): "paper", "notebook", "talk", "poster"
sns.set_context("notebook")

# CHANGE COLOR
# Colors available: blue, green, red, cyan, magenta, yellow, black, white
# OR USE HEXCODE
hue_colors = { "Yes": "black", "No": "#A23456" }
sns.scatterplot(x="total_bill", y="tip", data=tips, hue="smoker",
  hue_order=["Yes", "No"], palette=hue_colors)

# CHANGE POINT SIZE
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter",
  size="bill_size")

# CHANGE POINT STYLE
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter",
  style="smoker")

# CHANGE TRANSPARENCY
sns.relplot(x="total_bill", y="tip", data=tips, kind="scatter",
  alpha=0.4)
```
