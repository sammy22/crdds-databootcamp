
# World Happiness Report

Required Libraries for the script


```python
# !pip install bs4
# !pip install numpy
# !pip install pandas
# !pip install plotly
# !pip install cufflinks
```

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




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Region</th>
      <th>Happiness Rank</th>
      <th>Happiness Score</th>
      <th>Lower Confidence Interval</th>
      <th>Upper Confidence Interval</th>
      <th>Economy (GDP per Capita)</th>
      <th>Family</th>
      <th>Health (Life Expectancy)</th>
      <th>Freedom</th>
      <th>Trust (Government Corruption)</th>
      <th>Generosity</th>
      <th>Dystopia Residual</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Denmark</td>
      <td>Western Europe</td>
      <td>1</td>
      <td>7.526</td>
      <td>7.460</td>
      <td>7.592</td>
      <td>1.44178</td>
      <td>1.16374</td>
      <td>0.79504</td>
      <td>0.57941</td>
      <td>0.44453</td>
      <td>0.36171</td>
      <td>2.73939</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Switzerland</td>
      <td>Western Europe</td>
      <td>2</td>
      <td>7.509</td>
      <td>7.428</td>
      <td>7.590</td>
      <td>1.52733</td>
      <td>1.14524</td>
      <td>0.86303</td>
      <td>0.58557</td>
      <td>0.41203</td>
      <td>0.28083</td>
      <td>2.69463</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Iceland</td>
      <td>Western Europe</td>
      <td>3</td>
      <td>7.501</td>
      <td>7.333</td>
      <td>7.669</td>
      <td>1.42666</td>
      <td>1.18326</td>
      <td>0.86733</td>
      <td>0.56624</td>
      <td>0.14975</td>
      <td>0.47678</td>
      <td>2.83137</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Norway</td>
      <td>Western Europe</td>
      <td>4</td>
      <td>7.498</td>
      <td>7.421</td>
      <td>7.575</td>
      <td>1.57744</td>
      <td>1.12690</td>
      <td>0.79579</td>
      <td>0.59609</td>
      <td>0.35776</td>
      <td>0.37895</td>
      <td>2.66465</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Finland</td>
      <td>Western Europe</td>
      <td>5</td>
      <td>7.413</td>
      <td>7.351</td>
      <td>7.475</td>
      <td>1.40598</td>
      <td>1.13464</td>
      <td>0.81091</td>
      <td>0.57104</td>
      <td>0.41004</td>
      <td>0.25492</td>
      <td>2.82596</td>
    </tr>
  </tbody>
</table>
</div>



Checking the size of dataframes for different years


```python
print(df_2015.shape)
print(df_2016.shape)
print(df_2017.shape)
print(df_2018.shape)
print(df_2019.shape)
```

    (158, 12)
    (157, 13)
    (155, 12)
    (156, 9)
    (156, 9)


Checking the columns and their name before joining the data frames.


```python
print(sorted(df_2015.columns))
print(sorted(df_2016.columns))
print(sorted(df_2017.columns))
print(sorted(df_2018.columns))
print(sorted(df_2019.columns))              
```

    ['Country', 'Dystopia Residual', 'Economy (GDP per Capita)', 'Family', 'Freedom', 'Generosity', 'Happiness Rank', 'Happiness Score', 'Health (Life Expectancy)', 'Region', 'Standard Error', 'Trust (Government Corruption)']
    ['Country', 'Dystopia Residual', 'Economy (GDP per Capita)', 'Family', 'Freedom', 'Generosity', 'Happiness Rank', 'Happiness Score', 'Health (Life Expectancy)', 'Lower Confidence Interval', 'Region', 'Trust (Government Corruption)', 'Upper Confidence Interval']
    ['Country', 'Dystopia.Residual', 'Economy..GDP.per.Capita.', 'Family', 'Freedom', 'Generosity', 'Happiness.Rank', 'Happiness.Score', 'Health..Life.Expectancy.', 'Trust..Government.Corruption.', 'Whisker.high', 'Whisker.low']
    ['Country or region', 'Freedom to make life choices', 'GDP per capita', 'Generosity', 'Healthy life expectancy', 'Overall rank', 'Perceptions of corruption', 'Score', 'Social support']
    ['Country or region', 'Freedom to make life choices', 'GDP per capita', 'Generosity', 'Healthy life expectancy', 'Overall rank', 'Perceptions of corruption', 'Score', 'Social support']


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




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Region_15</th>
      <th>Happiness.Rank_15</th>
      <th>Happiness.Score_15</th>
      <th>Standard.Error</th>
      <th>Economy_15</th>
      <th>Family_15</th>
      <th>Health_15</th>
      <th>Freedom_15</th>
      <th>Trust_15</th>
      <th>...</th>
      <th>Happiness.Score_16</th>
      <th>Lower.Confidence.Interval</th>
      <th>Upper.Confidence.Interval</th>
      <th>Economy_16</th>
      <th>Family_16</th>
      <th>Health_16</th>
      <th>Freedom_16</th>
      <th>Trust_16</th>
      <th>Generosity_16</th>
      <th>'Dystopia.Residual_16</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Switzerland</td>
      <td>Western Europe</td>
      <td>1.0</td>
      <td>7.587</td>
      <td>0.03411</td>
      <td>1.39651</td>
      <td>1.34951</td>
      <td>0.94143</td>
      <td>0.66557</td>
      <td>0.41978</td>
      <td>...</td>
      <td>7.509</td>
      <td>7.428</td>
      <td>7.590</td>
      <td>1.52733</td>
      <td>1.14524</td>
      <td>0.86303</td>
      <td>0.58557</td>
      <td>0.41203</td>
      <td>0.28083</td>
      <td>2.69463</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Iceland</td>
      <td>Western Europe</td>
      <td>2.0</td>
      <td>7.561</td>
      <td>0.04884</td>
      <td>1.30232</td>
      <td>1.40223</td>
      <td>0.94784</td>
      <td>0.62877</td>
      <td>0.14145</td>
      <td>...</td>
      <td>7.501</td>
      <td>7.333</td>
      <td>7.669</td>
      <td>1.42666</td>
      <td>1.18326</td>
      <td>0.86733</td>
      <td>0.56624</td>
      <td>0.14975</td>
      <td>0.47678</td>
      <td>2.83137</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Denmark</td>
      <td>Western Europe</td>
      <td>3.0</td>
      <td>7.527</td>
      <td>0.03328</td>
      <td>1.32548</td>
      <td>1.36058</td>
      <td>0.87464</td>
      <td>0.64938</td>
      <td>0.48357</td>
      <td>...</td>
      <td>7.526</td>
      <td>7.460</td>
      <td>7.592</td>
      <td>1.44178</td>
      <td>1.16374</td>
      <td>0.79504</td>
      <td>0.57941</td>
      <td>0.44453</td>
      <td>0.36171</td>
      <td>2.73939</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Norway</td>
      <td>Western Europe</td>
      <td>4.0</td>
      <td>7.522</td>
      <td>0.03880</td>
      <td>1.45900</td>
      <td>1.33095</td>
      <td>0.88521</td>
      <td>0.66973</td>
      <td>0.36503</td>
      <td>...</td>
      <td>7.498</td>
      <td>7.421</td>
      <td>7.575</td>
      <td>1.57744</td>
      <td>1.12690</td>
      <td>0.79579</td>
      <td>0.59609</td>
      <td>0.35776</td>
      <td>0.37895</td>
      <td>2.66465</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Canada</td>
      <td>North America</td>
      <td>5.0</td>
      <td>7.427</td>
      <td>0.03553</td>
      <td>1.32629</td>
      <td>1.32261</td>
      <td>0.90563</td>
      <td>0.63297</td>
      <td>0.32957</td>
      <td>...</td>
      <td>7.404</td>
      <td>7.335</td>
      <td>7.473</td>
      <td>1.44015</td>
      <td>1.09610</td>
      <td>0.82760</td>
      <td>0.57370</td>
      <td>0.31329</td>
      <td>0.44834</td>
      <td>2.70485</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>159</th>
      <td>Belize</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.956</td>
      <td>5.710</td>
      <td>6.202</td>
      <td>0.87616</td>
      <td>0.68655</td>
      <td>0.45569</td>
      <td>0.51231</td>
      <td>0.10771</td>
      <td>0.23684</td>
      <td>3.08039</td>
    </tr>
    <tr>
      <th>160</th>
      <td>Somalia</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.440</td>
      <td>5.321</td>
      <td>5.559</td>
      <td>0.00000</td>
      <td>0.33613</td>
      <td>0.11466</td>
      <td>0.56778</td>
      <td>0.31180</td>
      <td>0.27225</td>
      <td>3.83772</td>
    </tr>
    <tr>
      <th>161</th>
      <td>Somaliland Region</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>5.057</td>
      <td>4.934</td>
      <td>5.180</td>
      <td>0.25558</td>
      <td>0.75862</td>
      <td>0.33108</td>
      <td>0.39130</td>
      <td>0.36794</td>
      <td>0.51479</td>
      <td>2.43801</td>
    </tr>
    <tr>
      <th>162</th>
      <td>Namibia</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>4.574</td>
      <td>4.374</td>
      <td>4.774</td>
      <td>0.93287</td>
      <td>0.70362</td>
      <td>0.34745</td>
      <td>0.48614</td>
      <td>0.10398</td>
      <td>0.07795</td>
      <td>1.92198</td>
    </tr>
    <tr>
      <th>163</th>
      <td>South Sudan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>3.832</td>
      <td>3.596</td>
      <td>4.068</td>
      <td>0.39394</td>
      <td>0.18519</td>
      <td>0.15781</td>
      <td>0.19662</td>
      <td>0.13015</td>
      <td>0.25899</td>
      <td>2.50929</td>
    </tr>
  </tbody>
</table>
<p>164 rows ?? 24 columns</p>
</div>



Outer join will have countries that may not have data for all the years. Using inner join to eliminate this problem


```python
df_outer = pd.merge(df_2015, df_2016, on='Country', how='inner',suffixes=["_15","_16"])
df_outer = pd.merge(df_outer, df_2017, on='Country', how='inner',suffixes=["","_17"])
df_outer = pd.merge(df_outer, df_2018, on='Country', how='inner',suffixes=["_17","_18"])
df_outer = pd.merge(df_outer, df_2019, on='Country', how='inner',suffixes=["_17","_19"])
df_outer.columns
```




    Index(['Country', 'Region_15', 'Happiness.Rank_15', 'Happiness.Score_15',
           'Standard.Error', 'Economy_15', 'Family_15', 'Health_15', 'Freedom_15',
           'Trust_15', 'Generosity_15', ''Dystopia.Residual_15', 'Region_16',
           'Happiness.Rank_16', 'Happiness.Score_16', 'Lower.Confidence.Interval',
           'Upper.Confidence.Interval', 'Economy_16', 'Family_16', 'Health_16',
           'Freedom_16', 'Trust_16', 'Generosity_16', ''Dystopia.Residual_16',
           'Happiness.Rank_17', 'Happiness.Score_17', 'Whisker.high',
           'Whisker.low', 'Economy_17', 'Family_17', 'Health.Life.Expectancy',
           'Freedom_17', 'Generosity_17', 'Trust_17', 'Dystopia.Residual',
           'Happiness.Rank_18', 'Happiness.Score_18', 'Economy_18', 'Family_18',
           'Health_17', 'Freedom_18', 'Generosity_18', 'Trust_18',
           'Happiness.Rank', 'Happiness.Score', 'Economy', 'Family', 'Health_19',
           'Freedom', 'Generosity', 'Trust'],
          dtype='object')



Statistical summary of the columns for year 2015-19


