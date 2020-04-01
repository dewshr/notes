# Pandas

- [Pandas](#pandas)
  - [Reading files](#reading-files)
  - [Grouping](#grouping)

## Reading files

Specify header while reading tab-delimited file:

```python
cols = ["col1", "col2", "col3" ]
df = pd.read_csv("data.tab", sep="\t", names=cols)
```


## Grouping

Group by two columns and count total in groups:

```
df.groupby(['col1', 'col2']).count()
```

Accessing an index from a multi-index DataFrame:

```python
pandas.MultiIndex.get_level_values
```
