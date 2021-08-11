

## World Happiness Report

load happiness reports for the years 2015-19


```python
import pandas as pd
```


```python
df_2015 = pd.read_csv('2015.csv')
df_2016 = pd.read_csv('2016.csv')
df_2017 = pd.read_csv('2017.csv')
df_2018 = pd.read_csv('2018.csv')
df_2019 = pd.read_csv('2019.csv')
```

Have a Peek at the data


```python
df_2018.head()
df_2016.head()
```

Checking the size of dataframes for different years


```python
print(df_2015.shape)
print(df_2016.shape)
print(df_2017.shape)
print(df_2018.shape)
print(df_2019.shape)
```

Checking the columns and their name before joining the data frames.


```python
print(sorted(df_2015.columns))
print(sorted(df_2016.columns))
print(sorted(df_2017.columns))
print(sorted(df_2018.columns))
print(sorted(df_2019.columns))              
```

Merge becomes complex as the columns have different names for every year.
Either we have to rename all the columns or creates new dataframes according to the plots required.
Rename all the df columns to a consistent format for joins.


```python
df_2015=df_2015.rename(columns={"Dystopia Residual":"'Dystopia.Residual",'Economy (GDP per Capita)':'Economy','Happiness Rank':'Happiness.Rank','Happiness Score':'Happiness.Score', 'Health (Life Expectancy)':'Health', 'Standard Error':'Standard.Error', 'Trust (Government Corruption)':'Trust'})
df_2016=df_2016.rename(columns={"Dystopia Residual":"'Dystopia.Residual",'Economy (GDP per Capita)':'Economy','Happiness Rank':'Happiness.Rank','Happiness Score':'Happiness.Score', 'Health (Life Expectancy)':'Health', 'Standard Error':'Standard.Error', 'Trust (Government Corruption)':'Trust','Lower Confidence Interval':'Lower.Confidence.Interval','Upper Confidence Interval':'Upper.Confidence.Interval'})
df_2017=df_2017.rename(columns={'Economy..GDP.per.Capita.':'Economy', 'Health..Life.Expectancy.':'Health.Life.Expectancy', 'Trust..Government.Corruption.':'Trust'})
df_2018=df_2018.rename(columns={'Country or region':'Country', 'Freedom to make life choices':'Freedom', 'GDP per capita':'Economy',  'Healthy life expectancy':'Health', 'Overall rank':'Happiness.Rank', 'Perceptions of corruption':'Trust', 'Score':'Happiness.Score', 'Social support':'Family'})
df_2019=df_2019.rename(columns={'Country or region':'Country', 'Freedom to make life choices':'Freedom', 'GDP per capita':'Economy',  'Healthy life expectancy':'Health', 'Overall rank':'Happiness.Rank', 'Perceptions of corruption':'Trust', 'Score':'Happiness.Score', 'Social support':'Family'})
```


```python
df_outer = pd.merge(df_2015, df_2016, on='Country', how='outer',suffixes=["_15","_16"])
df_outer
```

Outer join will have countries that may not have data for all the years. Using inner join to eliminate this problem


```python
df_outer = pd.merge(df_2015, df_2016, on='Country', how='inner',suffixes=["_15","_16"])
df_outer = pd.merge(df_outer, df_2017, on='Country', how='inner',suffixes=["","_17"])
df_outer = pd.merge(df_outer, df_2018, on='Country', how='inner',suffixes=["_17","_18"])
df_outer = pd.merge(df_outer, df_2019, on='Country', how='inner',suffixes=["_17","_19"])
df_outer.columns
```


```python
import numpy as np
import pandas as pd

import plotly.express as px
import plotly.graph_objects as go
from plotly.offline import download_plotlyjs, init_notebook_mode, plot, iplot
import cufflinks as cf

init_notebook_mode(connected=True)
cf.go_offline()
```


```python
px.line(df_outer, x='Country', y=['Happiness.Rank_15', 'Happiness.Rank_16','Happiness.Rank_17','Happiness.Rank_18'], title='Happiness rank for different Years')
```

Finding the happiest country for years 2015-19


```python
print("Happiest Country in 2015: " +df_outer[df_outer['Happiness.Score_15']==df_outer['Happiness.Score_15'].max()].Country)
print("Happiest Country in 2016: " +df_outer[df_outer['Happiness.Score_16']==df_outer['Happiness.Score_16'].max()].Country)
print("Happiest Country in 2017: " +df_outer[df_outer['Happiness.Score_17']==df_outer['Happiness.Score_17'].max()].Country)
print("Happiest Country in 2018: " +df_outer[df_outer['Happiness.Score_18']==df_outer['Happiness.Score_18'].max()].Country)
print("Happiest Country in 2019: " +df_outer[df_outer['Happiness.Score']==df_outer['Happiness.Score'].max()].Country)
```

Correlation between HappinessRank and different columns/factors of the dataframe for the years 2015-19


```python
corr_data = {'2015':df_2015.corr()['Happiness.Rank'],'2016': df_2016.corr()['Happiness.Rank'],'2017':df_2017.corr()['Happiness.Rank'],'2018':df_2018.corr()['Happiness.Rank'],'2019':df_2019.corr()['Happiness.Rank']}
corr_frame = pd.concat(corr_data,axis = 1)
corr_frame
```


```python
import plotly.graph_objects as go

fig = go.Figure(data=go.Heatmap({'z': corr_frame.values.tolist(),
            'x': corr_frame.columns.tolist(),
            'y': corr_frame.index.tolist()}))
fig.show()
```


```python
px.line(df_2019, x='Happiness.Score', y=['Economy','Health','Trust'], title='Happiness score vs. Economy, Health and Trust')
```

Visualizing the distribution of Happiness Score for the year 2015-19


```python
import plotly.figure_factory as ff
variables = [ df_2015['Happiness.Score'],df_2016['Happiness.Score'],df_2017['Happiness.Score'],df_2018['Happiness.Score'],df_2019['Happiness.Score']]
labels = ['2015','2016', '2017','2018','2019']
fig = ff.create_distplot(variables, labels, show_hist=False)
fig.show()

```


```python

fig = px.box(df_2019, y="Happiness.Score",points="all")
fig.show()
```