```python
df_outer.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Happiness.Rank_15</th>
      <th>Happiness.Score_15</th>
      <th>Standard.Error</th>
      <th>Economy_15</th>
      <th>Family_15</th>
      <th>Health_15</th>
      <th>Freedom_15</th>
      <th>Trust_15</th>
      <th>Generosity_15</th>
      <th>'Dystopia.Residual_15</th>
      <th>...</th>
      <th>Generosity_18</th>
      <th>Trust_18</th>
      <th>Happiness.Rank</th>
      <th>Happiness.Score</th>
      <th>Economy</th>
      <th>Family</th>
      <th>Health_19</th>
      <th>Freedom</th>
      <th>Generosity</th>
      <th>Trust</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>...</td>
      <td>141.000000</td>
      <td>140.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
      <td>141.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>78.276596</td>
      <td>5.406716</td>
      <td>0.045473</td>
      <td>0.860514</td>
      <td>1.000370</td>
      <td>0.648851</td>
      <td>0.430532</td>
      <td>0.140460</td>
      <td>0.235838</td>
      <td>2.090167</td>
      <td>...</td>
      <td>0.181830</td>
      <td>0.111429</td>
      <td>75.553191</td>
      <td>5.485383</td>
      <td>0.928078</td>
      <td>1.228574</td>
      <td>0.746965</td>
      <td>0.395511</td>
      <td>0.183241</td>
      <td>0.109440</td>
    </tr>
    <tr>
      <th>std</th>
      <td>46.535410</td>
      <td>1.169844</td>
      <td>0.015110</td>
      <td>0.395357</td>
      <td>0.271197</td>
      <td>0.231794</td>
      <td>0.149151</td>
      <td>0.120327</td>
      <td>0.129458</td>
      <td>0.552971</td>
      <td>...</td>
      <td>0.101922</td>
      <td>0.098181</td>
      <td>44.750966</td>
      <td>1.096099</td>
      <td>0.381611</td>
      <td>0.282676</td>
      <td>0.219697</td>
      <td>0.142888</td>
      <td>0.097749</td>
      <td>0.096107</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>2.839000</td>
      <td>0.018480</td>
      <td>0.000000</td>
      <td>0.139950</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.328580</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>3.203000</td>
      <td>0.046000</td>
      <td>0.378000</td>
      <td>0.192000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>37.000000</td>
      <td>4.518000</td>
      <td>0.036330</td>
      <td>0.594480</td>
      <td>0.855630</td>
      <td>0.515290</td>
      <td>0.328780</td>
      <td>0.059890</td>
      <td>0.140740</td>
      <td>1.758730</td>
      <td>...</td>
      <td>0.106000</td>
      <td>0.050000</td>
      <td>37.000000</td>
      <td>4.628000</td>
      <td>0.657000</td>
      <td>1.098000</td>
      <td>0.581000</td>
      <td>0.305000</td>
      <td>0.108000</td>
      <td>0.047000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>77.000000</td>
      <td>5.286000</td>
      <td>0.043170</td>
      <td>0.920490</td>
      <td>1.035160</td>
      <td>0.708060</td>
      <td>0.434770</td>
      <td>0.105010</td>
      <td>0.214880</td>
      <td>2.085280</td>
      <td>...</td>
      <td>0.172000</td>
      <td>0.081500</td>
      <td>74.000000</td>
      <td>5.467000</td>
      <td>0.987000</td>
      <td>1.303000</td>
      <td>0.802000</td>
      <td>0.426000</td>
      <td>0.175000</td>
      <td>0.082000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>119.000000</td>
      <td>6.302000</td>
      <td>0.050150</td>
      <td>1.154060</td>
      <td>1.227910</td>
      <td>0.813250</td>
      <td>0.546040</td>
      <td>0.175210</td>
      <td>0.311050</td>
      <td>2.451760</td>
      <td>...</td>
      <td>0.245000</td>
      <td>0.135250</td>
      <td>114.000000</td>
      <td>6.199000</td>
      <td>1.237000</td>
      <td>1.457000</td>
      <td>0.884000</td>
      <td>0.508000</td>
      <td>0.247000</td>
      <td>0.140000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>158.000000</td>
      <td>7.587000</td>
      <td>0.136930</td>
      <td>1.690420</td>
      <td>1.402230</td>
      <td>1.025250</td>
      <td>0.669730</td>
      <td>0.551910</td>
      <td>0.795880</td>
      <td>3.602140</td>
      <td>...</td>
      <td>0.598000</td>
      <td>0.457000</td>
      <td>154.000000</td>
      <td>7.769000</td>
      <td>1.684000</td>
      <td>1.624000</td>
      <td>1.141000</td>
      <td>0.631000</td>
      <td>0.566000</td>
      <td>0.453000</td>
    </tr>
  </tbody>
</table>
<p>8 rows ?? 48 columns</p>
</div>



Comparing the mean happiness score for different years


```python
y_vars = [ df_2015['Happiness.Score'].mean(),df_2016['Happiness.Score'].mean(),df_2017['Happiness.Score'].mean(),df_2018['Happiness.Score'].mean(),df_2019['Happiness.Score'].mean()]
x_vars = ['2015','2016', '2017','2018','2019']
fig =px.line(df_outer, x=x_vars, y=y_vars, title='Avreage Happiness Score for different Years')
fig.update_xaxes(title_text='Years')
fig.update_yaxes(title_text='Mean Score')
fig.show()
```


<div>                            <div id="da01aa2a-d939-4ea6-abc7-255deb928760" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("da01aa2a-d939-4ea6-abc7-255deb928760")) {                    Plotly.newPlot(                        "da01aa2a-d939-4ea6-abc7-255deb928760",                        [{"hovertemplate":"x=%{x}<br>y=%{y}<extra></extra>","legendgroup":"","line":{"color":"#636efa","dash":"solid"},"mode":"lines","name":"","orientation":"v","showlegend":false,"type":"scatter","x":["2015","2016","2017","2018","2019"],"xaxis":"x","y":[5.3757341772151905,5.382184713375795,5.354019355773926,5.375916666666668,5.407096153846153],"yaxis":"y"}],                        {"legend":{"tracegroupgap":0},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"title":{"text":"Avreage Happiness Score for different Years"},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Years"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Mean Score"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('da01aa2a-d939-4ea6-abc7-255deb928760');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



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


<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.2.0.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




<script type="text/javascript">
window.PlotlyConfig = {MathJaxConfig: 'local'};
if (window.MathJax) {MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
if (typeof require !== 'undefined') {
require.undef("plotly");
requirejs.config({
    paths: {
        'plotly': ['https://cdn.plot.ly/plotly-2.2.0.min']
    }
});
require(['plotly'], function(Plotly) {
    window._Plotly = Plotly;
});
}
</script>




```python
px.line(df_outer, x='Country', y=['Happiness.Rank_15', 'Happiness.Rank_16','Happiness.Rank_17','Happiness.Rank_18'], title='Happiness rank for different Years')
```


<div>                            <div id="646a8542-fd9f-4e58-bb19-58a0d1ba3608" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("646a8542-fd9f-4e58-bb19-58a0d1ba3608")) {                    Plotly.newPlot(                        "646a8542-fd9f-4e58-bb19-58a0d1ba3608",                        [{"hovertemplate":"variable=Happiness.Rank_15<br>Country=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Happiness.Rank_15","line":{"color":"#636efa","dash":"solid"},"mode":"lines","name":"Happiness.Rank_15","orientation":"v","showlegend":true,"type":"scatter","x":["Switzerland","Iceland","Denmark","Norway","Canada","Finland","Netherlands","Sweden","New Zealand","Australia","Israel","Costa Rica","Austria","Mexico","United States","Brazil","Luxembourg","Ireland","Belgium","United Arab Emirates","United Kingdom","Venezuela","Singapore","Panama","Germany","Chile","Qatar","France","Argentina","Czech Republic","Uruguay","Colombia","Thailand","Saudi Arabia","Spain","Malta","Kuwait","El Salvador","Guatemala","Uzbekistan","Slovakia","Japan","South Korea","Ecuador","Bahrain","Italy","Bolivia","Moldova","Paraguay","Kazakhstan","Slovenia","Lithuania","Nicaragua","Peru","Belarus","Poland","Malaysia","Croatia","Libya","Russia","Jamaica","Cyprus","Algeria","Kosovo","Turkmenistan","Mauritius","Estonia","Indonesia","Vietnam","Turkey","Kyrgyzstan","Nigeria","Bhutan","Azerbaijan","Pakistan","Jordan","Montenegro","China","Zambia","Romania","Serbia","Portugal","Latvia","Philippines","Morocco","Albania","Bosnia and Herzegovina","Dominican Republic","Mongolia","Greece","Lebanon","Hungary","Honduras","Tajikistan","Tunisia","Palestinian Territories","Bangladesh","Iran","Ukraine","Iraq","South Africa","Ghana","Zimbabwe","Liberia","India","Haiti","Congo (Kinshasa)","Nepal","Ethiopia","Sierra Leone","Mauritania","Kenya","Armenia","Botswana","Myanmar","Georgia","Malawi","Sri Lanka","Cameroon","Bulgaria","Egypt","Yemen","Mali","Congo (Brazzaville)","Uganda","Senegal","Gabon","Niger","Cambodia","Tanzania","Madagascar","Chad","Guinea","Ivory Coast","Burkina Faso","Afghanistan","Rwanda","Benin","Syria","Burundi","Togo"],"xaxis":"x","y":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,39,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,67,68,69,70,71,73,74,75,76,77,78,79,80,81,82,82,84,85,86,87,88,89,90,92,95,96,98,100,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,119,120,121,122,123,124,125,127,128,129,130,131,132,133,134,135,136,138,139,141,142,143,144,145,146,147,149,150,151,152,153,154,155,156,157,158],"yaxis":"y"},{"hovertemplate":"variable=Happiness.Rank_16<br>Country=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Happiness.Rank_16","line":{"color":"#EF553B","dash":"solid"},"mode":"lines","name":"Happiness.Rank_16","orientation":"v","showlegend":true,"type":"scatter","x":["Switzerland","Iceland","Denmark","Norway","Canada","Finland","Netherlands","Sweden","New Zealand","Australia","Israel","Costa Rica","Austria","Mexico","United States","Brazil","Luxembourg","Ireland","Belgium","United Arab Emirates","United Kingdom","Venezuela","Singapore","Panama","Germany","Chile","Qatar","France","Argentina","Czech Republic","Uruguay","Colombia","Thailand","Saudi Arabia","Spain","Malta","Kuwait","El Salvador","Guatemala","Uzbekistan","Slovakia","Japan","South Korea","Ecuador","Bahrain","Italy","Bolivia","Moldova","Paraguay","Kazakhstan","Slovenia","Lithuania","Nicaragua","Peru","Belarus","Poland","Malaysia","Croatia","Libya","Russia","Jamaica","Cyprus","Algeria","Kosovo","Turkmenistan","Mauritius","Estonia","Indonesia","Vietnam","Turkey","Kyrgyzstan","Nigeria","Bhutan","Azerbaijan","Pakistan","Jordan","Montenegro","China","Zambia","Romania","Serbia","Portugal","Latvia","Philippines","Morocco","Albania","Bosnia and Herzegovina","Dominican Republic","Mongolia","Greece","Lebanon","Hungary","Honduras","Tajikistan","Tunisia","Palestinian Territories","Bangladesh","Iran","Ukraine","Iraq","South Africa","Ghana","Zimbabwe","Liberia","India","Haiti","Congo (Kinshasa)","Nepal","Ethiopia","Sierra Leone","Mauritania","Kenya","Armenia","Botswana","Myanmar","Georgia","Malawi","Sri Lanka","Cameroon","Bulgaria","Egypt","Yemen","Mali","Congo (Brazzaville)","Uganda","Senegal","Gabon","Niger","Cambodia","Tanzania","Madagascar","Chad","Guinea","Ivory Coast","Burkina Faso","Afghanistan","Rwanda","Benin","Syria","Burundi","Togo"],"xaxis":"x","y":[2,3,1,4,6,5,7,10,8,9,11,14,12,21,13,17,20,19,18,28,23,44,22,25,16,24,36,32,26,27,29,31,33,34,37,30,41,46,39,49,45,53,57,51,42,50,59,55,70,54,63,60,48,64,61,57,47,74,67,56,73,69,38,77,65,66,72,79,96,78,85,103,84,81,92,80,88,83,106,71,86,94,68,82,90,109,87,89,101,99,93,91,104,100,98,108,110,105,123,112,116,124,131,150,118,136,125,107,115,111,130,122,121,137,119,126,132,117,114,129,120,147,135,127,145,128,134,142,140,149,148,144,151,139,145,154,152,153,156,157,155],"yaxis":"y"},{"hovertemplate":"variable=Happiness.Rank_17<br>Country=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Happiness.Rank_17","line":{"color":"#00cc96","dash":"solid"},"mode":"lines","name":"Happiness.Rank_17","orientation":"v","showlegend":true,"type":"scatter","x":["Switzerland","Iceland","Denmark","Norway","Canada","Finland","Netherlands","Sweden","New Zealand","Australia","Israel","Costa Rica","Austria","Mexico","United States","Brazil","Luxembourg","Ireland","Belgium","United Arab Emirates","United Kingdom","Venezuela","Singapore","Panama","Germany","Chile","Qatar","France","Argentina","Czech Republic","Uruguay","Colombia","Thailand","Saudi Arabia","Spain","Malta","Kuwait","El Salvador","Guatemala","Uzbekistan","Slovakia","Japan","South Korea","Ecuador","Bahrain","Italy","Bolivia","Moldova","Paraguay","Kazakhstan","Slovenia","Lithuania","Nicaragua","Peru","Belarus","Poland","Malaysia","Croatia","Libya","Russia","Jamaica","Cyprus","Algeria","Kosovo","Turkmenistan","Mauritius","Estonia","Indonesia","Vietnam","Turkey","Kyrgyzstan","Nigeria","Bhutan","Azerbaijan","Pakistan","Jordan","Montenegro","China","Zambia","Romania","Serbia","Portugal","Latvia","Philippines","Morocco","Albania","Bosnia and Herzegovina","Dominican Republic","Mongolia","Greece","Lebanon","Hungary","Honduras","Tajikistan","Tunisia","Palestinian Territories","Bangladesh","Iran","Ukraine","Iraq","South Africa","Ghana","Zimbabwe","Liberia","India","Haiti","Congo (Kinshasa)","Nepal","Ethiopia","Sierra Leone","Mauritania","Kenya","Armenia","Botswana","Myanmar","Georgia","Malawi","Sri Lanka","Cameroon","Bulgaria","Egypt","Yemen","Mali","Congo (Brazzaville)","Uganda","Senegal","Gabon","Niger","Cambodia","Tanzania","Madagascar","Chad","Guinea","Ivory Coast","Burkina Faso","Afghanistan","Rwanda","Benin","Syria","Burundi","Togo"],"xaxis":"x","y":[4,3,2,1,7,5,6,9,8,10,11,12,13,25,14,22,18,15,17,21,19,82,26,30,16,20,35,31,24,23,28,36,32,37,34,27,39,45,29,47,40,51,55,44,41,48,58,56,70,60,62,52,43,63,67,46,42,77,68,49,76,65,53,78,59,64,66,81,94,69,98,95,97,85,80,74,83,79,116,57,73,89,54,72,84,109,90,86,100,87,88,75,91,96,102,103,110,108,132,117,101,131,138,148,122,145,126,99,119,106,123,112,121,142,114,125,136,120,107,105,104,146,127,124,133,115,118,135,129,153,144,137,149,128,134,141,151,143,152,154,150],"yaxis":"y"},{"hovertemplate":"variable=Happiness.Rank_18<br>Country=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Happiness.Rank_18","line":{"color":"#ab63fa","dash":"solid"},"mode":"lines","name":"Happiness.Rank_18","orientation":"v","showlegend":true,"type":"scatter","x":["Switzerland","Iceland","Denmark","Norway","Canada","Finland","Netherlands","Sweden","New Zealand","Australia","Israel","Costa Rica","Austria","Mexico","United States","Brazil","Luxembourg","Ireland","Belgium","United Arab Emirates","United Kingdom","Venezuela","Singapore","Panama","Germany","Chile","Qatar","France","Argentina","Czech Republic","Uruguay","Colombia","Thailand","Saudi Arabia","Spain","Malta","Kuwait","El Salvador","Guatemala","Uzbekistan","Slovakia","Japan","South Korea","Ecuador","Bahrain","Italy","Bolivia","Moldova","Paraguay","Kazakhstan","Slovenia","Lithuania","Nicaragua","Peru","Belarus","Poland","Malaysia","Croatia","Libya","Russia","Jamaica","Cyprus","Algeria","Kosovo","Turkmenistan","Mauritius","Estonia","Indonesia","Vietnam","Turkey","Kyrgyzstan","Nigeria","Bhutan","Azerbaijan","Pakistan","Jordan","Montenegro","China","Zambia","Romania","Serbia","Portugal","Latvia","Philippines","Morocco","Albania","Bosnia and Herzegovina","Dominican Republic","Mongolia","Greece","Lebanon","Hungary","Honduras","Tajikistan","Tunisia","Palestinian Territories","Bangladesh","Iran","Ukraine","Iraq","South Africa","Ghana","Zimbabwe","Liberia","India","Haiti","Congo (Kinshasa)","Nepal","Ethiopia","Sierra Leone","Mauritania","Kenya","Armenia","Botswana","Myanmar","Georgia","Malawi","Sri Lanka","Cameroon","Bulgaria","Egypt","Yemen","Mali","Congo (Brazzaville)","Uganda","Senegal","Gabon","Niger","Cambodia","Tanzania","Madagascar","Chad","Guinea","Ivory Coast","Burkina Faso","Afghanistan","Rwanda","Benin","Syria","Burundi","Togo"],"xaxis":"x","y":[5,4,3,2,7,1,6,9,8,10,19,13,12,24,18,28,17,14,16,20,11,102,34,27,15,25,32,23,29,21,31,37,46,33,36,22,45,40,30,44,39,54,57,48,43,47,62,67,64,60,51,50,41,65,73,42,35,82,70,59,56,61,84,66,68,55,63,96,95,74,92,91,97,87,75,90,81,86,125,52,78,77,53,71,85,112,93,83,94,79,80,69,72,88,111,104,115,106,138,117,105,108,144,149,133,148,132,101,127,113,126,124,129,146,130,128,147,116,99,100,122,152,118,114,135,109,103,134,120,153,143,131,140,107,121,145,151,136,150,156,139],"yaxis":"y"}],                        {"legend":{"title":{"text":"variable"},"tracegroupgap":0},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"title":{"text":"Happiness rank for different Years"},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Country"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"value"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('646a8542-fd9f-4e58-bb19-58a0d1ba3608');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


Finding the happiest country for years 2015-19


```python
print("Happiest Country in 2015: " +df_outer[df_outer['Happiness.Score_15']==df_outer['Happiness.Score_15'].max()].Country)
print("Happiest Country in 2016: " +df_outer[df_outer['Happiness.Score_16']==df_outer['Happiness.Score_16'].max()].Country)
print("Happiest Country in 2017: " +df_outer[df_outer['Happiness.Score_17']==df_outer['Happiness.Score_17'].max()].Country)
print("Happiest Country in 2018: " +df_outer[df_outer['Happiness.Score_18']==df_outer['Happiness.Score_18'].max()].Country)
print("Happiest Country in 2019: " +df_outer[df_outer['Happiness.Score']==df_outer['Happiness.Score'].max()].Country)
```

    0    Happiest Country in 2015: Switzerland
    Name: Country, dtype: object
    2    Happiest Country in 2016: Denmark
    Name: Country, dtype: object
    3    Happiest Country in 2017: Norway
    Name: Country, dtype: object
    5    Happiest Country in 2018: Finland
    Name: Country, dtype: object
    5    Happiest Country in 2019: Finland
    Name: Country, dtype: object


Correlation between HappinessRank and different columns/factors of the dataframe for the years 2015-19


```python
corr_data = {'2015':df_2015.corr()['Happiness.Rank'],'2016': df_2016.corr()['Happiness.Rank'],'2017':df_2017.corr()['Happiness.Rank'],'2018':df_2018.corr()['Happiness.Rank'],'2019':df_2019.corr()['Happiness.Rank']}
corr_frame = pd.concat(corr_data,axis = 1)
corr_frame
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2015</th>
      <th>2016</th>
      <th>2017</th>
      <th>2018</th>
      <th>2019</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Happiness.Rank</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>Happiness.Score</th>
      <td>-0.992105</td>
      <td>-0.995743</td>
      <td>-0.992774</td>
      <td>-0.991749</td>
      <td>-0.989096</td>
    </tr>
    <tr>
      <th>Standard.Error</th>
      <td>0.158516</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Economy</th>
      <td>-0.785267</td>
      <td>-0.793577</td>
      <td>-0.813244</td>
      <td>-0.805897</td>
      <td>-0.801947</td>
    </tr>
    <tr>
      <th>Family</th>
      <td>-0.733644</td>
      <td>-0.733276</td>
      <td>-0.736753</td>
      <td>-0.737500</td>
      <td>-0.767465</td>
    </tr>
    <tr>
      <th>Health</th>
      <td>-0.735613</td>
      <td>-0.767991</td>
      <td>NaN</td>
      <td>-0.778700</td>
      <td>-0.787411</td>
    </tr>
    <tr>
      <th>Freedom</th>
      <td>-0.556886</td>
      <td>-0.557169</td>
      <td>-0.551608</td>
      <td>-0.530786</td>
      <td>-0.546606</td>
    </tr>
    <tr>
      <th>Trust</th>
      <td>-0.372315</td>
      <td>-0.387102</td>
      <td>-0.405842</td>
      <td>-0.371133</td>
      <td>-0.351959</td>
    </tr>
    <tr>
      <th>Generosity</th>
      <td>-0.160142</td>
      <td>-0.145369</td>
      <td>-0.132620</td>
      <td>-0.103602</td>
      <td>-0.047993</td>
    </tr>
    <tr>
      <th>'Dystopia.Residual</th>
      <td>-0.521999</td>
      <td>-0.542616</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Lower.Confidence.Interval</th>
      <td>NaN</td>
      <td>-0.994928</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Upper.Confidence.Interval</th>
      <td>NaN</td>
      <td>-0.995525</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Whisker.high</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.993058</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Whisker.low</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.991533</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Health.Life.Expectancy</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.780716</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Dystopia.Residual</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>-0.484506</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
import plotly.graph_objects as go

fig = go.Figure(data=go.Heatmap({'z': corr_frame.values.tolist(),
            'x': corr_frame.columns.tolist(),
            'y': corr_frame.index.tolist()}))
fig.show()
```


<div>                            <div id="c993fc3e-a7ad-4d33-874c-fe78b9e332ee" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c993fc3e-a7ad-4d33-874c-fe78b9e332ee")) {                    Plotly.newPlot(                        "c993fc3e-a7ad-4d33-874c-fe78b9e332ee",                        [{"type":"heatmap","x":["2015","2016","2017","2018","2019"],"y":["Happiness.Rank","Happiness.Score","Standard.Error","Economy","Family","Health","Freedom","Trust","Generosity","'Dystopia.Residual","Lower.Confidence.Interval","Upper.Confidence.Interval","Whisker.high","Whisker.low","Health.Life.Expectancy","Dystopia.Residual"],"z":[[1.0,1.0,1.0,1.0,1.0],[-0.9921053148284925,-0.9957433909369678,-0.9927744658907134,-0.9917487030301028,-0.9890962183233043],[0.15851647296190474,null,null,null,null],[-0.7852669153290183,-0.7935771153666057,-0.8132436353491986,-0.8058971977830865,-0.8019465355862588],[-0.7336435317250529,-0.7332763455700573,-0.7367526832313127,-0.7374999559747166,-0.7674653087738499],[-0.7356129584428032,-0.7679907812050296,null,-0.77869979974418,-0.7874106573506173],[-0.5568860883741894,-0.5571687096310289,-0.5516078425657571,-0.5307859884662018,-0.5466063956173797],[-0.37231511689406366,-0.3871016397372249,-0.4058423300313603,-0.3711332004198953,-0.35195851338710193],[-0.16014156750561243,-0.1453687806114181,-0.13261979066642146,-0.10360160566657028,-0.04799261091107459],[-0.5219994726332529,-0.5426158003215974,null,null,null],[null,-0.9949283766891872,null,null,null],[null,-0.9955251141928845,null,null,null],[null,null,-0.993058492839789,null,null],[null,null,-0.9915334758674776,null,null],[null,null,-0.7807158366857987,null,null],[null,null,-0.48450596468603413,null,null]]}],                        {"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c993fc3e-a7ad-4d33-874c-fe78b9e332ee');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
px.line(df_2019, x='Happiness.Score', y=['Economy','Health','Trust'], title='Happiness score vs. Economy, Health and Trust')
```


