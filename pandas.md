
#### Explore data
```python
# Print the first rows of the dataset
df.head()

# See more information about the dataset per column
df.describe(percentiles=[0.5], include="all")

# Plots each column of the dataset for fast exploration
pd.plotting.scatter_matrix(pollution, alpha=0.2)
```
