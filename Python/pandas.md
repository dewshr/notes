# Pandas

- [Pandas](#pandas)
  - [Reading files](#reading-files)

## Reading files

Specify header while reading tab-delimited file:

```python
cols = ["col1", "col2", "col3" ]
df = pd.read_csv("data.tab", sep="\t", names=cols)
```