<div>                            <div id="360cb9c0-4231-4ea5-9347-0a6e217d41f8" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("360cb9c0-4231-4ea5-9347-0a6e217d41f8")) {                    Plotly.newPlot(                        "360cb9c0-4231-4ea5-9347-0a6e217d41f8",                        [{"hovertemplate":"variable=Economy<br>Happiness.Score=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Economy","line":{"color":"#636efa","dash":"solid"},"mode":"lines","name":"Economy","orientation":"v","showlegend":true,"type":"scatter","x":[7.769,7.6,7.554,7.494,7.488,7.48,7.343,7.307,7.278,7.246,7.228,7.167,7.139,7.09,7.054,7.021,6.985,6.923,6.892,6.852,6.825,6.726,6.595,6.592,6.446,6.444,6.436,6.375,6.374,6.354,6.321,6.3,6.293,6.262,6.253,6.223,6.199,6.198,6.192,6.182,6.174,6.149,6.125,6.118,6.105,6.1,6.086,6.07,6.046,6.028,6.021,6.008,5.94,5.895,5.893,5.89,5.888,5.886,5.86,5.809,5.779,5.758,5.743,5.718,5.697,5.693,5.653,5.648,5.631,5.603,5.529,5.525,5.523,5.467,5.432,5.43,5.425,5.386,5.373,5.339,5.323,5.287,5.285,5.274,5.265,5.261,5.247,5.211,5.208,5.208,5.197,5.192,5.191,5.175,5.082,5.044,5.011,4.996,4.944,4.913,4.906,4.883,4.812,4.799,4.796,4.722,4.719,4.707,4.7,4.696,4.681,4.668,4.639,4.628,4.587,4.559,4.548,4.534,4.519,4.516,4.509,4.49,4.466,4.461,4.456,4.437,4.418,4.39,4.374,4.366,4.36,4.35,4.332,4.286,4.212,4.189,4.166,4.107,4.085,4.015,3.975,3.973,3.933,3.802,3.775,3.663,3.597,3.488,3.462,3.41,3.38,3.334,3.231,3.203,3.083,2.853],"xaxis":"x","y":[1.34,1.383,1.488,1.38,1.396,1.452,1.387,1.303,1.365,1.376,1.372,1.034,1.276,1.609,1.333,1.499,1.373,1.356,1.433,1.269,1.503,1.3,1.07,1.324,1.368,1.159,0.8,1.403,1.684,1.286,1.149,1.004,1.124,1.572,0.794,1.294,1.362,1.246,1.231,1.206,0.745,1.238,0.985,1.258,0.694,0.882,1.092,1.162,1.263,0.912,1.5,1.05,1.187,1.301,1.237,0.831,1.12,1.327,0.642,1.173,0.776,1.201,0.855,1.263,0.96,1.221,0.677,1.183,0.807,1.004,0.685,1.044,1.051,0.493,1.155,1.438,1.015,0.945,1.183,1.221,1.067,1.181,0.948,0.983,0.696,0.551,1.052,1.002,0.801,1.043,0.987,0.931,1.029,0.741,0.813,0.549,1.092,0.611,0.569,0.446,0.837,0.393,0.673,1.057,0.764,0.96,0.947,0.96,0.574,0.657,0.45,0.0,0.879,0.138,0.331,0.85,1.1,0.38,0.886,0.308,0.512,0.57,0.204,0.921,0.562,1.043,0.094,0.385,0.268,0.949,0.71,0.35,0.82,0.336,0.811,0.332,0.913,0.578,0.275,0.755,0.073,0.274,0.274,0.489,0.046,0.366,0.323,1.041,0.619,0.191,0.287,0.359,0.476,0.35,0.026,0.306],"yaxis":"y"},{"hovertemplate":"variable=Health<br>Happiness.Score=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Health","line":{"color":"#EF553B","dash":"solid"},"mode":"lines","name":"Health","orientation":"v","showlegend":true,"type":"scatter","x":[7.769,7.6,7.554,7.494,7.488,7.48,7.343,7.307,7.278,7.246,7.228,7.167,7.139,7.09,7.054,7.021,6.985,6.923,6.892,6.852,6.825,6.726,6.595,6.592,6.446,6.444,6.436,6.375,6.374,6.354,6.321,6.3,6.293,6.262,6.253,6.223,6.199,6.198,6.192,6.182,6.174,6.149,6.125,6.118,6.105,6.1,6.086,6.07,6.046,6.028,6.021,6.008,5.94,5.895,5.893,5.89,5.888,5.886,5.86,5.809,5.779,5.758,5.743,5.718,5.697,5.693,5.653,5.648,5.631,5.603,5.529,5.525,5.523,5.467,5.432,5.43,5.425,5.386,5.373,5.339,5.323,5.287,5.285,5.274,5.265,5.261,5.247,5.211,5.208,5.208,5.197,5.192,5.191,5.175,5.082,5.044,5.011,4.996,4.944,4.913,4.906,4.883,4.812,4.799,4.796,4.722,4.719,4.707,4.7,4.696,4.681,4.668,4.639,4.628,4.587,4.559,4.548,4.534,4.519,4.516,4.509,4.49,4.466,4.461,4.456,4.437,4.418,4.39,4.374,4.366,4.36,4.35,4.332,4.286,4.212,4.189,4.166,4.107,4.085,4.015,3.975,3.973,3.933,3.802,3.775,3.663,3.597,3.488,3.462,3.41,3.38,3.334,3.231,3.203,3.083,2.853],"xaxis":"x","y":[0.986,0.996,1.028,1.026,0.999,1.052,1.009,1.026,1.039,1.016,1.036,0.963,1.029,1.012,0.996,0.999,0.987,0.986,0.874,0.92,0.825,0.999,0.861,1.045,0.914,0.92,0.746,0.795,0.871,1.062,0.91,0.802,0.891,1.141,0.789,1.039,0.871,0.881,0.713,0.884,0.756,0.818,0.841,0.953,0.835,0.758,0.881,0.825,1.042,0.868,0.808,0.828,0.812,1.036,0.874,0.831,0.798,1.088,0.828,0.729,0.706,0.828,0.777,1.042,0.854,0.999,0.535,0.726,0.657,0.854,0.739,0.673,0.871,0.718,0.914,1.122,0.779,0.845,0.808,0.828,0.789,0.999,0.667,0.838,0.245,0.723,0.657,0.785,0.782,0.769,0.815,0.66,0.893,0.851,0.604,0.331,0.815,0.486,0.232,0.677,0.815,0.397,0.508,0.571,0.551,0.469,0.874,0.805,0.637,0.672,0.571,0.268,0.477,0.366,0.38,0.815,0.785,0.375,0.752,0.428,0.581,0.489,0.39,0.815,0.723,0.574,0.357,0.308,0.242,0.831,0.555,0.192,0.739,0.532,0.0,0.443,0.644,0.426,0.41,0.588,0.443,0.505,0.555,0.168,0.38,0.433,0.449,0.538,0.44,0.495,0.463,0.614,0.499,0.361,0.105,0.295],"yaxis":"y"},{"hovertemplate":"variable=Trust<br>Happiness.Score=%{x}<br>value=%{y}<extra></extra>","legendgroup":"Trust","line":{"color":"#00cc96","dash":"solid"},"mode":"lines","name":"Trust","orientation":"v","showlegend":true,"type":"scatter","x":[7.769,7.6,7.554,7.494,7.488,7.48,7.343,7.307,7.278,7.246,7.228,7.167,7.139,7.09,7.054,7.021,6.985,6.923,6.892,6.852,6.825,6.726,6.595,6.592,6.446,6.444,6.436,6.375,6.374,6.354,6.321,6.3,6.293,6.262,6.253,6.223,6.199,6.198,6.192,6.182,6.174,6.149,6.125,6.118,6.105,6.1,6.086,6.07,6.046,6.028,6.021,6.008,5.94,5.895,5.893,5.89,5.888,5.886,5.86,5.809,5.779,5.758,5.743,5.718,5.697,5.693,5.653,5.648,5.631,5.603,5.529,5.525,5.523,5.467,5.432,5.43,5.425,5.386,5.373,5.339,5.323,5.287,5.285,5.274,5.265,5.261,5.247,5.211,5.208,5.208,5.197,5.192,5.191,5.175,5.082,5.044,5.011,4.996,4.944,4.913,4.906,4.883,4.812,4.799,4.796,4.722,4.719,4.707,4.7,4.696,4.681,4.668,4.639,4.628,4.587,4.559,4.548,4.534,4.519,4.516,4.509,4.49,4.466,4.461,4.456,4.437,4.418,4.39,4.374,4.366,4.36,4.35,4.332,4.286,4.212,4.189,4.166,4.107,4.085,4.015,3.975,3.973,3.933,3.802,3.775,3.663,3.597,3.488,3.462,3.41,3.38,3.334,3.231,3.203,3.083,2.853],"xaxis":"x","y":[0.393,0.41,0.341,0.118,0.298,0.343,0.373,0.38,0.308,0.226,0.29,0.093,0.082,0.316,0.278,0.31,0.265,0.21,0.128,0.036,0.182,0.151,0.073,0.183,0.097,0.056,0.078,0.132,0.167,0.079,0.054,0.086,0.15,0.453,0.074,0.03,0.11,0.014,0.016,0.05,0.24,0.042,0.034,0.057,0.127,0.006,0.05,0.005,0.041,0.087,0.097,0.028,0.064,0.056,0.161,0.028,0.06,0.14,0.078,0.096,0.064,0.02,0.08,0.162,0.027,0.025,0.098,0.031,0.107,0.039,0.0,0.152,0.08,0.144,0.022,0.287,0.101,0.006,0.106,0.024,0.142,0.034,0.038,0.034,0.041,0.023,0.028,0.114,0.076,0.182,0.027,0.028,0.1,0.073,0.167,0.037,0.004,0.04,0.09,0.089,0.13,0.082,0.093,0.055,0.164,0.055,0.027,0.047,0.062,0.066,0.072,0.27,0.056,0.102,0.113,0.064,0.125,0.086,0.164,0.167,0.053,0.088,0.138,0.055,0.143,0.089,0.053,0.052,0.045,0.047,0.172,0.078,0.01,0.1,0.135,0.06,0.067,0.087,0.085,0.085,0.033,0.078,0.041,0.093,0.18,0.089,0.11,0.1,0.141,0.089,0.077,0.411,0.147,0.025,0.035,0.091],"yaxis":"y"}],                        {"legend":{"title":{"text":"variable"},"tracegroupgap":0},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"title":{"text":"Happiness score vs. Economy, Health and Trust"},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"Happiness.Score"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"value"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('360cb9c0-4231-4ea5-9347-0a6e217d41f8');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


Visualizing the distribution of Happiness Score for the year 2015-19


```python
import plotly.figure_factory as ff
variables = [ df_2015['Happiness.Score'],df_2016['Happiness.Score'],df_2017['Happiness.Score'],df_2018['Happiness.Score'],df_2019['Happiness.Score']]
labels = ['2015','2016', '2017','2018','2019']
fig = ff.create_distplot(variables, labels, show_hist=False)
fig.show()

```


<div>                            <div id="e9dcf19e-6096-41e3-8dff-eaef4628e1c8" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e9dcf19e-6096-41e3-8dff-eaef4628e1c8")) {                    Plotly.newPlot(                        "e9dcf19e-6096-41e3-8dff-eaef4628e1c8",                        [{"legendgroup":"2015","marker":{"color":"rgb(31, 119, 180)"},"mode":"lines","name":"2015","showlegend":true,"type":"scatter","x":[2.839,2.848496,2.857992,2.867488,2.8769839999999998,2.88648,2.895976,2.905472,2.914968,2.924464,2.93396,2.943456,2.952952,2.9624479999999997,2.971944,2.98144,2.990936,3.000432,3.009928,3.019424,3.02892,3.038416,3.047912,3.057408,3.066904,3.0764,3.085896,3.095392,3.104888,3.114384,3.1238799999999998,3.133376,3.142872,3.152368,3.161864,3.17136,3.180856,3.190352,3.199848,3.2093439999999998,3.2188399999999997,3.228336,3.237832,3.247328,3.256824,3.26632,3.275816,3.285312,3.2948079999999997,3.304304,3.3138,3.323296,3.332792,3.342288,3.351784,3.36128,3.3707759999999998,3.3802719999999997,3.389768,3.3992639999999996,3.40876,3.418256,3.427752,3.437248,3.446744,3.4562399999999998,3.4657359999999997,3.475232,3.484728,3.494224,3.50372,3.513216,3.522712,3.532208,3.541704,3.5511999999999997,3.560696,3.5701919999999996,3.579688,3.589184,3.59868,3.608176,3.6176719999999998,3.6271679999999997,3.6366639999999997,3.64616,3.6556559999999996,3.665152,3.674648,3.684144,3.69364,3.7031359999999998,3.7126319999999997,3.7221279999999997,3.731624,3.74112,3.750616,3.760112,3.769608,3.779104,3.7885999999999997,3.7980959999999997,3.8075919999999996,3.817088,3.8265839999999995,3.83608,3.845576,3.855072,3.864568,3.8740639999999997,3.88356,3.8930559999999996,3.902552,3.9120479999999995,3.921544,3.9310399999999994,3.940536,3.950032,3.9595279999999997,3.969024,3.9785199999999996,3.988016,3.997512,4.007008,4.016503999999999,4.026,4.035496,4.044992,4.054488,4.063984,4.07348,4.082976,4.092472,4.101967999999999,4.111464,4.12096,4.130456,4.139951999999999,4.149448,4.158944,4.1684399999999995,4.177936,4.187431999999999,4.196928,4.206424,4.21592,4.225415999999999,4.234912,4.244408,4.2539039999999995,4.2634,4.272895999999999,4.282392,4.291888,4.301384,4.31088,4.3203759999999996,4.329872,4.3393679999999994,4.348864,4.358359999999999,4.367856,4.377352,4.386848,4.396344,4.4058399999999995,4.415336,4.424832,4.434328,4.443823999999999,4.45332,4.462816,4.472312,4.481808,4.4913039999999995,4.5008,4.510296,4.519792,4.529287999999999,4.538784,4.54828,4.557776,4.567272,4.5767679999999995,4.586264,4.595759999999999,4.605256,4.614751999999999,4.624248,4.633744,4.64324,4.652735999999999,4.6622319999999995,4.671728,4.681223999999999,4.69072,4.700215999999999,4.709712,4.719208,4.728704,4.7382,4.7476959999999995,4.757192,4.766687999999999,4.776184,4.785679999999999,4.795176,4.804672,4.814168,4.823664,4.8331599999999995,4.842656,4.852152,4.861648,4.871143999999999,4.88064,4.890136,4.8996319999999995,4.909127999999999,4.918623999999999,4.92812,4.937615999999999,4.947112,4.956607999999999,4.966104,4.9756,4.9850959999999995,4.994591999999999,5.004087999999999,5.013584,5.023079999999999,5.032576,5.042071999999999,5.051568,5.061064,5.0705599999999995,5.080055999999999,5.089551999999999,5.099048,5.108544,5.118039999999999,5.127535999999999,5.137032,5.146528,5.156024,5.165519999999999,5.175015999999999,5.184512,5.194008,5.203503999999999,5.212999999999999,5.222496,5.231992,5.241488,5.250983999999999,5.260479999999999,5.269976,5.279472,5.288968,5.298463999999999,5.30796,5.317456,5.326952,5.336448,5.345943999999999,5.35544,5.364936,5.374432,5.383927999999999,5.3934239999999996,5.40292,5.412416,5.421912,5.431407999999999,5.440904,5.4504,5.459896,5.469391999999999,5.4788879999999995,5.488384,5.497879999999999,5.507376,5.516871999999999,5.526368,5.535864,5.54536,5.554855999999999,5.5643519999999995,5.573848,5.583343999999999,5.59284,5.602335999999999,5.611832,5.621328,5.630824,5.640319999999999,5.6498159999999995,5.659312,5.668807999999999,5.678304,5.687799999999999,5.697296,5.706792,5.716288,5.725783999999999,5.7352799999999995,5.744776,5.754271999999999,5.763767999999999,5.773263999999999,5.78276,5.792255999999999,5.801752,5.811247999999999,5.8207439999999995,5.83024,5.839735999999999,5.849231999999999,5.858727999999999,5.868224,5.877719999999999,5.887216,5.896711999999999,5.9062079999999995,5.915704,5.925199999999999,5.934695999999999,5.944191999999999,5.953688,5.963184,5.9726799999999995,5.982175999999999,5.991671999999999,6.001168,6.010664,6.020159999999999,6.029655999999999,6.039152,6.048648,6.058143999999999,6.067639999999999,6.077135999999999,6.086632,6.096128,6.105623999999999,6.115119999999999,6.124616,6.134112,6.1436079999999995,6.153103999999999,6.162599999999999,6.172096,6.181592,6.191088,6.200583999999999,6.21008,6.219576,6.2290719999999995,6.238567999999999,6.248063999999999,6.25756,6.267056,6.276552,6.286047999999999,6.295544,6.30504,6.3145359999999995,6.324032,6.333527999999999,6.343024,6.352519999999999,6.362016,6.371511999999999,6.381008,6.390504,6.3999999999999995,6.409495999999999,6.418991999999999,6.428488,6.437983999999999,6.44748,6.456975999999999,6.4664719999999996,6.475968,6.4854639999999995,6.494959999999999,6.504455999999999,6.513952,6.523447999999999,6.532944,6.542439999999999,6.5519359999999995,6.561432,6.570927999999999,6.580423999999999,6.589919999999999,6.599416,6.608911999999999,6.618408,6.627903999999999,6.6373999999999995,6.646895999999999,6.656391999999999,6.665887999999999,6.675383999999999,6.68488,6.694375999999999,6.703871999999999,6.713367999999999,6.7228639999999995,6.73236,6.741855999999999,6.751351999999999,6.760847999999999,6.770344,6.779839999999999,6.789335999999999,6.798831999999999,6.8083279999999995,6.817824,6.827319999999999,6.836815999999999,6.846311999999999,6.855808,6.865304,6.874799999999999,6.884295999999999,6.8937919999999995,6.903288,6.912784,6.922279999999999,6.931775999999999,6.941272,6.950767999999998,6.960263999999999,6.969759999999999,6.9792559999999995,6.988752,6.998248,7.007743999999999,7.017239999999999,7.026736,7.036231999999998,7.045727999999999,7.055223999999999,7.0647199999999994,7.074216,7.083712,7.093207999999999,7.102703999999999,7.1122,7.121695999999998,7.131191999999999,7.140687999999999,7.150183999999999,7.15968,7.169176,7.178671999999999,7.188167999999999,7.197664,7.207159999999998,7.216655999999999,7.226151999999999,7.235647999999999,7.245144,7.25464,7.264135999999999,7.273631999999999,7.283128,7.292624,7.302119999999999,7.311615999999999,7.321111999999999,7.330608,7.340104,7.349599999999999,7.359095999999999,7.368592,7.378088,7.387583999999999,7.397079999999999,7.406575999999999,7.416072,7.425567999999998,7.435063999999999,7.444559999999999,7.454056,7.463552,7.473048,7.482543999999999,7.492039999999999,7.501536,7.511031999999998,7.520527999999999,7.530023999999999,7.5395199999999996,7.549016,7.558512,7.568007999999999,7.577503999999999],"xaxis":"x","y":[0.03182592386691587,0.03249671491296214,0.03317816338334368,0.03387050167144494,0.03457396674078862,0.03528879961337618,0.036015244822704545,0.036753549832717225,0.03750396442413061,0.03826674004975846,0.03904212916063541,0.03983038450491383,0.04063175840167731,0.041446501991974465,0.0422748644695333,0.04311709229376118,0.04397342838777431,0.044844111324326516,0.045729374502624855,0.046629445319124344,0.04754454433548662,0.04847488444696692,0.049420670054560356,0.05038209624428906,0.0513593479770509,0.052352599292469076,0.053362012530191134,0.05438773757207474,0.05542991110867129,0.05648865593337767,0.05756408026756668,0.05865627711993311,0.05976532368319991,0.060891280771223556,0.06203419229941384,0.06319408481124494,0.06437096705348237,0.06556482960258185,0.06677564454453537,0.06800336521024423,0.0692479259682924,0.07050924207677453,0.0717872095956036,0.07308170536048456,0.07439258701949059,0.07571969313292647,0.07706284333689767,0.07842183857073949,0.07979646136818752,0.08118647621189687,0.0825916299506413,0.08401165227824761,0.08544625627304545,0.08689513899633872,0.0883579821481368,0.08983445277811904,0.09132420404954755,0.0928268760535923,0.09434209667129388,0.09586948248015194,0.09740863970211293,0.09895916518951733,0.10052064744537734,0.10209266767416839,0.10367480085916103,0.10526661686216465,0.10686768154142687,0.10847755788331703,0.110095807143328,0.1117219899918545,0.11335566766014876,0.11499640308181924,0.11664376202522245,0.11829731421209913,0.11995663441783523,0.12162130354877021,0.1232909096920441,0.12496504913355612,0.1266433273397218,0.12832535989883098,0.13001077341796485,0.1316992063715843,0.13339030989808828,0.13508374854083702,0.13677920093035156,0.13847636040462635,0.14017493556473945,0.14187465076320313,0.14357524652275877,0.1452764798836125,0.14697812467738527,0.14867997172635763,0.15038182896688934,0.15208352149620855,0.15378489154207353,0.15548579835513737,0.15718611802415247,0.1588857432144809,0.16058458283068897,0.1622825616043175,0.1639796196082318,0.16567571169925888,0.16737080689111433,0.16906488765991282,0.17075794918483092,0.17244999852676574,0.17414105374807912,0.17583114297676866,0.17752030341863037,0.17920858032119202,0.18089602589339462,0.18258269818517617,0.18426865993128236,0.18595397736376146,0.18763871899773946,0.18932295439516644,0.19100675291132146,0.19269018242892408,0.1943733080847486,0.19605619099366986,0.19773888697506672,0.19942144528650535,0.20110390736958614,0.20278630561279076,0.20446866213608875,0.20615098760198083,0.2078332800575413,0.20951552381190477,0.21119768835349673,0.21287972731114616,0.21456157746306018,0.21624315779743902,0.21792436862832007,0.21960509077002782,0.22128518477338105,0.22296449022657874,0.2246428251234494,0.22631998530149094,0.2279957439518848,0.22966985120339958,0.23134203378183896,0.23301199474642525,0.2346794133042309,0.23634394470351527,0.23800522020654277,0.23966284714219915,0.2413164090384551,0.2429654658344684,0.24460955417185276,0.2462481877644037,0.24788085784531708,0.24950703369070096,0.25112616321796133,0.2527376736574107,0.2543409722952498,0.2559354472858633,0.25752046853118704,0.25909538862472287,0.26065954385761064,0.262212255284011,0.2637528298429064,0.2652805615333035,0.266794732639686,0.26829461500447466,0.26977947134414604,0.2712485566055748,0.27270111935910274,0.2741364032247656,0.27555364832806545,0.27695209278163435,0.2783309741891077,0.27968953116750633,0.28102700488441834,0.28234264060627284,0.2836356892539994,0.28490540896239885,0.2861510666395557,0.28737193952267565,0.28856731672674907,0.2897365007825005,0.29087880916012776,0.29199357577538626,0.29308015247464,0.29413791049555593,0.2951662419001872,0.2961645609772668,0.2971323056105911,0.2980689386104619,0.2989739490052223,0.2998468532900021,0.30068719662987264,0.3014945540146841,0.3022685313629479,0.30300876657220543,0.3037149305134115,0.30438672796694544,0.3050238984979487,0.3056262172687791,0.30619349578645777,0.30672558258307625,0.3072223638272295,0.30768376386462526,0.3081097456861291,0.3085003113215954,0.30885550215794344,0.30917539918003994,0.309460123133062,0.30970983460512935,0.30992473402910736,0.3101050616026159,0.31025109712539617,0.31036315975333234,0.3104416076685517,0.31048683766519125,0.3104992846505476,0.3104794210615072,0.31042775619630353,0.3103448354618309,0.3102312395369124,0.3100875834521166,0.3099145155869011,0.30971271658506194,0.30948289818967645,0.30922580199893684,0.3089421981444824,0.30863288389407295,0.3082986821806609,0.3079404400601577,0.30755902710042204,0.30715533370423875,0.30673026936928405,0.3062847608883325,0.3058197504931831,0.30533619394602823,0.30483505858222937,0.30431732130869177,0.3037839665622554,0.30323598423275727,0.30267436755561644,0.3021001109790225,0.30151420801097956,0.3009176490516731,0.3003114192167824,0.29969649615752314,0.29907384788336,0.2984444305934491,0.29780918652298394,0.2971690418107115,0.2965249043939553,0.2958776619375365,0.2952281798030111,0.2945772990646409,0.29392583457850957,0.293274573111142,0.2926242715339157,0.2919756550894672,0.2913294157361663,0.29068621057658445,0.2900466603757143,0.2894113481744897,0.2887808180039314,0.28815557370498673,0.28753607785885305,0.28692275083226915,0.2863159699419279,0.28571606874181354,0.28512333643688165,0.2845380174261197,0.28396031097758695,0.28339037103762055,0.28282830617592186,0.2822741796677844,0.28172800971423667,0.2811897698003847,0.28065938919173744,0.28013675356779594,0.27962170579166395,0.2791140468139353,0.27861353670859584,0.27811989583815866,0.27763280614476027,0.2771519125634291,0.2766768245532579,0.2762071177417412,0.2757423356770535,0.275281991682632,0.2748255708079691,0.27437253186914046,0.2739223095721997,0.27347431671222366,0.27302794644046086,0.2725825745917468,0.27213756206406947,0.27169225724194995,0.27124599845510095,0.27079811646365043,0.2703479369611095,0.2698947830861567,0.26943797793427327,0.26897684706022973,0.26851072096247663,0.26803893754052754,0.26756084451654244,0.2670758018124491,0.26658318387412433,0.26608238193436096,0.2655728062066081,0.26505388800174623,0.26452508176047473,0.2639858669942542,0.26343575012810305,0.2628742662389703,0.26230098068383834,0.26171549061216126,0.2611174263577271,0.260506452705543,0.25988227002982844,0.2592446152997775,0.2585932629502499,0.2579280256151351,0.2572487547216754,0.2565553409446058,0.2558477145195328,0.25512584541552397,0.25438974336746517,0.25363945776926694,0.25287507742955934,0.2520967301920455,0.2513045824231916,0.2504988383704411,0.24967973939460628,0.24884756308057668,0.2480026222308822,0.24714526374709644,0.2462758674044274,0.24539484452520766,0.24450263655732032,0.24359971356389729,0.2426865726308822,0.24176373619929217,0.2408317503291995,0.2398911829026307,0.23894262177269665,0.2379866728663754,0.2370239582484249,0.2360551141539278,0.23508078899697246,0.23410164136292877,0.23311833799170376,0.23213155175927158,0.23114195966462386,0.2301502408291381,0.22915707451516018,0.2281631381703945,0.22716910550443767,0.22617564460354958,0.22518341608944478,0.22419307132760785,0.22320525069029742,0.2222205818790684,0.22123967831130253,0.22026313757485386,0.2192915399545634,0.218325447034004,0.21736540037543686,0.21641192028057074,0.21546550463432468,0.21452662783340534,0.21359573980112237,0.2126732650894857,0.21175960206924524,0.21085512220817207,0.20996016943751797,0.20907505960623526,0.20820008002222432,0.20733548907952024,0.20648151597006215,0.20563836047836748,0.20480619285718116,0.20398515378189935,0.20317535438134934,0.20237687634227197,0.20158977208467557,0.2008140650050448,0.20004974978423304,0.19929679275674525,0.19855513233799638,0.19782467950604163,0.19710531833421424,0.19639690657104658,0.19569927626383096,0.19501223442216667,0.19433556371784824,0.19366902321748372,0.19301234914427717,0.19236525566547727,0.19172743570207337,0.19109856175741594,0.19047828676155157,0.18986624492818668,0.18926205262132356,0.18866530922876987,0.1880755980398669,0.18749248712495567,0.18691553021426524,0.18634426757408884,0.18577822687829204,0.18521692407338336,0.18465986423556682,0.18410654241838134,0.1835564444897182,0.1830090479571954,0.18246382278105328,0.1819202321739078,0.18137773338687527,0.18083577848175475,0.1802938150891062,0.179751287152221,0.1792076356571248,0.17866229934888764,0.17811471543463558,0.1775643202737781,0.17701055005606378,0.17645284146816498,0.17589063234957308,0.1753233623386493,0.1747504735097209,0.1741714110021685,0.17358562364245372,0.17299256456006318,0.1723916917983415,0.17178246892116755,0.17116436561640627,0.1705368582970274,0.16989943070073027,0.169251574488859,0.16859278984530723,0.16792258607604063,0.1672404822097628,0.1665460076001588,0.16583870253002644,0.1651181188175082,0.16438382042449246,0.1636353840671431,0.16287239982836946,0.1620944717719236,0.16130121855766505,0.1604922740573991,0.15966728797055224,0.15882592643880833,0.15796787265869658,0.15709282749098008,0.15620051006556937,0.15529065838056064,0.15436302989386894,0.15341740210582663,0.15245357313100152,0.15147136225739868,0.1504706104911162,0.1494511810844593,0.14841296004543142,0.14735585662648684,0.14627980379037273,0.1451847586508582,0.1440707028861393,0.14293764312269036,0.14178561128735429,0.14061466492547578,0.1394248874829238,0.13821638854989826,0.13698930406447435,0.13574379647391954,0.13448005485191003,0.13319829496986668,0.13189875932075226,0.1305817170938034,0.12924746409879614,0.12789632263860326,0.12652864132895816,0.12514479486450758,0.12374518373040955,0.12233023385892744,0.12090039623064715,0.11945614642015467,0.11799798408620264,0.1165264324066109,0.11504203745833817,0.11354536754338693,0.1120370124614008,0.11051758273003225,0.10898770875435385,0.10744803994680842,0.1058992437993698,0.1043420049098005,0.10277702396407735,0.10120501667722781,0.09962671269500713,0.09804285445900626,0.0964541960379305,0.09486150192794376],"yaxis":"y"},{"legendgroup":"2016","marker":{"color":"rgb(255, 127, 14)"},"mode":"lines","name":"2016","showlegend":true,"type":"scatter","x":[2.905,2.9142419999999998,2.9234839999999997,2.9327259999999997,2.9419679999999997,2.9512099999999997,2.9604519999999996,2.9696939999999996,2.978936,2.988178,2.99742,3.006662,3.015904,3.025146,3.034388,3.04363,3.052872,3.062114,3.0713559999999998,3.0805979999999997,3.0898399999999997,3.0990819999999997,3.1083239999999996,3.117566,3.126808,3.13605,3.145292,3.154534,3.163776,3.173018,3.18226,3.191502,3.200744,3.209986,3.2192279999999998,3.2284699999999997,3.2377119999999997,3.2469539999999997,3.2561959999999996,3.2654379999999996,3.27468,3.283922,3.293164,3.302406,3.311648,3.32089,3.330132,3.339374,3.348616,3.357858,3.3670999999999998,3.3763419999999997,3.3855839999999997,3.3948259999999997,3.4040679999999996,3.41331,3.4225519999999996,3.431794,3.441036,3.450278,3.45952,3.468762,3.478004,3.487246,3.496488,3.50573,3.5149719999999998,3.5242139999999997,3.5334559999999997,3.5426979999999997,3.55194,3.5611819999999996,3.570424,3.579666,3.588908,3.59815,3.607392,3.616634,3.625876,3.635118,3.64436,3.653602,3.6628439999999998,3.672086,3.6813279999999997,3.69057,3.6998119999999997,3.709054,3.7182959999999996,3.727538,3.73678,3.746022,3.755264,3.764506,3.773748,3.78299,3.792232,3.801474,3.8107159999999998,3.8199579999999997,3.8291999999999997,3.8384419999999997,3.847684,3.8569259999999996,3.866168,3.87541,3.884652,3.893894,3.903136,3.912378,3.92162,3.930862,3.940104,3.949346,3.9585879999999998,3.96783,3.9770719999999997,3.986314,3.9955559999999997,4.004798,4.01404,4.023282,4.032524,4.041766,4.0510079999999995,4.06025,4.069492,4.078734,4.087975999999999,4.097218,4.10646,4.115702,4.124944,4.134186,4.143428,4.15267,4.161912,4.171154,4.180396,4.189638,4.19888,4.2081219999999995,4.217364,4.226606,4.235848,4.24509,4.254332,4.263574,4.272816,4.282058,4.2913,4.300542,4.309784,4.319026,4.328268,4.33751,4.346752,4.355994,4.3652359999999994,4.374478,4.38372,4.392962,4.402204,4.411446,4.420688,4.42993,4.439172,4.448414,4.457656,4.4668980000000005,4.47614,4.4853819999999995,4.494624,4.503866,4.513108,4.522349999999999,4.531592,4.540834,4.550076,4.559318,4.56856,4.577802,4.587044,4.596286,4.605528,4.61477,4.6240120000000005,4.633254,4.6424959999999995,4.651738,4.66098,4.670222,4.679464,4.688706,4.697948,4.70719,4.716432,4.725674,4.734916,4.744158,4.7534,4.762642,4.771884,4.781126,4.790368,4.7996099999999995,4.808852,4.818094,4.827336,4.836578,4.84582,4.855062,4.864304,4.873546,4.882788,4.89203,4.9012720000000005,4.910514,4.919756,4.928998,4.93824,4.947482,4.9567239999999995,4.965966,4.975208,4.984450000000001,4.993691999999999,5.002934,5.012176,5.021418,5.03066,5.039902,5.049144,5.0583860000000005,5.067628,5.0768699999999995,5.086112,5.095354,5.104596,5.113837999999999,5.12308,5.132322,5.141564000000001,5.150805999999999,5.160048,5.16929,5.178532000000001,5.187774,5.197016,5.206258,5.2155000000000005,5.224742,5.2339839999999995,5.243226,5.252468,5.26171,5.270951999999999,5.280194,5.289436,5.298678000000001,5.307919999999999,5.317162,5.326404,5.3356460000000006,5.344888,5.35413,5.363372,5.3726140000000004,5.381856,5.391098,5.40034,5.409582,5.418824,5.428066,5.437308,5.44655,5.455792000000001,5.465033999999999,5.474276,5.483518,5.4927600000000005,5.502002,5.511244,5.520486,5.529728,5.53897,5.548212,5.557454,5.566696,5.575938,5.58518,5.594422,5.603664,5.612906000000001,5.622148,5.63139,5.640632,5.6498740000000005,5.659116,5.668358,5.6776,5.686842,5.696084,5.705326,5.714568,5.72381,5.733052,5.742294,5.751536,5.760778,5.770020000000001,5.779262,5.788504,5.797746,5.8069880000000005,5.816230000000001,5.8254719999999995,5.834714,5.843956,5.853198000000001,5.86244,5.871682,5.880924,5.890166,5.899408,5.90865,5.917892,5.927134000000001,5.936376,5.945618,5.95486,5.9641020000000005,5.973344000000001,5.9825859999999995,5.991828,6.00107,6.010312000000001,6.019553999999999,6.028796,6.038038,6.047280000000001,6.056522,6.065764,6.075006,6.0842480000000005,6.09349,6.102732,6.111974,6.121216,6.130458,6.1396999999999995,6.148942,6.158184,6.167426000000001,6.176667999999999,6.18591,6.195152,6.204394000000001,6.213636,6.222878,6.23212,6.2413620000000005,6.250604,6.2598460000000005,6.269088,6.27833,6.287572,6.2968139999999995,6.306056,6.315298,6.324540000000001,6.333781999999999,6.343024,6.352266,6.361508000000001,6.37075,6.379992,6.389234,6.3984760000000005,6.407718,6.41696,6.426202,6.435444,6.444686,6.453928,6.46317,6.472412,6.481654000000001,6.490895999999999,6.500138,6.50938,6.518622000000001,6.527864,6.537106,6.546348,6.5555900000000005,6.564832,6.574074,6.583316,6.592558,6.6018,6.611042,6.620284,6.629526,6.638768000000001,6.64801,6.657252,6.666494,6.675736000000001,6.684978000000001,6.69422,6.703462,6.7127040000000004,6.721946,6.731188,6.74043,6.749672,6.758914,6.768156,6.777398,6.78664,6.795882000000001,6.805124,6.814366,6.823608,6.8328500000000005,6.842092000000001,6.851334,6.860576,6.869818,6.879060000000001,6.888302,6.897544,6.906786,6.916028000000001,6.925269999999999,6.934512,6.943754,6.952996000000001,6.962237999999999,6.97148,6.980722,6.9899640000000005,6.999206000000001,7.008448,7.01769,7.026932,7.036173999999999,7.0454159999999995,7.054658,7.0639,7.073142000000001,7.082383999999999,7.091626,7.100868,7.110110000000001,7.119352000000001,7.128594,7.137836,7.1470780000000005,7.156320000000001,7.165562000000001,7.174804,7.184046,7.193288000000001,7.202530000000001,7.211772,7.221014,7.230256000000001,7.239498000000001,7.24874,7.257982,7.267224000000001,7.276466000000001,7.285708,7.29495,7.3041920000000005,7.313434000000001,7.3226759999999995,7.331918,7.34116,7.350401999999999,7.359643999999999,7.368886,7.378128,7.387370000000001,7.396611999999999,7.405854,7.415096,7.4243380000000005,7.433580000000001,7.442822,7.452064,7.461306,7.470548000000001,7.479790000000001,7.489032,7.498274,7.507516000000001,7.516758000000001],"xaxis":"x","y":[0.03501162518204329,0.0359081115044337,0.036821464863166106,0.037751846596285665,0.03869941330101162,0.03966431642550804,0.04064670185172896,0.04164670947024458,0.042664472748013826,0.043700118290124795,0.04475376539657828,0.04582552561523791,0.04691550229211792,0.04802379012022089,0.04915047468817559,0.0502956320299585,0.05145932817701113,0.05264161871408864,0.05384254834019328,0.055062150435959675,0.0563004466388659,0.0575574464276458,0.058833146717274805,0.06012753146589089,0.06144057129499628,0.06277222312426607,0.06412242982225934,0.06549111987429819,0.06687820706874015,0.06828359020282483,0.06970715280922726,0.07114876290439481,0.07260827275968508,0.07408551869625876,0.07558032090461142,0.0770924832895573,0.07862179334140028,0.08016802203394863,0.08173092374994742,0.08331023623441708,0.08490568057629988,0.08651696121872683,0.08814376599812936,0.08978576621232594,0.09144261671762778,0.09311395605491275,0.0947994066045306,0.09649857476981127,0.09821105118886307,0.09993641097426298,0.10167421398015719,0.10342400509621563,0.10518531456780336,0.10695765834166632,0.10874053843635637,0.11053344333656412,0.1123358484104638,0.114147216349131,0.11596699762703848,0.11779463098260284,0.11962954391771605,0.12147115321516513,0.12331886547282903,0.12517207765351523,0.1270301776492991,0.12889254485921645,0.13075855077917128,0.1326275596029206,0.1344989288330219,0.1363720099006406,0.13824614879314948,0.1401206866884737,0.14199496059518102,0.14386830399734685,0.14574004750327713,0.14760951949721524,0.14947604679320783,0.1513389552903674,0.15319757062881312,0.15505121884564052,0.15689922703031817,0.15874092397897965,0.16057564084712586,0.1624027118003227,0.16422147466252465,0.16603127156171926,0.16783144957263205,0.16962136135628736,0.17140036579626008,0.1731678286315016,0.1749231230856536,0.17666563049280157,0.1783947409196438,0.1801098537840705,0.18181037847016845,0.18349573493966834,0.18516535433985903,0.18681867960798312,0.1884551660721209,0.1900742820485458,0.191675509435512,0.19325834430339764,0.19482229748109184,0.19636689513845926,0.19789167936467034,0.19939620874211833,0.2008800589155821,0.2023428231562216,0.20378411291991388,0.20520355839936016,0.20660080906930428,0.20797553422412018,0.2093274235069273,0.2106561874293078,0.2119615578805947,0.2132432886256164,0.21450115578967663,0.2157349583294632,0.21694451848848303,0.21812968223553347,0.21929031968463175,0.22042632549474966,0.22153761924761492,0.2226241458017835,0.22368587562111486,0.22472280507573245,0.22573495671350569,0.22672237950005317,0.22768514902523987,0.2286233676741252,0.22953716476031763,0.23042669661969367,0.23129214666246575,0.2321337253816098,0.23295167031571465,0.23374624596436758,0.23451774365427347,0.2352664813543786,0.23599280343838117,0.2366970803931155,0.23737970847142947,0.23804110928831265,0.23868172935918391,0.23930203957941462,0.23990253464433942,0.24048373240919707,0.2410461731886381,0.2415904189956515,0.24211705271997436,0.24262667724627907,0.2431199145126641,0.2435974045102137,0.2440598042246358,0.24450778652123994,0.24494203897475994,0.2453632626457887,0.2457721708058348,0.24616948761327334,0.24655594674270004,0.24693228997045494,0.24729926571931288,0.2476576275655785,0.2480081327120427,0.2483515404304817,0.2486886104775787,0.24902010148834708,0.2493467693513162,0.2496693655699041,0.2499886356145584,0.250305317270379,0.2506201389850589,0.2509338182220733,0.251247059824138,0.25156055439201114,0.2518749766837585,0.252190984039622,0.25250921483762784,0.25283028698505305,0.253154796450816,0.2534833158437984,0.2538163930420113,0.25415454987740393,0.2544982808809909,0.2548480520928065,0.25520429994102567,0.25556743019439393,0.25593781699188745,0.2563158019532957,0.2567016933741551,0.2570957655082028,0.25749825794021713,0.25790937505182143,0.2583292855825012,0.2587581222877595,0.25919598169599417,0.2596429239653264,0.2600989728412551,0.2605641157156441,0.2610383037871786,0.2615214523230523,0.26201344102127294,0.2625141144725957,0.2630232827207264,0.2635407219190519,0.26406617508181013,0.2645993529272349,0.2651399348098731,0.2656875697389268,0.26624187747914924,0.26680244973050476,0.2673688513825082,0.26794062183887263,0.26851727640783346,0.26909830775326726,0.26968318740149577,0.2702713672984718,0.27086228141184193,0.2714553473722485,0.2720499681480636,0.2726455337476764,0.27324142294333265,0.2738370050104906,0.27443164147661325,0.2750246878733055,0.2756154954857327,0.27620341309328894,0.2767877886955627,0.2773679712177349,0.2779433121896702,0.27851316739309917,0.27907689847146655,0.27963387449719707,0.28018347349135775,0.28072508389091744,0.28125810595905826,0.281781953134272,0.2822960533142479,0.2827998500708825,0.2832928037930321,0.283774392753983,0.28424411410094863,0.28470148476423396,0.2851460422840999,0.285577345553689,0.2859949754767641,0.2863985355393672,0.28678765229488756,0.2871619757623797,0.28752117973835656,0.28786496202262707,0.28819304455910666,0.2885051734928815,0.28880111914512535,0.2890806759078132,0.2893436620604679,0.2895899195114804,0.2898193134668342,0.2900317320293034,0.2902270857314708,0.2904053070061152,0.29056634959773087,0.29071018791913716,0.2908368163572841,0.2909462485325155,0.291038516515665,0.29111367000745286,0.2911717754847395,0.2912129153182173,0.29123718686617966,0.2912447015489804,0.29123558390880105,0.2912099706592874,0.291168009729562,0.2911098593070298,0.2910356868832961,0.2909456683073797,0.29083998685027757,0.2907188322847602,0.2905823999841141,0.29043089004334116,0.29026450642612955,0.29008345614069175,0.2898879484473191,0.28967819410028156,0.28945440462644134,0.28921679164269437,0.2889655662140943,0.28870093825424636,0.2884231159692888,0.2881323053465242,0.2878287096884668,0.28751252919284476,0.2871839605787967,0.2868431967592685,0.2864904265593608,0.28612583448012413,0.28574960050707937,0.2853618999625059,0.2849629034003294,0.28455277654224315,0.28413168025350277,0.28369977055667084,0.2832571986814096,0.2828041111482976,0.28234064988450097,0.2818669523690238,0.28138315180516804,0.2808893773177481,0.28038575417254663,0.2798724040154577,0.27934944512871784,0.27881699270163396,0.27827515911320344,0.2777240542240546,0.2771637856751571,0.2765944591908154,0.27601617888350916,0.27542904755823494,0.274833167014075,0.27422863834084144,0.273615562208739,0.27299403914911796,0.27236416982452094,0.27172605528635974,0.2710797972187134,0.27042549816686845,0.2697632617494048,0.2690931928527679,0.2684153978074244,0.2677299845448839,0.26703706273499894,0.26633674390313955,0.2656291415269807,0.2649143711127993,0.26419255025132427,0.2634637986533358,0.2627282381653362,0.2619859927657481,0.26123718854222544,0.26048195365077803,0.2597204182574957,0.25895271446378976,0.2581789762161193,0.25739933920127744,0.2566139407283487,0.25582291959853115,0.2550264159640337,0.2542245711773153,0.2534175276319391,0.2526054285963203,0.25178841804167096,0.2509666404654011,0.2501402407112426,0.24930936378731042,0.24847415468329132,0.2476347581878905,0.24679131870761395,0.24594398008790083,0.2450928854375429,0.24423817695725478,0.24337999577317787,0.24251848177600377,0.24165377346632053,0.240786007806685,0.239915320080829,0.23904184376030957,0.2381657103788118,0.23728704941421833,0.23640598817845915,0.2355226517150596,0.23463716270421442,0.23374964137512552,0.23286020542525115,0.23196896994604466,0.2310760473546735,0.23018154733115223,0.22928557676025,0.2283882396774899,0.22748963721849952,0.2265898675709414,0.22568902592820864,0.22478720444406827,0.2238844921873969,0.22298097509616677,0.22207673592983168,0.2211718542192843,0.22026640621356558,0.2193604648225492,0.2184540995548527,0.2175473764502741,0.21664035800611536,0.21573310309680013,0.21482566688627852,0.21391810073276907,0.21301045208549016,0.2121027643730945,0.21119507688363887,0.21028742463598857,0.20937983824268397,0.20847234376437593,0.20756496255605159,0.20665771110538034,0.20575060086360955,0.20484363806954806,0.20393682356729026,0.20303015261842058,0.20212361470955933,0.2012171933561976,0.20031086590386993,0.19940460332780294,0.1984983700322637,0.1975921236509177,0.19668581484957087,0.19577938713274848,0.194872776655614,0.19396591204278923,0.19305871421568055,0.19215109622994675,0.19124296312477512,0.19033421178564164,0.18942473082224845,0.18851440046331697,0.18760309246990792,0.18669067006891646,0.18577698790835012,0.18486189203596462,0.18394521990276375,0.18302680039281882,0.1821064538807794,0.18118399231836962,0.18025921935106548,0.17933193046605309,0.17840191317245185,0.1774689472146679,0.1765328048196256,0.17559325097847509,0.17465004376325138,0.17370293467880538,0.1727516690501736,0.17179598644540905,0.17083562113372075,0.16987030257862792,0.1688997559656453,0.16792370276387195,0.1669418613206751,0.16595394748850123,0.16495967528267866,0.16395875756890887,0.1629509067789852,0.1619358356531257,0.16091325800713757,0.15988288952250046,0.15884444855729607,0.15779765697578804,0.1567422409943216,0.15567793204109,0.15460446762720403,0.15352159222640152,0.1524290581606331,0.15132662648868594,0.1502140678949287,0.1490911635752049,0.1479577061168532,0.14681350036979415,0.1456583643056056,0.14449212986148813,0.1433146437660355,0.1421257683437291,0.14092538229511714,0.1397133814496624,0.1384896794883184,0.13725420863293894,0.13600692029973005,0.13474778571402118,0.13347679648375796,0.1321939651292233,0.13089932556662764,0.1295929335433455,0.12827486702272406,0.1269452265165518,0.12560413536344128,0.12425173995155783,0.12288820988430425,0.12151373808778039,0.12012854085900694,0.11873285785413158,0.11732695201602662,0.11591110944090201,0.11448563918377637,0.11305087300285235,0.11160716504307001,0.11015489145931989,0.10869444998001307,0.1072262594119229,0.10575075908741312,0.10426840825538143,0.102779685417443,0.10128508761107312,0.09978512964162332,0.09828034326529368],"yaxis":"y"},{"legendgroup":"2017","marker":{"color":"rgb(44, 160, 44)"},"mode":"lines","name":"2017","showlegend":true,"type":"scatter","x":[2.69300007820129,2.702688078403469,2.7123760786056477,2.722064078807827,2.731752079010006,2.7414400792121847,2.7511280794143635,2.760816079616543,2.7705040798187217,2.7801920800209006,2.7898800802230794,2.7995680804252583,2.8092560806274376,2.8189440808296164,2.8286320810317953,2.838320081233974,2.8480080814361535,2.8576960816383323,2.867384081840511,2.87707208204269,2.886760082244869,2.896448082447048,2.906136082649227,2.915824082851406,2.925512083053585,2.935200083255764,2.944888083457943,2.954576083660122,2.9642640838623007,2.9739520840644795,2.983640084266659,2.9933280844688377,3.0030160846710166,3.0127040848731954,3.0223920850753747,3.0320800852775536,3.0417680854797324,3.0514560856819113,3.0611440858840906,3.0708320860862695,3.0805200862884483,3.090208086490627,3.099896086692806,3.1095840868949853,3.119272087097164,3.128960087299343,3.138648087501522,3.1483360877037008,3.15802408790588,3.167712088108059,3.177400088310238,3.1870880885124167,3.1967760887145955,3.206464088916775,3.2161520891189537,3.2258400893211325,3.235528089523312,3.2452160897254907,3.2549040899276696,3.2645920901298484,3.2742800903320273,3.2839680905342066,3.2936560907363854,3.3033440909385643,3.313032091140743,3.322720091342922,3.3324080915451013,3.34209609174728,3.351784091949459,3.3614720921516383,3.371160092353817,3.380848092555996,3.390536092758175,3.4002240929603538,3.409912093162533,3.419600093364712,3.429288093566891,3.4389760937690697,3.4486640939712485,3.458352094173428,3.4680400943756067,3.4777280945777855,3.487416094779965,3.4971040949821437,3.5067920951843226,3.5164800953865014,3.5261680955886803,3.5358560957908596,3.5455440959930384,3.5552320961952173,3.564920096397396,3.574608096599575,3.5842960968017543,3.593984097003933,3.603672097206112,3.613360097408291,3.6230480976104698,3.632736097812649,3.642424098014828,3.6521120982170068,3.6618000984191856,3.6714880986213645,3.681176098823544,3.6908640990257227,3.7005520992279015,3.710240099430081,3.7199280996322597,3.7296160998344385,3.739304100036618,3.7489921002387963,3.7586801004409756,3.7683681006431544,3.7780561008453333,3.7877441010475126,3.797432101249691,3.8071201014518703,3.816808101654049,3.826496101856228,3.8361841020584073,3.8458721022605857,3.855560102462765,3.865248102664944,3.8749361028671228,3.884624103069302,3.894312103271481,3.9040001034736598,3.9136881036758386,3.9233761038780175,3.933064104080197,3.9427521042823757,3.9524401044845545,3.962128104686734,3.9718161048889122,3.9815041050910915,3.9911921052932704,4.000880105495449,4.010568105697629,4.020256105899807,4.029944106101986,4.039632106304165,4.049320106506344,4.059008106708523,4.068696106910702,4.078384107112881,4.08807210731506,4.097760107517239,4.107448107719418,4.117136107921597,4.126824108123776,4.136512108325954,4.1462001085281335,4.155888108730313,4.165576108932491,4.1752641091346705,4.18495210933685,4.194640109539028,4.2043281097412075,4.214016109943387,4.223704110145565,4.2333921103477445,4.243080110549923,4.252768110752102,4.262456110954282,4.27214411115646,4.281832111358639,4.291520111560818,4.301208111762997,4.310896111965176,4.320584112167355,4.330272112369534,4.339960112571713,4.349648112773892,4.359336112976071,4.36902411317825,4.378712113380429,4.388400113582608,4.3980881137847865,4.407776113986966,4.417464114189144,4.4271521143913235,4.436840114593503,4.446528114795681,4.4562161149978605,4.46590411520004,4.475592115402218,4.4852801156043975,4.494968115806576,4.504656116008755,4.514344116210934,4.524032116413113,4.533720116615292,4.543408116817471,4.55309611701965,4.562784117221829,4.572472117424008,4.582160117626187,4.591848117828366,4.601536118030545,4.611224118232724,4.6209121184349025,4.630600118637082,4.64028811883926,4.6499761190414395,4.659664119243619,4.669352119445797,4.6790401196479765,4.688728119850156,4.698416120052334,4.708104120254513,4.717792120456693,4.727480120658871,4.73716812086105,4.74685612106323,4.756544121265408,4.766232121467587,4.775920121669766,4.785608121871945,4.795296122074124,4.804984122276303,4.814672122478482,4.824360122680661,4.83404812288284,4.843736123085019,4.853424123287198,4.863112123489376,4.872800123691556,4.882488123893735,4.892176124095913,4.9018641242980925,4.911552124500272,4.92124012470245,4.9309281249046295,4.940616125106809,4.950304125308987,4.9599921255111665,4.969680125713346,4.979368125915524,4.989056126117703,4.998744126319882,5.008432126522061,5.01812012672424,5.027808126926419,5.037496127128598,5.047184127330777,5.056872127532956,5.066560127735135,5.076248127937314,5.085936128139492,5.095624128341672,5.105312128543851,5.115000128746029,5.1246881289482085,5.134376129150388,5.144064129352566,5.1537521295547455,5.163440129756925,5.173128129959103,5.1828161301612825,5.192504130363462,5.20219213056564,5.211880130767819,5.221568130969998,5.231256131172177,5.240944131374356,5.250632131576535,5.260320131778714,5.270008131980893,5.279696132183072,5.289384132385251,5.29907213258743,5.308760132789608,5.318448132991788,5.328136133193967,5.337824133396145,5.3475121335983244,5.357200133800504,5.366888134002682,5.3765761342048615,5.386264134407041,5.395952134609219,5.4056401348113985,5.415328135013578,5.425016135215756,5.434704135417935,5.444392135620114,5.454080135822293,5.463768136024472,5.473456136226651,5.48314413642883,5.492832136631009,5.502520136833188,5.512208137035367,5.521896137237546,5.531584137439724,5.541272137641904,5.550960137844083,5.560648138046261,5.57033613824844,5.58002413845062,5.589712138652798,5.5994001388549774,5.609088139057157,5.618776139259335,5.628464139461514,5.638152139663694,5.647840139865872,5.657528140068051,5.66721614027023,5.676904140472409,5.686592140674588,5.696280140876767,5.705968141078946,5.715656141281125,5.725344141483304,5.735032141685483,5.744720141887662,5.75440814208984,5.76409614229202,5.773784142494199,5.783472142696377,5.793160142898556,5.802848143100736,5.812536143302915,5.822224143505094,5.831912143707273,5.841600143909451,5.8512881441116305,5.86097614431381,5.870664144515988,5.8803521447181675,5.890040144920347,5.899728145122525,5.9094161453247045,5.919104145526884,5.928792145729062,5.938480145931241,5.948168146133421,5.957856146335599,5.967544146537778,5.977232146739957,5.986920146942136,5.996608147144315,6.006296147346494,6.015984147548673,6.025672147750852,6.035360147953031,6.04504814815521,6.054736148357389,6.064424148559567,6.074112148761746,6.083800148963926,6.093488149166104,6.1031761493682835,6.112864149570463,6.122552149772641,6.1322401499748205,6.141928150177,6.151616150379178,6.161304150581357,6.170992150783537,6.180680150985715,6.190368151187894,6.200056151390073,6.209744151592252,6.219432151794431,6.22912015199661,6.238808152198789,6.248496152400968,6.258184152603147,6.267872152805326,6.277560153007505,6.287248153209683,6.296936153411862,6.306624153614042,6.31631215381622,6.326000154018399,6.335688154220579,6.345376154422757,6.3550641546249365,6.364752154827116,6.374440155029294,6.384128155231473,6.393816155433653,6.403504155635831,6.41319215583801,6.422880156040189,6.432568156242368,6.442256156444547,6.451944156646726,6.461632156848905,6.471320157051084,6.481008157253263,6.490696157455442,6.500384157657621,6.510072157859799,6.519760158061978,6.529448158264158,6.539136158466336,6.548824158668515,6.558512158870695,6.568200159072873,6.577888159275052,6.587576159477232,6.59726415967941,6.606952159881589,6.616640160083769,6.626328160285947,6.636016160488126,6.645704160690305,6.655392160892484,6.665080161094663,6.674768161296842,6.684456161499021,6.6941441617012,6.703832161903378,6.713520162105558,6.723208162307737,6.732896162509915,6.742584162712095,6.752272162914274,6.761960163116452,6.771648163318632,6.781336163520811,6.791024163722989,6.800712163925169,6.810400164127348,6.820088164329526,6.829776164531706,6.839464164733885,6.849152164936063,6.858840165138242,6.868528165340422,6.8782161655426,6.887904165744779,6.897592165946959,6.907280166149137,6.916968166351316,6.926656166553496,6.936344166755674,6.946032166957853,6.955720167160033,6.965408167362211,6.97509616756439,6.984784167766568,6.994472167968748,7.004160168170927,7.013848168373105,7.023536168575285,7.033224168777464,7.042912168979642,7.052600169181822,7.062288169384001,7.071976169586179,7.0816641697883576,7.091352169990538,7.101040170192716,7.110728170394895,7.120416170597075,7.130104170799253,7.139792171001432,7.149480171203612,7.15916817140579,7.168856171607969,7.178544171810149,7.188232172012327,7.197920172214506,7.207608172416684,7.217296172618864,7.226984172821043,7.236672173023221,7.246360173225401,7.25604817342758,7.265736173629758,7.275424173831938,7.285112174034117,7.294800174236295,7.3044881744384735,7.314176174640654,7.323864174842832,7.333552175045011,7.343240175247191,7.352928175449369,7.362616175651548,7.372304175853728,7.381992176055906,7.391680176258085,7.401368176460265,7.411056176662443,7.420744176864622,7.4304321770668,7.44012017726898,7.449808177471159,7.459496177673337,7.469184177875517,7.478872178077696,7.488560178279874,7.498248178482054,7.507936178684233,7.517624178886411,7.5273121790885895],"xaxis":"x","y":[0.021928490209875837,0.02249329811750618,0.0230716227586607,0.023663817344266994,0.024270238110816834,0.0248912435831455,0.025527193797079187,0.026178449483497453,0.026845371215574912,0.027528318521185,0.028227648962660574,0.02894371718631798,0.0296768739443546,0.030427465091928382,0.031195830562418016,0.03198230332404313,0.03278720832119628,0.03361086140399654,0.034453568249723614,0.03531562327992348,0.03619730857709766,0.03709889280499011,0.038020630136574,0.03896275919391112,0.039925502004106564,0.040909062975616445,0.041913627899178636,0.042939362977631804,0.04398641388885929,0.04505490488604956,0.04614493793939543,0.04725659192326676,0.04838992185278024,0.049544958173560846,0.050721706108337865,0.05192014506384835,0.05314022810133,0.05438188147367566,0.05564500423209611,0.056929467904890105,0.05823511625066266,0.05956176508805331,0.06090920220374792,0.06227718734024233,0.06366545226451231,0.06507370091842044,0.06650160965135422,0.0679488275352529,0.06941497676183332,0.07089965312147578,0.07240242656288272,0.07392284183226867,0.0754604191904946,0.07701465520621338,0.07858502362275273,0.08017097629613183,0.0817719442012822,0.08338733850323396,0.08501655168972691,0.08665895876142084,0.08831391847561226,0.08998077463910933,0.09165885744568598,0.0933474848533244,0.0950459639962598,0.09675359262667618,0.09846966058075679,0.10019345126366931,0.1019242431479747,0.10366131127987685,0.1054039287876879,0.1071513683868704,0.10890290387602596,0.11065781161824104,0.11241537200226301,0.11417487087807582,0.11593560096155969,0.1176968632030666,0.11945796811490922,0.12121823705296171,0.12297700344777902,0.12473361398089242,0.12648742970218782,0.12823782708456466,0.12998419901236619,0.13172595570038997,0.1334625255406167,0.1351933558741439,0.13691791368616274,0.138635686222185,0.14034618152409953,0.14204892888501985,0.14374347922226566,0.14542940536820703,0.1471063022790904,0.14877378716234213,0.15043149952323173,0.15207910113214662,0.15371627591409978,0.15534272976244445,0.15695819027911914,0.15856240644407218,0.16015514821683974,0.1617362060735426,0.1633053904828567,0.16486253132476902,0.16640747725617897,0.16794009502761975,0.16946026875557552,0.17096789915504215,0.17246290273713039,0.1739452109766295,0.17541476945455434,0.17687153698076533,0.17831548470179662,0.17974659519905106,0.18116486158250528,0.18257028658504376,0.18396288166246944,0.18534266610416192,0.18670966615923795,0.1880639141829312,0.18940544780775587,0.19073430914382733,0.19205054401251334,0.1933542012173631,0.1946453318560128,0.1959239886765083,0.19719022548119702,0.1984440965810512,0.19968565630296867,0.20091495855227592,0.20213205643232424,0.2033370019227215,0.20452984561739532,0.2057106365233208,0.20687942192038813,0.20803624728251446,0.20918115625974665,0.2103141907207296,0.21143539085455734,0.2125447953306675,0.21364244151508452,0.2147283657409798,0.2158026036311776,0.21686519046992153,0.21791616162089916,0.21895555298823358,0.21998340151687235,0.2209997457285392,0.2220046262891765,0.2229980866035819,0.22398017343274218,0.22495093752919149,0.22591043428556187,0.22685872439136784,0.2277958744929612,0.22872195785151064,0.22963705499381162,0.23054125435070638,0.23143465287789317,0.23231735665394063,0.23318948145037516,0.23405115326880147,0.23490250884012634,0.23574369608110365,0.23657487450358344,0.23739621557204957,0.23820790300525194,0.23901013301798887,0.23980311449936947,0.24058706912418584,0.24136223139434157,0.24212884860762926,0.2428871807515103,0.24363750031993034,0.2443800920516015,0.24511525258859526,0.2458432900545173,0.24656452355196776,0.24727928257944154,0.24798790636827345,0.24869074314069306,0.24938814929050815,0.2500804884884048,0.2507681307143033,0.25145145121966095,0.2521308294230633,0.25280664774287886,0.25347929037116623,0.2541491419934482,0.2548165864593388,0.2554820054093913,0.25614577686387807,0.2568082737795384,0.25746986258062454,0.25813090167084607,0.25879173993304444,0.2594527152236384,0.2601141528690452,0.26077636417141353,0.2614396449311061,0.26210427399341335,0.2627705118270099,0.26343859914162326,0.26410875555233954,0.2647811782978382,0.2654560410197204,0.2661334926098892,0.26681365613271735,0.26749662782845424,0.2681824762040263,0.2688712412170144,0.26956293355822203,0.27025753403781133,0.2709549930795293,0.27165523032706096,0.27235813436602263,0.27306356256456776,0.2737713410350044,0.27448126471823836,0.275193097592247,0.27590657300516386,0.27662139413292586,0.2773372345607968,0.27805373898742886,0.27877052404948655,0.2794871792642232,0.2802032680867527,0.28091832907815617,0.28163187717994753,0.28234340508984057,0.28305238473319577,0.28375826882399324,0.2844604925086642,0.2851584750856479,0.28585162179310325,0.2865393256568023,0.28722096938988595,0.287895927335846,0.28856356744583694,0.2892232532812127,0.28987434603200257,0.2905162065419547,0.29114819733069164,0.2917696846035383,0.29238004023961367,0.29297864374889976,0.2935648841891371,0.2941381620336277,0.29469789098127075,0.29524349970047875,0.29577443349897864,0.2962901559119151,0.2967901502011187,0.2972739207589026,0.29774099441027996,0.2981909216080697,0.2986232775159427,0.29903766297511253,0.2994337053510065,0.2998110592569394,0.30016940715249607,0.30050845981503305,0.30082795668341306,0.3011276660738039,0.3014073852680761,0.30166694047604264,0.3019061866734674,0.30212500731845804,0.302323313949503,0.3025010456690568,0.30265816851717536,0.3027946747402849,0.30291058196070025,0.3030059322530096,0.3030807911339092,0.303135246472475,0.30316940732823716,0.30318340272472394,0.3031773803664402,0.3031515053074189,0.3031059585796882,0.3030409357900755,0.30295664569383046,0.30285330875356037,0.30273115569189357,0.3025904260462015,0.3024313667335342,0.30225423063371926,0.30205927519831643,0.3018467610928097,0.3016169508790808,0.30137010774480155,0.3011064942859688,0.3008263713483403,0.30052999693302673,0.30021762517099404,0.29988950537066794,0.29954588114228103,0.2991869896020194,0.2988130606584486,0.2984243163830864,0.2980209704664071,0.2976032277599697,0.2971712839047543,0.2967253250452437,0.29626552762819575,0.2957920582845221,0.29530507379217014,0.29480472111739037,0.29429113753131997,0.2937644507983743,0.29322477943252023,0.2926722330171589,0.29210691258401383,0.29152891104612194,0.29093831367980183,0.2903351986502565,0.2897196375753173,0.2890916961217347,0.28845143462832784,0.2877989087503184,0.28713417011916065,0.28645726701226737,0.2857682450271215,0.285067147754424,0.2843540174450935,0.28362889566616467,0.28289182394088586,0.2821428443685868,0.28138200022021836,0.2806093365057821,0.2798249005102454,0.2790287422948923,0.2782209151614804,0.2774014760769493,0.2765704860568548,0.27572801050611373,0.2748741195160645,0.27400888811725843,0.27313239648780263,0.2722447301174898,0.27134597992831483,0.27043624235237135,0.26951561936845303,0.26858421849904013,0.2676421527696317,0.2666895406326961,0.26572650585874147,0.2647531773972501,0.2637696892104088,0.2627761800827223,0.2617727934097441,0.2607596769692335,0.25973698267812273,0.2587048663387018,0.2576634873774195,0.25661300857966607,0.2555535958238221,0.25448541781776723,0.25340864584089734,0.25232345349454655,0.25123001646352316,0.25012851229124616,0.24901912017075029,0.24790202075356435,0.2467773959781988,0.24564542891969046,0.2445063036613624,0.2433602051896418,0.24220731931246803,0.24104783260151233,0.23988193235810862,0.23870980660247956,0.23753164408553876,0.2363476343222477,0.235157967645211,0.23396283527692294,0.23276242941881553,0.23155694335501528,0.2303465715684896,0.22913150986707004,0.22791195551665708,0.2266881073787608,0.2254601660494069,0.224228333996344,0.22299281569140914,0.22175381773488378,0.2205115489686479,0.21926622057497155,0.2180180461578212,0.2167672418036496,0.21551402611873297,0.2142586202402598,0.21300124781854216,0.21174213496789504,0.21048151018395475,0.20921960422543215,0.20795664995855545,0.20669288216272969,0.20542853729623095,0.2041638532210587,0.20289906888638665,0.20163442397037837,0.20037015848047357,0.19910651231258344,0.19784372476998133,0.19658203404301378,0.1953216766511021,0.19406288684882703,0.192805895998231,0.19155093190977743,0.19029821815471729,0.18904797335189996,0.18780041043234252,0.18655573588512306,0.18531414898839718,0.18407584102955696,0.18284099451872948,0.18160978239998218,0.180382367264737,0.17915890057200967,0.17793952188016418,0.17672435809493067,0.1755135227384597,0.17430711524417475,0.17310522028215067,0.1719079071196891,0.17071522902164604,0.16952722269497178,0.16834390778174507,0.16716528640482584,0.16599134277002597,0.1648220428284878,0.16365733400269022,0.16249714497922987,0.1613413855712322,0.16018994665292574,0.1590427001685803,0.15789949921765972,0.15676017821768243,0.15562455314589638,0.15449242186050877,0.15336356450180688,0.15223774397311213,0.1511147065011197,0.14999418227476702,0.14887588616138459,0.14775951849848323,0.1466447659591454,0.14553130248861748,0.14441879030931082,0.14330688099108507,0.14219521658332498,0.14108343080500635,0.13997115028862597,0.13885799587359143,0.13774358394438535,0.13662752780857335,0.1355094391095052,0.1343889292683477,0.13326561094992054,0.13213909954665468,0.13100901467486764,0.12987498167746012,0.12873663312706785,0.127593610323665,0.12644556478060875,0.12529215969312613,0.12413307138329525,0.12296799071565148,0.12179662447763702,0.12061869671925798,0.11943395004645854,0.11824214686289676,0.11704307055502824,0.11583652661561067,0.1146223437010104,0.11340037461794861,0.11217049723562185,0.11093261531944398,0.10968665928296256,0.10843258685486622,0.10717038365833133,0.10590006370032579,0.10462166976886651,0.10333527373659562,0.10204097676943487,0.10073890943946232,0.09942923174155208,0.09811213301370325,0.09678783176137706,0.0954565753865517,0.09411863982257954,0.09277432907630434,0.09142397467927124,0.09006793505020648,0.08870659477128684,0.08734036378106148],"yaxis":"y"},{"legendgroup":"2018","marker":{"color":"rgb(214, 39, 40)"},"mode":"lines","name":"2018","showlegend":true,"type":"scatter","x":[2.905,2.9144539999999997,2.923908,2.933362,2.9428159999999997,2.95227,2.961724,2.9711779999999997,2.980632,2.990086,2.9995399999999997,3.008994,3.018448,3.0279019999999996,3.037356,3.04681,3.0562639999999996,3.065718,3.075172,3.0846259999999996,3.09408,3.103534,3.1129879999999996,3.122442,3.131896,3.1413499999999996,3.150804,3.160258,3.1697119999999996,3.179166,3.18862,3.1980739999999996,3.207528,3.216982,3.2264359999999996,3.23589,3.245344,3.2547979999999996,3.264252,3.273706,3.2831599999999996,3.292614,3.302068,3.311522,3.320976,3.33043,3.3398839999999996,3.349338,3.3587919999999998,3.368246,3.3777,3.3871539999999998,3.3966079999999996,3.406062,3.4155159999999998,3.42497,3.434424,3.4438779999999998,3.4533319999999996,3.462786,3.4722399999999998,3.481694,3.491148,3.5006019999999998,3.5100559999999996,3.51951,3.5289639999999998,3.538418,3.547872,3.5573259999999998,3.5667799999999996,3.576234,3.5856879999999998,3.595142,3.604596,3.6140499999999998,3.6235039999999996,3.632958,3.6424119999999998,3.651866,3.66132,3.6707739999999998,3.6802279999999996,3.689682,3.6991359999999998,3.70859,3.718044,3.7274979999999998,3.7369519999999996,3.746406,3.7558599999999998,3.765314,3.774768,3.7842219999999998,3.7936759999999996,3.80313,3.8125839999999998,3.822038,3.831492,3.8409459999999997,3.8504,3.859854,3.8693079999999997,3.878762,3.888216,3.8976699999999997,3.9071239999999996,3.916578,3.9260319999999997,3.935486,3.94494,3.9543939999999997,3.9638479999999996,3.973302,3.982756,3.99221,4.001664,4.011118,4.020572,4.030025999999999,4.039479999999999,4.048934,4.058388,4.067842,4.077296,4.08675,4.096204,4.105658,4.115112,4.124566,4.13402,4.143473999999999,4.152928,4.162382,4.171836,4.18129,4.1907440000000005,4.200198,4.209652,4.219106,4.22856,4.238014,4.247468,4.256921999999999,4.266376,4.27583,4.285284,4.294738,4.304192,4.313646,4.3231,4.332554,4.342008,4.351462,4.360916,4.370369999999999,4.379824,4.389278,4.398732,4.408186,4.4176400000000005,4.427094,4.436548,4.446002,4.455456,4.46491,4.474364,4.483818,4.493272,4.502726,4.51218,4.521634,4.5310880000000004,4.540542,4.549996,4.55945,4.568904,4.578358,4.587812,4.597265999999999,4.60672,4.616174,4.625628,4.635082,4.6445359999999996,4.65399,4.663444,4.672898,4.682352,4.691806,4.7012599999999996,4.710714,4.720168,4.729622,4.739076,4.74853,4.757984,4.767438,4.776892,4.786346,4.7958,4.805254,4.8147079999999995,4.824161999999999,4.833616,4.84307,4.852524,4.861978,4.871432,4.880886,4.89034,4.899794,4.909248,4.918702,4.9281559999999995,4.937609999999999,4.947063999999999,4.956518,4.965972,4.975426,4.98488,4.994334,5.003788,5.013242,5.022696,5.03215,5.0416039999999995,5.051057999999999,5.060512,5.069966,5.07942,5.088874000000001,5.098328,5.107782,5.117236,5.12669,5.136144,5.145598,5.1550519999999995,5.164506,5.173959999999999,5.183414,5.192868,5.202322000000001,5.211776,5.22123,5.230684,5.240138,5.249592,5.259046,5.2684999999999995,5.277953999999999,5.287408,5.296862,5.306316000000001,5.31577,5.325224,5.334678,5.344132,5.353586,5.36304,5.372494,5.3819479999999995,5.391401999999999,5.400856,5.41031,5.419764,5.4292180000000005,5.438672,5.448126,5.45758,5.467034,5.476488,5.485942,5.4953959999999995,5.50485,5.514304,5.523758,5.533212,5.5426660000000005,5.55212,5.561574,5.571028,5.580482,5.589936,5.59939,5.6088439999999995,5.618298,5.627752,5.637206,5.646660000000001,5.656114,5.665568,5.675022,5.684476,5.69393,5.703384,5.712838,5.722292,5.731745999999999,5.7412,5.750654,5.760108,5.7695620000000005,5.779016,5.78847,5.797924,5.807378,5.816832,5.826286,5.8357399999999995,5.845194,5.854648,5.864102,5.873556,5.8830100000000005,5.892464,5.901918,5.911372,5.920826,5.93028,5.939734,5.9491879999999995,5.958642,5.968095999999999,5.97755,5.987004000000001,5.9964580000000005,6.005912,6.015366,6.02482,6.034274,6.043728,6.053182,6.062636,6.072089999999999,6.081544,6.090998,6.100452000000001,6.1099060000000005,6.11936,6.128814,6.138268,6.147722,6.157176,6.16663,6.1760839999999995,6.185537999999999,6.194992,6.204446000000001,6.2139,6.2233540000000005,6.232808,6.242262,6.251716,6.26117,6.270624,6.280078,6.2895319999999995,6.298986,6.30844,6.317894,6.327348000000001,6.3368020000000005,6.346256,6.35571,6.365164,6.374618,6.384072,6.393526,6.4029799999999994,6.412434,6.421888,6.431342,6.440796000000001,6.4502500000000005,6.459704,6.469158,6.478612,6.488066,6.49752,6.506974,6.516428,6.525881999999999,6.535336,6.544790000000001,6.554244,6.5636980000000005,6.573152,6.582606,6.59206,6.601514,6.610968,6.620422,6.629875999999999,6.63933,6.648784,6.658238,6.667692000000001,6.6771460000000005,6.6866,6.696054,6.705508,6.714962,6.724416,6.73387,6.743323999999999,6.752778,6.762232,6.771686,6.781140000000001,6.7905940000000005,6.800048,6.809502,6.818956,6.82841,6.837864,6.847318,6.856772,6.866225999999999,6.87568,6.885134000000001,6.894588000000001,6.9040420000000005,6.913496,6.92295,6.932404,6.941858,6.951312,6.960766,6.970219999999999,6.979674000000001,6.989127999999999,6.998582000000001,7.008036000000001,7.0174900000000004,7.026944,7.036398,7.045852,7.055306,7.06476,7.074214,7.083668000000001,7.093121999999999,7.102575999999999,7.112030000000001,7.121484000000001,7.130938,7.140392,7.149846,7.1593,7.168754,7.178208,7.1876619999999996,7.197115999999999,7.206570000000001,7.216024000000001,7.225478000000001,7.234932000000001,7.244386,7.25384,7.263294,7.272748,7.282202,7.291656,7.3011099999999995,7.310563999999999,7.320017999999999,7.329472000000001,7.338926000000001,7.348380000000001,7.357834,7.367288,7.376742,7.386196,7.39565,7.405104,7.4145579999999995,7.424012000000001,7.433466000000001,7.442919999999999,7.452374000000001,7.461828000000001,7.471282,7.480736,7.49019,7.499644,7.509098,7.518552,7.5280059999999995,7.537459999999999,7.546914000000001,7.556368000000001,7.565822000000001,7.575276000000001,7.58473,7.594184,7.603638,7.613092,7.622546],"xaxis":"x","y":[0.04084095486202727,0.04183274719292237,0.04283637639718852,0.04385160616208755,0.04487818979192338,0.045915870541733086,0.04696438198613589,0.04802344842366375,0.04909278531678544,0.05017209976771713,0.051261091029990366,0.0523594510556119,0.0534668650775175,0.054583012226875546,0.055707566184648345,0.05684019586666628,0.05798056614131189,0.05912833857874905,0.06028317223046717,0.06144472443774536,0.06261265166746804,0.06378661037355685,0.06496625788211058,0.06615125329817459,0.06734125843189194,0.06853593874162166,0.06973496429144423,0.07093801072031355,0.07214476021996159,0.07335490251850778,0.07456813586658537,0.07578416802266022,0.07700271723408941,0.07822351321035105,0.07944629808476952,0.08067082736096409,0.08189687084016528,0.08312421352547615,0.08435265649909553,0.08558201776848215,0.08681213307741345,0.08804285667787926,0.08927406205876409,0.09050564262729374,0.09173751233926428,0.09296960627413597,0.09420188115115363,0.09543431578275446,0.09666691146164463,0.09789969227806346,0.09913270536391196,0.10036602106060163,0.10159973300767319,0.10283395814945281,0.1040688366572473,0.10530453176483101,0.10654122951524728,0.1077791384172362,0.10901848900989847,0.11025953333452827,0.11150254431287662,0.11274781503145435,0.1139956579318383,0.11524640390731507,0.11650040130657213,0.11775801484553186,0.11901962442881499,0.1202856238827175,0.12155641960198177,0.12283242911304823,0.12411407955686855,0.12540180609476467,0.1266960502412101,0.1279972581277991,0.12930587870304916,0.1306223618730578,0.13194715658838685,0.13328070888290205,0.13462345987062116,0.1359758437069419,0.137338285520912,0.13871119932548232,0.14009498591293357,0.14149003074289632,0.14289670183058642,0.1443153476430537,0.14574629501138764,0.14718984706694535,0.14864628120974716,0.15011584711724715,0.1515987648017017,0.15309522272435316,0.15460537597459406,0.15612934452220276,0.1576672115506204,0.15921902187909148,0.16078478048130343,0.16236445110793857,0.16395795502029553,0.16556516984184733,0.16718592853427827,0.16882001850418432,0.1704671808462325,0.17212710972815382,0.17379945192249502,0.1754838064895757,0.1771797246155924,0.17888670960928332,0.1806042170600107,0.18233165515955196,0.18406838518928997,0.18581372217388958,0.18756693570192495,0.18932725091328517,0.19109384965254383,0.19286587178683076,0.19464241668608623,0.19642254486292873,0.19820527976871224,0.19998960974170343,0.20177449010266804,0.20355884539253086,0.20534157174615006,0.20712153939566097,0.20889759529624796,0.21066856586666166,0.2124332598362494,0.21419047118977882,0.2159389822008361,0.2176775665441576,0.21940499247682813,0.22112002607791315,0.222821434535757,0.22450798947188075,0.22617847029016788,0.22783166753981168,0.2294663862803376,0.23108144943689782,0.23267570113396288,0.23424800999551512,0.23579727239987489,0.23732241567736456,0.23882240123914086,0.24029622762569916,0.2417429334637726,0.24316160032061943,0.24455135544501042,0.2459113743845826,0.24724088346964262,0.2485391621539452,0.24980554520346546,0.25103942472472063,0.2522402520247558,0.2534075392955245,0.25454086111601965,0.25563985576619136,0.25670422634737355,0.2577337417046645,0.2587282371474493,0.2596876149650154,0.2606118447349752,0.26150096342301615,0.2623550752732783,0.26317435148946655,0.26395902970760937,0.2647094132621721,0.2654258702480269,0.2661088323815713,0.2667587936650575,0.2673763088589478,0.26796199176785646,0.26851651334634513,0.2690405996315323,0.2695350295101331,0.2700006323281762,0.2704382853522331,0.27084891109155257,0.2712334744910064,0.2715929800052227,0.2719284685647178,0.27224101444520793,0.27253172205162324,0.2728017226286301,0.2730521709096959,0.2732842417169126,0.27349912652393066,0.2736980299944196,0.2738821665085024,0.27405275668957946,0.2742110239438679,0.2743581910248559,0.2744954766346756,0.2746240920741668,0.27474523795311445,0.27486010097180374,0.2749698507846676,0.2750756369563658,0.27517858602017126,0.27527979864803986,0.27538034694118885,0.2754812718494405,0.2755835807269711,0.27568824503147793,0.2757961981731136,0.2759083335188471,0.2760255025572234,0.27614851322776374,0.2762781284185295,0.276415064634635,0.2765599908397523,0.27671352747191186,0.2768762456341661,0.27704866645994564,0.2772312606522225,0.27742444819487416,0.27762859823396324,0.27784402912595807,0.27807100864927553,0.27830975437490213,0.2785604341912453,0.2788231669778005,0.2790980234216846,0.2793850269705903,0.2796841549152422,0.2799953395940236,0.2803184697120537,0.2806533917666525,0.28099991157083165,0.2813577958662051,0.281726774016489,0.2821065397726003,0.2824967531002555,0.28289704206087535,0.283307004736591,0.2837262111901492,0.2841542054505767,0.2845905075155679,0.28503461536169444,0.28548600695372245,0.2859441422445471,0.2864084651574993,0.2868784055430896,0.2873533811025571,0.2878327992709711,0.28831605905299196,0.28880255280482947,0.28929166795634975,0.28978278866775325,0.29027529741569613,0.29076857650423976,0.29126200949648007,0.2917549825632478,0.2922468857457592,0.29273711412963127,0.2932250689281926,0.2937101584735314,0.29419179911424187,0.29466941601935,0.2951424438883665,0.2956103275679397,0.29607252257602135,0.2965284955349391,0.2969777245151951,0.29741969929223305,0.29785392151881523,0.29827990481602595,0.29869717478626295,0.2991052689519109,0.2995037366236802,0.2998921387028693,0.30027004742205043,0.3006370460288839,0.3009927284179599,0.3013366987157084,0.3016685708235513,0.301987967924555,0.3022945219589149,0.3025878730736303,0.30286766905174795,0.30313356472651,0.3033852213857208,0.3036223061715555,0.30384449148094006,0.3040514543715067,0.3042428759779998,0.3044184409438147,0.3045778368721938,0.304720753801378,0.3048468837078091,0.30495592004122635,0.30504755729526795,0.30512149061690585,0.3051774154577936,0.3052150272703015,0.3052340212507472,0.30523409213201136,0.3052149340274616,0.30517624032776747,0.30511770365191926,0.3050390158534432,0.3049398680825131,0.3048199509043507,0.3046789544740245,0.30451656876745625,0.30433248386817097,0.30412639030904637,0.30389797946804414,0.30364694401666353,0.3033729784195898,0.303075779483789,0.3027550469550599,0.3024104841598537,0.30204179868995223,0.301648703127424,0.3012309158070871,0.3007881616135461,0.30032017280973206,0.2998266898937166,0.299307462480474,0.29876225020513164,0.29819082364417593,0.2975929652509831,0.296968470301987,0.29631714784973395,0.2956388216790488,0.2949333312624831,0.29420053271123564,0.2934402997177023,0.29265252448584606,0.2918371186455877,0.29099401414746173,0.2901231641338253,0.2892245437829627,0.28829815112250995,0.2873440078086859,0.2863621598679324,0.2853526783976348,0.28431566022274135,0.2832512285051901,0.28215953330319965,0.2810407520776153,0.27989509014263947,0.27872278105844966,0.2775240869633535,0.2762992988433059,0.2750487367367961,0.27377274987329125,0.27247171674361603,0.2711460451008405,0.2697961718904574,0.26842256310883134,0.26702571358910726,0.26560614671400173,0.264164414055088,0.26270109493843663,0.26121679593667513,0.25971215028776756,0.2581878172410359,0.2566444813311706,0.25508285158120886,0.25350366063567575,0.25190766382531943,0.2502956381650794,0.24866838128715352,0.24702671031124085,0.24537146065424914,0.24370348478195855,0.24202365090532915,0.2403328416243386,0.23863195252241517,0.23692189071470535,0.23520357335358988,0.2334779260950126,0.2317458815293352,0.23000837758056958,0.22826635587796412,0.22652076010402222,0.22477253432315283,0.22302262129521652,0.22127196077831918,0.2195214878252563,0.2177721310780612,0.21602481106513977,0.21428043850548523,0.212539912624476,0.2108041194857396,0.20907393034353747,0.20735020002008364,0.2056337653121538,0.20392544343125818,0.20222603048158258,0.20053629997978145,0.19885700142060392,0.19718885889220444,0.19553256974484756,0.19388880331656405,0.1922581997191515,0.19064136868774476,0.1890388884969854,0.1874513049466379,0.18587913041929038,0.18432284301257754,0.18278288574813115,0.18125966585926276,0.1797535541591393,0.1782648844909932,0.17679395326166308,0.1753410190595417,0.1739063023577502,0.1724899853031399,0.17109221159147012,0.16971308642888583,0.16835267657957387,0.16701101049926018,0.1656880785539675,0.16438383332324483,0.16309818998684916,0.16183102679365624,0.16058218561137702,0.1593514725554352,0.15813865869519889,0.15694348083555626,0.15576564237165455,0.1546048142144559,0.15346063578460822,0.1523327160719738,0.15122063475802436,0.15012394339817486,0.14904216666101855,0.14797480362129964,0.1469213291033678,0.14588119507176986,0.14485383206553756,0.1438386506726713,0.14283504304124875,0.1418423844235315,0.14086003474940126,0.13988734022541838,0.13892363495576251,0.13796824258130552,0.13702047793304165,0.13607964869611006,0.13514505708063504,0.13421600149563373,0.13329177822224586,0.13237168308258207,0.1314550131005065,0.13054106815071084,0.12962915259249405,0.1287185768846989,0.12780865917832485,0.12689872688339926,0.12598811820675543,0.12507618365744969,0.12416228751662221,0.12324580926870099,0.12232614499093947,0.12140270869837001,0.12047493364137152,0.11954227355314348,0.11860420384450582,0.11766022274354929,0.11670985237779892,0.11575263979666778,0.11478815793212295,0.11381600649561585,0.11283581280947472,0.11184723257110511,0.11084995054849099,0.10984368120565148,0.10882816925685719,0.10780319014858655,0.10676855046835382,0.10572408827972472,0.10466967338299549,0.10360520750119646,0.10253062439125026,0.101445889880303,0.10035100182741728,0.09924599001100856,0.0981309159425783,0.09700587260748987,0.09587098413370947,0.09472640538961306,0.09357232151215139,0.0924089473668287,0.091236526941138,0.09005533267325931,0.08886566471800197,0.08766785015212922,0.08646224212137069,0.08524921893157006,0.08402918308657575,0.08280256027560658,0.08156979831297477,0.08033136603315479,0.07908775214431911,0.07783946404355378,0.07658702659707854,0.0753309808888713],"yaxis":"y"},{"legendgroup":"2019","marker":{"color":"rgb(148, 103, 189)"},"mode":"lines","name":"2019","showlegend":true,"type":"scatter","x":[2.853,2.862832,2.8726640000000003,2.882496,2.892328,2.9021600000000003,2.911992,2.9218240000000004,2.9316560000000003,2.941488,2.9513200000000004,2.9611520000000002,2.970984,2.9808160000000004,2.990648,3.00048,3.0103120000000003,3.020144,3.029976,3.0398080000000003,3.04964,3.0594720000000004,3.0693040000000003,3.079136,3.0889680000000004,3.0988,3.108632,3.1184640000000003,3.128296,3.138128,3.1479600000000003,3.157792,3.167624,3.1774560000000003,3.187288,3.19712,3.2069520000000002,3.216784,3.2266160000000004,3.236448,3.24628,3.2561120000000003,3.265944,3.275776,3.2856080000000003,3.29544,3.3052720000000004,3.3151040000000003,3.324936,3.3347680000000004,3.3446000000000002,3.354432,3.3642640000000004,3.374096,3.383928,3.3937600000000003,3.403592,3.4134240000000005,3.4232560000000003,3.433088,3.4429200000000004,3.4527520000000003,3.462584,3.4724160000000004,3.4822480000000002,3.4920800000000005,3.5019120000000004,3.511744,3.5215760000000005,3.5314080000000003,3.54124,3.5510720000000005,3.5609040000000003,3.570736,3.5805680000000004,3.5904000000000003,3.600232,3.6100640000000004,3.6198960000000002,3.629728,3.6395600000000004,3.649392,3.659224,3.6690560000000003,3.678888,3.68872,3.6985520000000003,3.708384,3.7182160000000004,3.7280480000000003,3.7378800000000005,3.7477120000000004,3.757544,3.7673760000000005,3.7772080000000003,3.78704,3.7968720000000005,3.8067040000000003,3.816536,3.8263680000000004,3.8362000000000003,3.846032,3.8558640000000004,3.8656960000000002,3.875528,3.8853600000000004,3.895192,3.905024,3.9148560000000003,3.9246880000000006,3.93452,3.9443520000000003,3.9541840000000006,3.964016,3.9738480000000003,3.9836800000000006,3.9935120000000004,4.003344,4.0131760000000005,4.023008,4.03284,4.0426720000000005,4.052504000000001,4.062336,4.0721680000000005,4.082000000000001,4.091832,4.101664,4.111496000000001,4.121328,4.13116,4.140992000000001,4.150824,4.160656,4.170488000000001,4.18032,4.190152,4.199984000000001,4.209816,4.219648,4.229480000000001,4.239312,4.249144,4.2589760000000005,4.268808,4.27864,4.2884720000000005,4.298304,4.308136,4.3179680000000005,4.3278,4.337632,4.347464,4.357296,4.367128,4.37696,4.386792,4.396624,4.406456,4.416288,4.42612,4.435952,4.445784000000001,4.455616000000001,4.465448,4.475280000000001,4.485112,4.494944,4.504776000000001,4.514608000000001,4.52444,4.5342720000000005,4.544104,4.553936,4.5637680000000005,4.573600000000001,4.583432,4.5932640000000005,4.603096000000001,4.612928,4.62276,4.632592000000001,4.642424,4.652256,4.662088000000001,4.67192,4.681752,4.691584000000001,4.701416,4.711248,4.721080000000001,4.730912,4.740744,4.750576000000001,4.760408,4.77024,4.7800720000000005,4.789904,4.799736,4.8095680000000005,4.8194,4.829232,4.8390640000000005,4.848896,4.858728,4.86856,4.878392,4.888224000000001,4.898056,4.907888000000001,4.917720000000001,4.927552,4.937384,4.947216000000001,4.957048,4.96688,4.976712,4.986544,4.996376000000001,5.006208000000001,5.01604,5.025872000000001,5.035704000000001,5.045536,5.0553680000000005,5.065200000000001,5.075032,5.084864,5.094696000000001,5.104528,5.1143600000000005,5.124192000000001,5.134024,5.143856,5.153688000000001,5.16352,5.173352,5.183184000000001,5.193016,5.202848,5.212680000000001,5.222512,5.232344,5.242176000000001,5.252008,5.26184,5.271672000000001,5.281504,5.291336,5.3011680000000005,5.311,5.320832000000001,5.3306640000000005,5.340496,5.350328,5.3601600000000005,5.369992,5.379824,5.3896560000000004,5.399488,5.409320000000001,5.419152,5.428984000000001,5.438816000000001,5.448648,5.45848,5.468312000000001,5.478144,5.487976,5.497808,5.50764,5.517472000000001,5.527304000000001,5.537136,5.546968000000001,5.556800000000001,5.566632,5.5764640000000005,5.586296000000001,5.596128,5.60596,5.615792000000001,5.625624,5.6354560000000005,5.645288000000001,5.65512,5.664952,5.674784000000001,5.684616,5.694448,5.704280000000001,5.714112,5.723944000000001,5.733776000000001,5.743608,5.75344,5.763272000000001,5.773104,5.782936,5.792768000000001,5.8026,5.812432,5.8222640000000006,5.832096,5.841928000000001,5.8517600000000005,5.861592,5.871424000000001,5.8812560000000005,5.891088,5.90092,5.9107520000000005,5.920584,5.930416000000001,5.940248,5.950080000000001,5.959912000000001,5.969744,5.979576,5.989408000000001,5.99924,6.009072,6.018904000000001,6.028736,6.038568000000001,6.048400000000001,6.058232,6.068064000000001,6.077896000000001,6.087728,6.0975600000000005,6.107392000000001,6.117224,6.127056,6.136888000000001,6.14672,6.1565520000000005,6.166384000000001,6.176216,6.186048,6.195880000000001,6.205712,6.215544,6.225376000000001,6.235208,6.245040000000001,6.254872000000001,6.264704,6.274536,6.284368000000001,6.2942,6.304032,6.313864000000001,6.323696,6.333528,6.3433600000000006,6.353192000000001,6.363024000000001,6.3728560000000005,6.382688,6.392520000000001,6.4023520000000005,6.412184,6.422016,6.4318480000000005,6.44168,6.451512000000001,6.461344,6.471176000000001,6.481008000000001,6.49084,6.500672000000001,6.510504000000001,6.520336,6.530168,6.540000000000001,6.549832,6.559664000000001,6.569496000000001,6.579328,6.589160000000001,6.598992000000001,6.608824,6.6186560000000005,6.628488000000001,6.63832,6.6481520000000005,6.657984000000001,6.667816,6.6776480000000005,6.687480000000001,6.697312,6.707144,6.716976000000001,6.726808,6.73664,6.746472000000001,6.756304,6.766136000000001,6.775968000000001,6.7858,6.795632000000001,6.805464000000001,6.815296,6.825128,6.834960000000001,6.844792,6.854624000000001,6.864456000000001,6.874288,6.884120000000001,6.8939520000000005,6.903784,6.913616000000001,6.9234480000000005,6.93328,6.943112000000001,6.9529440000000005,6.962776000000002,6.972608000000001,6.98244,6.992272,7.002104000000001,7.011936,7.021768,7.031600000000001,7.041432,7.051264,7.061096000000001,7.070928,7.08076,7.090592000000001,7.100424,7.110256,7.120088000000001,7.12992,7.139752000000001,7.149584000000001,7.159416,7.169248000000001,7.179080000000001,7.188912,7.198744000000001,7.208576000000001,7.218408,7.228240000000001,7.238072000000001,7.247904,7.257736000000001,7.267568000000001,7.2774,7.2872319999999995,7.297064000000001,7.306896,7.3167279999999995,7.326560000000001,7.336392000000002,7.346224000000001,7.356056000000001,7.365888,7.375720000000001,7.385552000000001,7.395384,7.405216000000001,7.4150480000000005,7.42488,7.434712000000001,7.4445440000000005,7.454376,7.464208000000001,7.4740400000000005,7.483872,7.493704000000001,7.503536,7.513368,7.523200000000001,7.533032,7.542864000000002,7.552696000000001,7.562528,7.5723600000000015,7.582192000000001,7.592024,7.6018560000000015,7.611688000000001,7.62152,7.6313520000000015,7.641184000000001,7.651016,7.660848,7.670680000000001,7.680512,7.690344,7.700176000000001,7.710008,7.71984,7.729672000000001,7.739504,7.749336,7.759168000000001],"xaxis":"x","y":[0.036031848497998335,0.036897625198576534,0.03777170647406768,0.03865378325745079,0.03954353897718955,0.04044065022274473,0.0413447874486308,0.042255615716193726,0.043172795472127615,0.04409598336258602,0.04502483308157844,0.045958996252179425,0.04689812333891079,0.047841864589494174,0.048789871004006204,0.04974179532930875,0.05069729307646649,0.05165602355870934,0.052617650947346994,0.05358184534289662,0.05454828385854418,0.05551665171292972,0.05648664332911727,0.057457963436496964,0.05843032817225624,0.05940346617895853,0.06037711969468215,0.061351045632091455,0.06232501664274948,0.06329882216292812,0.06427226943712853,0.06524518451550024,0.06621741322133183,0.06718882208478578,0.06815929923906446,0.06912875527522347,0.07009712405188977,0.07106436345620072,0.07203045611235233,0.07299541003423035,0.07395925921870135,0.07492206417625312,0.07588391239580676,0.07684491874066246,0.07780522577270083,0.07876500400212688,0.07972445206023168,0.08068379679283258,0.08164329327226556,0.0826032247260136,0.08356390238028266,0.08452566521707207,0.0854888796435282,0.08645393907262151,0.08742126341444685,0.08839129847770694,0.08936451528121168,0.09034140927549712,0.091322499474945,0.09230832750106271,0.09329945653786574,0.09429647020058206,0.09529997131918636,0.09631058063854284,0.09732893543722305,0.09835568806733384,0.09939150441796618,0.10043706230514221,0.10149304979140115,0.1025601634384189,0.10363910649631128,0.1047305870335076,0.10583531601132086,0.10695400530756459,0.10808736569378417,0.1092361047708773,0.11040092486807636,0.11158252091044782,0.11278157826024415,0.11399877053760052,0.11523475742622188,0.11649018246984373,0.11776567086537193,0.11906182725872053,0.12037923354945908,0.12171844671046816,0.12307999662886415,0.12446438397451043,0.12587207810246606,0.12730351499574608,0.12875909525477097,0.13023918213987157,0.13174409967318704,0.13327413080624917,0.13482951565947995,0.13641044983975334,0.1380170828420706,0.13964951654128147,0.14130780377965108,0.14299194705591692,0.14470189732130534,0.14643755288779403,0.14819875845368435,0.1499853042513285,0.1517969253216026,0.15363330091944977,0.15549405405452923,0.15737875117070743,0.15928690196779363,0.16121795936858735,0.16317131963394008,0.16514632262815926,0.1671422522366782,0.16915833693751575,0.1711937505276123,0.17324761300468866,0.1753189916048198,0.1774069019954449,0.17951030962304867,0.1816281312142673,0.1837592364286611,0.18590244966089153,0.1880565519895271,0.19022028326918008,0.19239234436215574,0.1945713995052709,0.19675607880698273,0.19894498086944734,0.20113667552961154,0.20332970671294773,0.20552259539293713,0.20771384264893095,0.2099019328145495,0.21208533670833712,0.2142625149379466,0.21643192126873884,0.21859200604728693,0.22074121966992327,0.22287801608614666,0.2250008563264072,0.22710821204352888,0.22919856905680824,0.23127043088764257,0.2333223222753903,0.23535279266207346,0.23736041963446244,0.23934381231207313,0.24130161466964098,0.24323250878270894,0.24513521798509486,0.24700850992718368,0.24885119952420634,0.2506621517839448,0.2524402845036216,0.2541845708261097,0.25589404164599966,0.2575677878565453,0.2592049624290054,0.260804782316453,0.26236653017472666,0.2638895558938291,0.2653732779337465,0.26681718445937896,0.26822083427000826,0.2695838575194966,0.27090595622420727,0.27218690455645694,0.27342654892213786,0.27462480782201365,0.2757816714970309,0.2768972013588846,0.2779715292079147,0.2790048562413105,0.27999745185543284,0.2809496522469364,0.2818618588181954,0.28273453639336193,0.2835682112521685,0.28436346898935855,0.2851209522083522,0.2858413580584532,0.28652543562555455,0.2871739831869093,0.2877878453410965,0.2883679100248213,0.28891510542864324,0.2894303968241309,0.2899147833152778,0.2903692945272916,0.29079498724609193,0.2911929420219928,0.291564259751142,0.2919100582482953,0.29223146882447093,0.29252963288289685,0.2928056985464978,0.2930608173299094,0.29329614086870054,0.2935128177181119,0.2937119902331714,0.29389479154156756,0.2940623426200966,0.29421574948490403,0.2943561005050779,0.2944844638484585,0.29460188506778295,0.29470938483448983,0.2948079568267152,0.2948985657771437,0.2949821456855201,0.2950595981997333,0.295131791168479,0.2951995573675915,0.29526369340122033,0.2953249587781001,0.29538407516225945,0.29544172579660455,0.29549855509692957,0.2955551684130438,0.2956121319528664,0.295669972864527,0.2957291794707416,0.29579020164900516,0.29585345135043895,0.2959193032495043,0.29598809551618976,0.296060130701754,0.2961356767286167,0.2962149679745728,0.29629820644114796,0.2963855629956197,0.29647717867598794,0.2965731660480275,0.296673610603448,0.29677857218815573,0.29688808644965164,0.2970021662926974,0.2971208033325428,0.2972439693352491,0.2973716176349189,0.2975036845180083,0.29764009056529517,0.2977807419425456,0.2979255316314323,0.29807434059282534,0.29822703885517443,0.29838348652136265,0.2985435346880806,0.2987070262724955,0.2988737967417433,0.299043674741508,0.2992164826207804,0.2993920368506543,0.29957014833584567,0.2997506226184238,0.2999332599740624,0.3001178554019245,0.30030419851007506,0.3004920732991332,0.30068125784758765,0.30087152390298766,0.3010626363838866,0.3012543527981252,0.30144642258366433,0.30163858637878543,0.3018305752290282,0.3020221097387614,0.30221289917572375,0.3024026405373058,0.3025910175876826,0.30277769987522063,0.3029623417398189,0.30314458132003785,0.3033240395699844,0.303500319296003,0.30367300422320875,0.3038416581018635,0.30400582386346536,0.3041650228362579,0.3043187540296265,0.3044664934965638,0.30460769378304114,0.3047417834727201,0.30486816683499957,0.3049862235838793,0.30509530875459595,0.3051947527043849,0.3052838612430964,0.30536191589874484,0.3054281743223477,0.30548187083570694,0.3055222171250213,0.30554840308244163,0.3055595977969009,0.3055549506947334,0.30553359282978076,0.30549463832188095,0.3054371859417821,0.3053603208397409,0.3052631164142171,0.30514463631630767,0.3050039365847572,0.30484006790562224,0.3046520779899298,0.30443901406194707,0.3041999254500025,0.3039338662711369,0.3036398982002666,0.3033170933139523,0.3029645369983492,0.30258133091042405,0.3021665959810978,0.30171947544858846,0.30123913790989065,0.3007247803780669,0.3001756313327985,0.2995909537514855,0.2989700481080806,0.2983122553268053,0.2976169596779075,0.2968835916026932,0.29611163045521643,0.29530060714818307,0.29445010669090343,0.29355977060742205,0.29262929922332487,0.29165845381015476,0.29064705857683093,0.2895950024980003,0.28850224096982063,0.2873687972843001,0.28619476391397886,0.28498030359944493,0.2837256502329152,0.28243110953189504,0.28109705949773345,0.2797239506547092,0.27831230606617924,0.27686272112513977,0.2753758631174926,0.27385247055717926,0.272293352293266,0.27069938638997026,0.26907151878153035,0.2674107617047274,0.2657181919127554,0.26399494867502726,0.2622422315683569,0.26046129806580753,0.2586534609303093,0.2568200854209367,0.2549625863204858,0.2530824247937254,0.2511811050863481,0.24926017107531423,0.24732120268184896,0.2453658121589095,0.24339564026542712,0.2414123523400735,0.23941763428768617,0.23741318849181894,0.2354007296671623,0.2333819806657876,0.23135866825133727,0.22933251885537423,0.22730525433014814,0.22527858771201525,0.2232542190096739,0.2212338310312355,0.21921908526396813,0.2172116178202911,0.2152130354633043,0.21322491172477745,0.21124878312811846,0.20928614552839023,0.2073384505809402,0.20540710234967405,0.20349345406540328,0.20159880504410552,0.199724397774254,0.1978714151817007,0.19604097807989146,0.1942341428124361,0.19245189909432153,0.1906951680572662,0.1889648005039318,0.18726157537491864,0.18558619843165688,0.18393930115751317,0.18232143987861782,0.18073309510513375,0.17917467109288313,0.1776464956244787,0.17614882000834506,0.17468181929326157,0.17324559269534118,0.17184016423365894,0.17046548357006405,0.1691214270480656,0.16780779892507997,0.16652433279172218,0.16527069317129986,0.16404647729215233,0.16285121702499378,0.16168438097700763,0.1605453767340322,0.15943355324182845,0.1583482033171194,0.15728856627881588,0.15625383068961635,0.15524313719799368,0.15425558147043333,0.15329021720368333,0.15234605920672953,0.15142208654217498,0.15051724571672673,0.1496304539105551,0.1487606022353672,0.14790655901118185,0.14706717305193853,0.14624127695027225,0.1454276903520064,0.14462522321116153,0.14383267901656147,0.14304885798140402,0.14227256018750406,0.14150258867623713,0.14073775247858658,0.13997686957706765,0.13921876979268633,0.138462297590496,0.13770631479772633,0.13694970322887115,0.13619136721254838,0.13543023601537563,0.13466526615852753,0.13389544362307473,0.1331197859406364,0.13233734416628998,0.131547204731121,0.13074849117219509,0.12994036573814846,0.1291220308689953,0.12829273054913087,0.12745175153289862,0.12659842444245084,0.12573212473799136,0.1248522735608252,0.12395833844998087,0.12304983393347407,0.12212632199558777,0.12118741242183075,0.12023276302350498,0.11926207974406888,0.11827511664972239,0.11727167580686895,0.11625160704931677,0.11521480763827452,0.1141612218183833,0.11309084027318418,0.11200369948357909,0.11089988099297506,0.10977951058292247,0.10864275736317898,0.1074898327802127,0.10632098954825281,0.10513652050706458,0.10393675741068482,0.10272206965140296,0.10149286292331344,0.10024957782979006,0.09899268843924845,0.09772270079357737,0.09644015137361149,0.09514560552601027,0.09383965585589123,0.09252292058953657,0.0911960419114558,0.08985968428005417,0.08851453272609278,0.08716129113808702,0.08580068053871671,0.08443343735625333,0.0830603116949433,0.08168206560819807,0.08029947137835716,0.07891330980670387,0.07752436851731231,0.07613344027820335,0.07474132134318634,0.07334880981764504,0.07195670405141985,0.07056580106180947,0.06917689498960292,0.06779077559091783,0.06640822676749186,0.06503002513794268,0.06365693865237522,0.062289725252565493,0.060929131579822965],"yaxis":"y"},{"legendgroup":"2015","marker":{"color":"rgb(31, 119, 180)","symbol":"line-ns-open"},"mode":"markers","name":"2015","showlegend":false,"type":"scatter","x":[7.587,7.561,7.527,7.522,7.427,7.406,7.378,7.364,7.286,7.284,7.278,7.226,7.2,7.187,7.119,6.983,6.946,6.94,6.937,6.901,6.867,6.853,6.81,6.798,6.786,6.75,6.67,6.611,6.575,6.574,6.505,6.485,6.477,6.455,6.411,6.329,6.302,6.298,6.295,6.269,6.168,6.13,6.123,6.003,5.995,5.987,5.984,5.975,5.96,5.948,5.89,5.889,5.878,5.855,5.848,5.833,5.828,5.824,5.813,5.791,5.77,5.759,5.754,5.716,5.709,5.695,5.689,5.605,5.589,5.548,5.477,5.474,5.429,5.399,5.36,5.332,5.286,5.268,5.253,5.212,5.194,5.192,5.192,5.14,5.129,5.124,5.123,5.102,5.098,5.073,5.057,5.013,5.007,4.971,4.959,4.949,4.898,4.885,4.876,4.874,4.867,4.857,4.839,4.8,4.788,4.786,4.739,4.715,4.694,4.686,4.681,4.677,4.642,4.633,4.61,4.571,4.565,4.55,4.518,4.517,4.514,4.512,4.507,4.436,4.419,4.369,4.35,4.332,4.307,4.297,4.292,4.271,4.252,4.218,4.194,4.077,4.033,3.995,3.989,3.956,3.931,3.904,3.896,3.845,3.819,3.781,3.681,3.678,3.667,3.656,3.655,3.587,3.575,3.465,3.34,3.006,2.905,2.839],"xaxis":"x","y":["2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015","2015"],"yaxis":"y2"},{"legendgroup":"2016","marker":{"color":"rgb(255, 127, 14)","symbol":"line-ns-open"},"mode":"markers","name":"2016","showlegend":false,"type":"scatter","x":[7.526,7.509,7.501,7.498,7.413,7.404,7.339,7.334,7.313,7.291,7.267,7.119,7.104,7.087,7.039,6.994,6.952,6.929,6.907,6.871,6.778,6.739,6.725,6.705,6.701,6.65,6.596,6.573,6.545,6.488,6.481,6.478,6.474,6.379,6.379,6.375,6.361,6.355,6.324,6.269,6.239,6.218,6.168,6.084,6.078,6.068,6.005,5.992,5.987,5.977,5.976,5.956,5.921,5.919,5.897,5.856,5.835,5.835,5.822,5.813,5.802,5.771,5.768,5.743,5.658,5.648,5.615,5.56,5.546,5.538,5.528,5.517,5.51,5.488,5.458,5.44,5.401,5.389,5.314,5.303,5.291,5.279,5.245,5.196,5.185,5.177,5.163,5.161,5.155,5.151,5.145,5.132,5.129,5.123,5.121,5.061,5.057,5.045,5.033,4.996,4.907,4.876,4.875,4.871,4.813,4.795,4.793,4.754,4.655,4.643,4.635,4.575,4.574,4.513,4.508,4.459,4.415,4.404,4.395,4.362,4.36,4.356,4.324,4.276,4.272,4.252,4.236,4.219,4.217,4.201,4.193,4.156,4.139,4.121,4.073,4.028,3.974,3.956,3.916,3.907,3.866,3.856,3.832,3.763,3.739,3.739,3.724,3.695,3.666,3.622,3.607,3.515,3.484,3.36,3.303,3.069,2.905],"xaxis":"x","y":["2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016","2016"],"yaxis":"y2"},{"legendgroup":"2017","marker":{"color":"rgb(44, 160, 44)","symbol":"line-ns-open"},"mode":"markers","name":"2017","showlegend":false,"type":"scatter","x":[7.53700017929077,7.52199983596802,7.50400018692017,7.49399995803833,7.4689998626709,7.3769998550415,7.31599998474121,7.31400012969971,7.28399991989136,7.28399991989136,7.21299982070923,7.0789999961853,7.00600004196167,6.99300003051758,6.97700023651123,6.95100021362305,6.89099979400635,6.86299991607666,6.71400022506714,6.65199995040894,6.64799976348877,6.63500022888184,6.60900020599365,6.59899997711182,6.57800006866455,6.57200002670288,6.52699995040894,6.4539999961853,6.4539999961853,6.4520001411438,6.44199991226196,6.42399978637695,6.42199993133545,6.40299987792969,6.375,6.35699987411499,6.3439998626709,6.16800022125244,6.10500001907349,6.09800004959106,6.08699989318848,6.08400011062622,6.07100009918213,6.00799989700317,6.00299978256226,5.97300004959106,5.97100019454956,5.96400022506714,5.96299982070923,5.95599985122681,5.92000007629395,5.90199995040894,5.87200021743774,5.84999990463257,5.83799982070923,5.83799982070923,5.82499980926514,5.82299995422363,5.82200002670288,5.81899976730347,5.80999994277954,5.75799989700317,5.71500015258789,5.62900018692017,5.62099981307983,5.61100006103516,5.56899976730347,5.52500009536743,5.5,5.49300003051758,5.47200012207031,5.42999982833862,5.39499998092651,5.33599996566772,5.32399988174438,5.31099987030029,5.29300022125244,5.27899980545044,5.27299976348877,5.26900005340576,5.26200008392334,5.25,5.23699998855591,5.2350001335144,5.23400020599365,5.23000001907349,5.22700023651123,5.22499990463257,5.19500017166138,5.18200016021729,5.18100023269653,5.17500019073486,5.15100002288818,5.07399988174438,5.07399988174438,5.04099988937378,5.01100015640259,5.00400018692017,4.96199989318848,4.95499992370605,4.8289999961853,4.80499982833862,4.77500009536743,4.7350001335144,4.71400022506714,4.70900011062622,4.69500017166138,4.69199991226196,4.64400005340576,4.60799980163574,4.57399988174438,4.55299997329712,4.55000019073486,4.54500007629395,4.53499984741211,4.51399993896484,4.49700021743774,4.46500015258789,4.46000003814697,4.44000005722046,4.37599992752075,4.31500005722046,4.29199981689453,4.29099988937378,4.28599977493286,4.28000020980835,4.19000005722046,4.17999982833862,4.16800022125244,4.13899993896484,4.11999988555908,4.09600019454956,4.08099985122681,4.03200006484985,4.02799987792969,3.97000002861023,3.93600010871887,3.875,3.80800008773804,3.79500007629395,3.79399991035461,3.76600003242493,3.65700006484985,3.64400005340576,3.6029999256134,3.59299993515015,3.59100008010864,3.53299999237061,3.50699996948242,3.49499988555908,3.47099995613098,3.46199989318848,3.34899997711182,2.90499997138977,2.69300007820129],"xaxis":"x","y":["2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017","2017"],"yaxis":"y2"},{"legendgroup":"2018","marker":{"color":"rgb(214, 39, 40)","symbol":"line-ns-open"},"mode":"markers","name":"2018","showlegend":false,"type":"scatter","x":[7.632,7.594,7.555,7.495,7.487,7.441,7.328,7.324,7.314,7.272,7.19,7.139,7.072,6.977,6.965,6.927,6.91,6.886,6.814,6.774,6.711,6.627,6.489,6.488,6.476,6.441,6.43,6.419,6.388,6.382,6.379,6.374,6.371,6.343,6.322,6.31,6.26,6.192,6.173,6.167,6.141,6.123,6.105,6.096,6.083,6.072,6.0,5.973,5.956,5.952,5.948,5.945,5.933,5.915,5.891,5.89,5.875,5.835,5.81,5.79,5.762,5.752,5.739,5.681,5.663,5.662,5.64,5.636,5.62,5.566,5.524,5.504,5.483,5.483,5.472,5.43,5.41,5.398,5.358,5.358,5.347,5.321,5.302,5.295,5.254,5.246,5.201,5.199,5.185,5.161,5.155,5.131,5.129,5.125,5.103,5.093,5.082,4.982,4.975,4.933,4.88,4.806,4.758,4.743,4.724,4.707,4.671,4.657,4.631,4.623,4.592,4.586,4.571,4.559,4.5,4.471,4.456,4.447,4.441,4.433,4.424,4.419,4.417,4.41,4.377,4.356,4.35,4.34,4.321,4.308,4.301,4.245,4.19,4.166,4.161,4.141,4.139,4.103,3.999,3.964,3.808,3.795,3.774,3.692,3.632,3.59,3.587,3.582,3.495,3.462,3.408,3.355,3.303,3.254,3.083,2.905],"xaxis":"x","y":["2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018","2018"],"yaxis":"y2"},{"legendgroup":"2019","marker":{"color":"rgb(148, 103, 189)","symbol":"line-ns-open"},"mode":"markers","name":"2019","showlegend":false,"type":"scatter","x":[7.769,7.6,7.554,7.494,7.488,7.48,7.343,7.307,7.278,7.246,7.228,7.167,7.139,7.09,7.054,7.021,6.985,6.923,6.892,6.852,6.825,6.726,6.595,6.592,6.446,6.444,6.436,6.375,6.374,6.354,6.321,6.3,6.293,6.262,6.253,6.223,6.199,6.198,6.192,6.182,6.174,6.149,6.125,6.118,6.105,6.1,6.086,6.07,6.046,6.028,6.021,6.008,5.94,5.895,5.893,5.89,5.888,5.886,5.86,5.809,5.779,5.758,5.743,5.718,5.697,5.693,5.653,5.648,5.631,5.603,5.529,5.525,5.523,5.467,5.432,5.43,5.425,5.386,5.373,5.339,5.323,5.287,5.285,5.274,5.265,5.261,5.247,5.211,5.208,5.208,5.197,5.192,5.191,5.175,5.082,5.044,5.011,4.996,4.944,4.913,4.906,4.883,4.812,4.799,4.796,4.722,4.719,4.707,4.7,4.696,4.681,4.668,4.639,4.628,4.587,4.559,4.548,4.534,4.519,4.516,4.509,4.49,4.466,4.461,4.456,4.437,4.418,4.39,4.374,4.366,4.36,4.35,4.332,4.286,4.212,4.189,4.166,4.107,4.085,4.015,3.975,3.973,3.933,3.802,3.775,3.663,3.597,3.488,3.462,3.41,3.38,3.334,3.231,3.203,3.083,2.853],"xaxis":"x","y":["2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019","2019"],"yaxis":"y2"}],                        {"barmode":"overlay","hovermode":"closest","legend":{"traceorder":"reversed"},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y2","domain":[0.0,1.0],"zeroline":false},"yaxis":{"anchor":"free","domain":[0.35,1],"position":0.0},"yaxis2":{"anchor":"x","domain":[0,0.25],"dtick":1,"showticklabels":false}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e9dcf19e-6096-41e3-8dff-eaef4628e1c8');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python

fig = px.box(df_2019, y="Happiness.Score",points="all")
fig.show()
```


<div>                            <div id="42b145a3-55e6-4e02-8162-7b24eb97ddf0" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("42b145a3-55e6-4e02-8162-7b24eb97ddf0")) {                    Plotly.newPlot(                        "42b145a3-55e6-4e02-8162-7b24eb97ddf0",                        [{"alignmentgroup":"True","boxpoints":"all","hovertemplate":"Happiness.Score=%{y}<extra></extra>","legendgroup":"","marker":{"color":"#636efa"},"name":"","notched":false,"offsetgroup":"","orientation":"v","showlegend":false,"type":"box","x0":" ","xaxis":"x","y":[7.769,7.6,7.554,7.494,7.488,7.48,7.343,7.307,7.278,7.246,7.228,7.167,7.139,7.09,7.054,7.021,6.985,6.923,6.892,6.852,6.825,6.726,6.595,6.592,6.446,6.444,6.436,6.375,6.374,6.354,6.321,6.3,6.293,6.262,6.253,6.223,6.199,6.198,6.192,6.182,6.174,6.149,6.125,6.118,6.105,6.1,6.086,6.07,6.046,6.028,6.021,6.008,5.94,5.895,5.893,5.89,5.888,5.886,5.86,5.809,5.779,5.758,5.743,5.718,5.697,5.693,5.653,5.648,5.631,5.603,5.529,5.525,5.523,5.467,5.432,5.43,5.425,5.386,5.373,5.339,5.323,5.287,5.285,5.274,5.265,5.261,5.247,5.211,5.208,5.208,5.197,5.192,5.191,5.175,5.082,5.044,5.011,4.996,4.944,4.913,4.906,4.883,4.812,4.799,4.796,4.722,4.719,4.707,4.7,4.696,4.681,4.668,4.639,4.628,4.587,4.559,4.548,4.534,4.519,4.516,4.509,4.49,4.466,4.461,4.456,4.437,4.418,4.39,4.374,4.366,4.36,4.35,4.332,4.286,4.212,4.189,4.166,4.107,4.085,4.015,3.975,3.973,3.933,3.802,3.775,3.663,3.597,3.488,3.462,3.41,3.38,3.334,3.231,3.203,3.083,2.853],"y0":" ","yaxis":"y"}],                        {"boxmode":"group","legend":{"tracegroupgap":0},"margin":{"t":60},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"xaxis":{"anchor":"y","domain":[0.0,1.0]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"Happiness.Score"}}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('42b145a3-55e6-4e02-8162-7b24eb97ddf0');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>
