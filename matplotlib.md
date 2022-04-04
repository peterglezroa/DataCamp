# Matplotlib

### Add data
* fig -> Container that holds everything that you see in the page.
* axis -> The canvas where we draw our data
```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots()

# Add one series
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL"])

# Add second series
ax.plot(austin_weather["MONTH"], austin_weather["MLY-TAVG-NORMAL"])

plt.show()
```

### Subplots
```python
# Grid of 3 rows and 2 cols
fig, ax = plt.subplots(3, 2)

ax[0, 0].plot(seattle_weather["MONTH"], seattle_weather["MLY-PRCP-NORM"])

# To make all plots have the same range of values
fig, ax = plt.subplots(3, 2, sharey=True)
```

### Time-series
```python
climate_change = pd.read_csv("climate_change.csv", parse_dates=["date"],
  index_col="date")

# Considering dates strings are indexed in the pandas df
ax.plot(climate_change.index, climate_change["co2"])
ax.set_xlabel("Time")
ax.set_ylabel("CO2 (ppm)")
plt.show()

# Zooming on a date
sixties = climate_change["1960-01-01":"1969-12-31"]
ax.plot(sixties.index, sixties["co2"])
ax.set_xlabel("Time")
ax.set_ylabel("CO2 (ppm)")
plt.show()
```

#### Using twin axes
Have 2 different y axis when the values are very different of each var
```python
ax.plot(climate_change.index, climate_change["co2"], color="blue")
ax.set_xlabel("Time", color="blue")
ax.set_ylabel("CO2 (ppm)")
ax2 = ax.twinx()
ax2.plot(climate_change.index, climate_change["relative_temp"], color="red")
ax2.set_ylabel("Relative temperature (Celsius)", color="red")
plt.show()
```

#### Add annotations
```python
ax2.annotate(">1 degree",
  xy=(pd.Timestamp("2015-10-06"), 1), # What we are annotating x = pd... y=1
  xytext=(pd.Timestamp("2008-10-06"), -0.2) # Where to display the text
  arrowprops={
      "arrowstyle": "->",
      "color": "gray"
  }) # Add an arrow that connects both points
```

### Bar Charts
```python
fig, ax = plt.subplots()
ax.bar(medals.index, medals["Gold"], label="Gold")
# Add bronze and silver medals
ax.bar(medals.index, medals["Silver"], bottom=medals["Gold"], label="Silver")
ax.bar(medals.index, medals["Bronze"], bottom=medals["Gold"]+medals["Silver"],
  label="Bronze")

# Rotate ticks to avoid overlapping
ax.set_xticklabels(medals.index, rotation=90)
ax.set_ylabel("Number of medals")

ax.legend() # Show labels
plt.show()

# To add error bars
ax.bar("Rowing", mens_rowing["Height"].mean(),yerr=mens_rowing["Height"].std())
```

### Histograms
```python
ax.hist(mens_rowing["Height"], label="Rowing")
ax.hist(mens_gymnastic["Height"], label="Gymnastic")
ax.set_xlabel("Height (cm)")
ax.set_ylabel("# of observations")
ax.legend()
plt.show()

# To change the default amount of bars (10):
ax.hist(mens_rowing["Height"], label="Rowing", bins=5)
# Or use certain range of values for the bars
ax.hist(mens_rowing["Height"], label="Rowing",
  bins=[150, 160, 170, 180, 190, 200, 210])

# To make them transparent: histtype
ax.hist(mens_rowing["Height"], label="Rowing", histtype="step")
```

### Boxplots
```python
ax.boxplot([mens_rowing["Height"], mens_gymnastics["Height"]])
ax.set_xticklabels(["Rowing", "Gymnastics"])
ax.set_ylabel("Height (cm)")
plt.show()
```

### Scatter Plots
```python
ax.scatter(climate_change["co2"], climate_change["relative_temp"])
ax.set_xlabel("CO2")
ax.set_ylabel("Relative temperature (Celsius)")
plt.show()

# For color to change according to the continous index
ax.scatter(climate_change["co2"], climate_change["relative_temp"],
  c=climate_change.index)
```

### Customizing data appearance
```python
# Set markers as triangles
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL"],
  marker="v")

# Set pointed line
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL"],
  linestyle="--")

# Remove line
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL"],
  linestyle="None")

# Choose a color
ax.plot(seattle_weather["MONTH"], seattle_weather["MLY-TAVG-NORMAL"],
  color=red)

# Add labels
ax.set_xlabel("Time (months)")
ax.set_ylabel("Avg temperature (Farenheit)")

# Set title
ax.set_title("Weather patterns in Austin and Seattle")
```

### Save visualizations
```python
fig, ax = plt.subplots()
ax.bar(medals.index, medals["Gold"]

# Set a size to the figure
fig.set_size_inches([5, 3]) # Width 5, Height 3

# Formats:
#   - png: High quality
#   - jpg: Compressed
#   - svg: vectors
fig.savefig("gold_medals.png")
```

### Plot pair of data into multiple plots
```python
sns.pairplot(data=df)
```

### Check later
* [3d visualizations](https://matplotlib.org/mpl_toolkits/mplot3d/tutorial.html)
* [images] (https://matplotlib.org/users/image_tutorial.html)
* [With animation](https://matplotlib.org/api/animation_api.html)
