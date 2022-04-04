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

* You can use **beeswarm plot** to see where the datapoints land
```python
sns.catplot(data=tips, kind="box", x="time", y="total_bill")

# By default, the whiskers extend to 1.5* the interquartile range.
# To change the whiskers: whis=[first percentile, second percentile]
sns.catplot(data=tips, kind="box", x="time", y="total_bill", whis=[5, 95])

# To omit outliers: sym=""
sns.catplot(data=tips, kind="box", x="time", y="total_bill", sym="")
```

#### BeeSwarm plot
```python
pollution_nov = pollution[pollution.month == 10]

sns.swarmplot(y="city", x="O3", data=pollution_nov, size=4)
plt.xlabel("Ozone (O3)")
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

#### KDE plots
* Values over defined number of intervals
```python
# Filter dataset to the year 2012
sns.kdeplot(pollution[pollution.year == 2012].O3, 
            # Shade under kde and add a helpful label
            shade = True,
            label = '2012')

# Filter dataset to everything except the year 2012
sns.kdeplot(pollution[pollution.year != 2012].O3, 
            # Again, shade under kde and add a helpful label
            shade = True,
            label = 'other years')
plt.show()
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

# Or Define a custom continuous color palette with as_cmap
# Also: dark_palette, or color_pallete("red:blue")
color_palette = sns.light_palette('orangered', as_cmap = True)
sns.scatterplot(x = 'CO', y = 'NO2', hue = 'O3', data = cinci_2014,
  palette = color_palette)

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

# REMOVE BORDERS
sns.despine(top=True, right=True, left=True, bottom=True)

# CHANGING FONT SIZE
sns.set(font_scale = 2)
```

### Hightlight an specific point
```python
houston_pollution = pollution[pollution.city  ==  'Houston'].copy()

# Find the highest observed O3 value
max_O3 = houston_pollution.O3.max()

# Make a column that denotes which day had highest O3
houston_pollution["point_type"] = ['Highest O3 Day' if O3  ==  max_O3 else 'Others' for O3 in houston_pollution.O3]

# Encode the hue of the points with the O3 generated column
sns.scatterplot(x = 'NO2',
                y = 'SO2',
                hue = "point_type",
                data = houston_pollution)
plt.show()

# Or multiple points
# Make a vector where Long Beach is orangered; else lightgray
is_lb = ['orangered' if city  ==  'Long Beach' else 'lightgray' for city in pollution['city']]

# Map facecolors to the list is_lb and set alpha to 0.3
sns.regplot(x = 'CO',
            y = 'O3',
            data = pollution,
            fit_reg = False, 
            scatter_kws = {'facecolors': is_lb, "alpha": 0.3})
plt.show()
```

### Annotations

#### Text
```python
sns.scatterplot(x="NO2", y="SO2", data=houston_pollution)

# X: 13, Y: 33
plt.text(13, 33, "Look at this outlier",
  fontdict={"ha": "left", "size": "x-large"})
```

#### Text with arrow
```python
sns.scatterplot(x="NO2", y="SO2", data=houston_pollution)

plt.annotate("A buried point to look at", xy=(45.5, 11), xytext=(60, 22),
  arrowprops={"facecolor": "grey", "width": 3}, backgroundcolor="white")
```

### Multiple subplots
```python
# Create a subplot of one row & two cols
f, (ax1, ax2) = plt.subplots(1,2)

sns.lineplot("month", "NO2", "year", ax=ax1, data=pol_by_month, palette="RdBu", ax = ax1)
sns.barplot("year", "count", ax=ax2, data=obs_by_year, palette="RdBu", ax = ax2)

# To remove legends:
ax1.legend_.remove()
ax2.legend_.remove()
```
