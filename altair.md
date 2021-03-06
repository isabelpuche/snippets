# snippets
Snippets from master in data science class
# Import altair

```python
import altair as alt
# if in notebook
alt.renderers.enable("notebook")
```

# Disable maximum rows
```python
alt.data_transformers.disable_max_rows()
```

# Histogram

```python
alt.Chart(df).mark_bar().encode(
    x=alt.X('Brain Weight',bin=True,title="Brain Weight Binned"),
    y="count()"
).properties(
    width=200,
    height=200,
    title="Histogram of Brain Weight"
)
``` 

# Scatter Plot 

```python
scatter = alt.Chart(df).mark_circle().encode(
    x="Body Weight",
    y="Brain Weight"
)
``` 
# Bar Chart
```python
trends_bar=alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="sum(value)",
    color="search_term"
)
```

# Line Chart
```python
trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
)
```

# Trail

troops_viz = alt.Chart(troops).mark_trail(
    strokeCap="square"
).encode(
    latitude="lat",
    longitude="lon",
    color=alt.Color("direction",scale=alt.Scale(domain=['A', 'R'],range=["#EBD2A8","#888888"])),
    size=alt.Size("survivors",scale=alt.Scale(range=[1,75])),
    detail="division"
).properties(
width=800
)


# Map
```python
countries = alt.topo_feature("https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json",
feature="countries")

background = alt.Chart(countries).mark_geoshape(
fill="lightgrey",
stroke="white"
)

temp = weather_df.groupby("city").mean().reset_index()
cities = alt.Chart(temp).mark_point().encode(
latitude="lat",
longitude="lon",
size=alt.value(1),
color=alt.Color("temp",scale=alt.Scale(range=["red","orange","lightblue"]))
)

background+cities

```

# Horizontal concatenation

```python
trends_bar|trends_line
```

# Vertical concatenation

```python
trends_bar&trends_line
```

# Single selection

```python
select_search_term = alt.selection_single(encodings=["x"])

trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).transform_filter(
    select_search_term
)

trends_bar=alt.Chart(trends).mark_bar().encode(
    x="search_term",
    y="sum(value)",
    color="search_term"
).properties(
    selection=select_search_term
)

trends_bar|trends_line
```

# Interval Selection

```python
selection = alt.selection_interval(encodings=["x"])

trends_line=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).transform_filter(
    selection
)

trends_line_small=alt.Chart(trends).mark_line().encode(
    x=alt.X("date:T",timeUnit="yearmonth"),
    y="sum(value)",
    color="search_term"
).properties(
    height=50,
    selection=selection
)

(trends_line&trends_line_small)
```
