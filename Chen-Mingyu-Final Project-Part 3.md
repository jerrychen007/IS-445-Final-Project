# IS 445 - Final Project: City of Rockford Crime Offenses Analysis


```python
%matplotlib inline
```


```python
import pandas as pd
import matplotlib.pyplot as pyplot
import plotly.graph_objs as go
import numpy as np
from ipywidgets import interact, interactive, fixed, interact_manual, widgets
import seaborn as sns
import traitlets
import palettable
import cufflinks as cf
import warnings
warnings.filterwarnings('ignore')
```


```python
data = pd.read_csv('cm_offense_archive.csv')
data['Occurred Date'] = data['Occurred Date'].astype(str).str[:-4].astype(np.int64)
data
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
      <th>CaseNo</th>
      <th>Description</th>
      <th>Occurred Date</th>
      <th>Occurred Day of Week</th>
      <th>Occurred Time</th>
      <th>X</th>
      <th>Y</th>
      <th>Crime Against Code</th>
      <th>Crime Against</th>
      <th>Group</th>
      <th>Beat</th>
      <th>Ward</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>16-009615</td>
      <td>90C - Disorderly Conduct</td>
      <td>2016</td>
      <td>1</td>
      <td>1006</td>
      <td>-89.124354</td>
      <td>42.284104</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>16-014351</td>
      <td>90C - Disorderly Conduct</td>
      <td>2016</td>
      <td>5</td>
      <td>1906</td>
      <td>-89.070417</td>
      <td>42.254735</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>16-016247</td>
      <td>13B - Simple Assault</td>
      <td>2016</td>
      <td>2</td>
      <td>1550</td>
      <td>-89.042083</td>
      <td>42.238147</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>16-016181</td>
      <td>290 - Destruction/Damage/Vandalism of Property</td>
      <td>2016</td>
      <td>3</td>
      <td>1138</td>
      <td>-89.029085</td>
      <td>42.241433</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>16-023111</td>
      <td>23H - All Other Larceny</td>
      <td>2016</td>
      <td>6</td>
      <td>100</td>
      <td>-89.029201</td>
      <td>42.211097</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>14.0</td>
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
    </tr>
    <tr>
      <th>200617</th>
      <td>20-084708</td>
      <td>200 - Arson</td>
      <td>2020</td>
      <td>0</td>
      <td>109</td>
      <td>-89.070647</td>
      <td>42.253152</td>
      <td>PR</td>
      <td>PROPERTY</td>
      <td>A</td>
      <td>RP06</td>
      <td>11.0</td>
    </tr>
    <tr>
      <th>200618</th>
      <td>20-084477</td>
      <td>23F - Theft From Motor Vehicle</td>
      <td>2020</td>
      <td>6</td>
      <td>1420</td>
      <td>-89.053822</td>
      <td>42.260884</td>
      <td>PR</td>
      <td>PROPERTY</td>
      <td>A</td>
      <td>RP09</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>200619</th>
      <td>20-084777</td>
      <td>290 - Destruction/Damage/Vandalism of Property</td>
      <td>2020</td>
      <td>6</td>
      <td>1300</td>
      <td>-89.053850</td>
      <td>42.264499</td>
      <td>PR</td>
      <td>PROPERTY</td>
      <td>A</td>
      <td>RP09</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>200620</th>
      <td>20-084777</td>
      <td>23F - Theft From Motor Vehicle</td>
      <td>2020</td>
      <td>6</td>
      <td>1300</td>
      <td>-89.053850</td>
      <td>42.264499</td>
      <td>PR</td>
      <td>PROPERTY</td>
      <td>A</td>
      <td>RP09</td>
      <td>10.0</td>
    </tr>
    <tr>
      <th>200621</th>
      <td>20-084878</td>
      <td>23C - Shoplifting</td>
      <td>2020</td>
      <td>0</td>
      <td>1330</td>
      <td>-88.973476</td>
      <td>42.266605</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>200622 rows × 12 columns</p>
</div>




```python
Occurred_Each_day = data['Occurred Day of Week'].value_counts().sort_index()
plt = Occurred_Each_day.plot(kind='bar',rot=0)
plt.set_xlabel('Occurred Day of Week')
plt.set_ylabel('counts')
plt
```




    <AxesSubplot:xlabel='Occurred Day of Week', ylabel='counts'>




    
![png](output_4_1.png)
    



```python
data.columns
```




    Index(['CaseNo', 'Description', 'Occurred Date', 'Occurred Day of Week',
           'Occurred Time', 'X', 'Y', 'Crime Against Code', 'Crime Against',
           'Group', 'Beat', 'Ward'],
          dtype='object')




```python
def scatter(x, y, color_scaling, colormap='Blues'):
    pyplot.figure(figsize=(10, 10), dpi=70)
    pyplot.scatter(data[x], data[y], c=(data[color_scaling]), cmap=colormap)
    pyplot.show()
```


```python
interact(scatter, x=['X'],
            y=['Y'],
            color_scaling=['Occurred Day of Week'],
            colormap=pyplot.colormaps())
```


    interactive(children=(Dropdown(description='x', options=('X',), value='X'), Dropdown(description='y', options=…





    <function __main__.scatter(x, y, color_scaling, colormap='Blues')>




```python
def number_of_occurrences(Day):
    filtered_data = data[(data['Occurred Day of Week']==Day)]
    Occurred_Each_year = filtered_data['Occurred Date'].value_counts().sort_index()
    plt2 = Occurred_Each_year.plot(kind='line',rot=0)
    plt2.set_xlabel('Year')
    plt2.set_ylabel('counts')
    plt2
interact(number_of_occurrences, Day = (0,6))
```


    interactive(children=(IntSlider(value=3, description='Day', max=6), Output()), _dom_classes=('widget-interact'…





    <function __main__.number_of_occurrences(Day)>




```python
Occurred_Each_year = data['Occurred Date'].value_counts().sort_index()
plt3 = Occurred_Each_year.plot(kind='line',rot=0)
plt3.set_xlabel('Year')
plt3.set_ylabel('counts')
plt3
```




    <AxesSubplot:xlabel='Year', ylabel='counts'>




    
![png](output_9_1.png)
    


### Citations

1. City of Rockford Crime Offenses 2011-Present. https://data.illinois.gov/dataset/116city_of_rockford_crime_offenses_2011present<br>
2. Summary (SRS) Data with Estimated Crimes. http://s3-us-gov-west-1.amazonaws.com/cg-d4b776d0-d898-4153-90c8-8336f86bdfec/estimated_crimes_1979_2019.csv
