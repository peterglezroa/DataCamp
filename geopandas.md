# GeoPandas

### Shapefile components
* map.shp -> Contains the geometry
* map.dbf -> Holds attributes for each geometry
* map.shx -> links the attributes to the geometry
```python
import geopandas as gpd

geo_df = gpd.read_file("map_files/map.shp")
geo_df.head()

# To see geometry of GeoDataFrame
service_district.loc[0, "geometry"]

# To plot
school_districts.plot()

# Or to add colors
school_districts.plot(column = "district", legend=True)

pl.show()
```

### Scatterplots over polygons
```python
# First plot the polygons
school_districts.plot(column = "district", legend = True, cmap="Set2")j

# Add the scatter plot
plt.scatter(schools.lng, schools.lat, maker="p", c="darkgreen")

plt.title("Nashville Schools and School Districts")
plt.show()
```

### Style
```python
# To set color map use cmap
schools.plot(column="distrcit", cmap="Set2", legend=True)

# Legends on the side to better accomodate space

# 3 Columns of all the districts (there are to many)
lkwds = {"title": "District Number", "loc": "upper left",
  "bbox_to_anchor": (1, 1.03), "ncol": 3}
council_dists.plot(column="district", cmap="Set3", legend=True,
  legend_kwds=lkwds)
```

### Dependencies
1. Fiona: Provides an python API for OGR
2. GDAL/OGR:
    1. GDAL for translating raster data (pixel like map)
    2. OGR for translating vector data (vectors like map)

## folium
Map representations with js and using google maps

## Choropleth
Maps with variation of color

```python
# Note that for continues values
districts_with_counts.plot(column="school_density", legend=True)

# To change color
districts_with_counts.plot(column="school_density", cmap="BuGn",
  edgecolor="black", legend=True)
```

### With folium
* `geo_data`: source data for the polygons (geojson file or GeoDataFrame)
* `name`: Name of the geometry column for the polygons
* `data`: source DataFrame or Series
* `columns`: a list of columns that corresponds to the polygons and one that has the value to plot
* `fill_color`: Color palette
* `fill_opacity`: 0-1
* `line_color`: color for borders
* `line_opacity`: 0-1
* `legend_name`: creates a title for the legend
* `key_on`: a GeoJSON variable to bind the data to (always starts with `feature`)

```python
nashville = [36.1636, -86.7823]
m = folium.Map(location=nashville, zoom_start=10)

m.choropleth(
  geo_data=districts_with_counts,
  name="geometry",
  data=districts_with_counts,
  columns=["district", "school_density"],
  key_on="feature.properties.district",
  fill_color="YlGn",
  legend_name="Schools per km squared by School District",
)

folium.LayerControl().add_to(m)
display(m)
```
