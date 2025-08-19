```python
from IPython.display import display, HTML
display(HTML("<style>.container { width:100% !important; }</style>"))
```


<style>.container { width:100% !important; }</style>


# WTA Data Analysis

####  by Nazgul Altynbekova 




```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import seaborn as sns
from pylab import rcParams

%matplotlib inline
```


```python
import warnings
warnings.simplefilter("ignore", UserWarning)
warnings.simplefilter("ignore", FutureWarning)

```


```python
pd.set_option('display.max_columns', 40)
pd.set_option('display.max_rows', 40)
```


```python
rcParams['figure.figsize'] = 15, 10
rcParams['font.size'] = 20
```

# 1. Wrangling, reshaping, EDA

### Collecting data


```python
wta14 = pd.read_excel("./datasets/2014.xlsx")
wta15 = pd.read_excel("./datasets/2015.xlsx")
wta16 = pd.read_excel("./datasets/2016.xlsx")
wta17 = pd.read_excel("./datasets/2017.xlsx")
wta18 = pd.read_excel("./datasets/2018.xlsx")
wta19 = pd.read_excel("./datasets/2019.xlsx")
wta20 = pd.read_excel("./datasets/2020.xlsx")
wta21 = pd.read_excel("./datasets/2021.xlsx")
wta22 = pd.read_excel("./datasets/2022.xlsx")
wta23 = pd.read_excel("./datasets/2023.xlsx")
```


```python
wta = pd.concat([wta14, wta15, wta16, wta17, wta18, wta19, wta20, wta21, wta22, wta23], 
                ignore_index = True, sort = False) 
wta.head()
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
      <th>WTA</th>
      <th>Location</th>
      <th>Tournament</th>
      <th>Date</th>
      <th>Tier</th>
      <th>Court</th>
      <th>Surface</th>
      <th>Round</th>
      <th>Best of</th>
      <th>Winner</th>
      <th>Loser</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>L3</th>
      <th>Wsets</th>
      <th>Lsets</th>
      <th>Comment</th>
      <th>B365W</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Muguruza G.</td>
      <td>Mchale C.</td>
      <td>63.0</td>
      <td>66.0</td>
      <td>933.0</td>
      <td>882.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.20</td>
      <td>1.61</td>
      <td>2.45</td>
      <td>1.52</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.43</td>
      <td>1.62</td>
      <td>2.25</td>
      <td>1.57</td>
      <td>2.58</td>
      <td>1.67</td>
      <td>2.34</td>
      <td>1.57</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Meusburger Y.</td>
      <td>Barthel M.</td>
      <td>49.0</td>
      <td>34.0</td>
      <td>1138.0</td>
      <td>1545.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.40</td>
      <td>1.55</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.39</td>
      <td>1.64</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.67</td>
      <td>2.38</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Pliskova Ka.</td>
      <td>Ormaechea P.</td>
      <td>71.0</td>
      <td>62.0</td>
      <td>839.0</td>
      <td>954.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.57</td>
      <td>2.25</td>
      <td>1.62</td>
      <td>2.25</td>
      <td>1.57</td>
      <td>2.25</td>
      <td>1.61</td>
      <td>2.46</td>
      <td>1.62</td>
      <td>2.20</td>
      <td>1.67</td>
      <td>2.46</td>
      <td>1.60</td>
      <td>2.30</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Hampton J.</td>
      <td>Paszek T.</td>
      <td>28.0</td>
      <td>179.0</td>
      <td>1781.0</td>
      <td>355.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.20</td>
      <td>4.33</td>
      <td>1.25</td>
      <td>3.75</td>
      <td>1.25</td>
      <td>3.75</td>
      <td>1.24</td>
      <td>4.44</td>
      <td>1.22</td>
      <td>3.75</td>
      <td>1.29</td>
      <td>4.60</td>
      <td>1.26</td>
      <td>3.82</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Larsson J.</td>
      <td>Dominguez Lino L.</td>
      <td>84.0</td>
      <td>69.0</td>
      <td>753.0</td>
      <td>842.0</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.61</td>
      <td>2.20</td>
      <td>1.55</td>
      <td>2.40</td>
      <td>1.67</td>
      <td>2.10</td>
      <td>1.68</td>
      <td>2.31</td>
      <td>1.62</td>
      <td>2.20</td>
      <td>1.68</td>
      <td>2.40</td>
      <td>1.60</td>
      <td>2.28</td>
    </tr>
  </tbody>
</table>
</div>



### Checking the dataset


```python
# to check if files has been read completely
print(wta14.shape)
print(wta15.shape)
print(wta16.shape)
print(wta17.shape)
print(wta18.shape)
print(wta19.shape)
print(wta20.shape)
print(wta21.shape)
print(wta22.shape)
print(wta23.shape)
```

    (2476, 38)
    (2521, 36)
    (2522, 36)
    (2500, 36)
    (2469, 36)
    (2472, 32)
    (1055, 32)
    (2447, 32)
    (2369, 32)
    (2491, 32)



```python
#to check if concatenated dataset haven't lost anything in the process
filescount =  sum(df.count().sum() for df in [wta14, wta15, wta16, wta17, wta18, wta19, wta20, wta21, wta22, wta23])
allcount = wta.count().sum() 

if filescount == allcount:
    print('Combined dataset is complete')
else:
    print('Combined dataset is NOT complete')
```

    Combined dataset is complete



```python
#to arrange dataset in ascending order by Date with new indexes
wta = wta.sort_values(by = ['Date'], ignore_index = True)
wta
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
      <th>WTA</th>
      <th>Location</th>
      <th>Tournament</th>
      <th>Date</th>
      <th>Tier</th>
      <th>Court</th>
      <th>Surface</th>
      <th>Round</th>
      <th>Best of</th>
      <th>Winner</th>
      <th>Loser</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>L3</th>
      <th>Wsets</th>
      <th>Lsets</th>
      <th>Comment</th>
      <th>B365W</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>Shenzhen</td>
      <td>Shenzhen Open</td>
      <td>2013-12-29</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Soler Espinosa S.</td>
      <td>Tsurenko L.</td>
      <td>82.0</td>
      <td>68.0</td>
      <td>773.0</td>
      <td>872.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.40</td>
      <td>1.55</td>
      <td>2.38</td>
      <td>1.53</td>
      <td>2.43</td>
      <td>1.63</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.50</td>
      <td>1.65</td>
      <td>2.35</td>
      <td>1.57</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Svitolina E.</td>
      <td>Lepchenko V.</td>
      <td>45.0</td>
      <td>54.0</td>
      <td>1160.0</td>
      <td>1087.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.83</td>
      <td>1.83</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.06</td>
      <td>1.84</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.10</td>
      <td>1.89</td>
      <td>2.00</td>
      <td>1.77</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Lisicki S.</td>
      <td>Rybarikova M.</td>
      <td>15.0</td>
      <td>37.0</td>
      <td>2920.0</td>
      <td>1450.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.28</td>
      <td>3.50</td>
      <td>1.35</td>
      <td>3.10</td>
      <td>1.36</td>
      <td>3.00</td>
      <td>1.34</td>
      <td>3.49</td>
      <td>1.36</td>
      <td>2.88</td>
      <td>1.40</td>
      <td>3.55</td>
      <td>1.33</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Voegele S.</td>
      <td>Keys M.</td>
      <td>50.0</td>
      <td>38.0</td>
      <td>1130.0</td>
      <td>1380.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>3.25</td>
      <td>1.33</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.83</td>
      <td>1.48</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>3.25</td>
      <td>1.50</td>
      <td>2.64</td>
      <td>1.46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Petkovic A.</td>
      <td>Mattek-Sands B.</td>
      <td>43.0</td>
      <td>48.0</td>
      <td>1217.0</td>
      <td>1144.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.00</td>
      <td>1.45</td>
      <td>2.70</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.46</td>
      <td>2.90</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>3.00</td>
      <td>1.44</td>
      <td>2.71</td>
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
      <th>23317</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-03</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Gauff C.</td>
      <td>Vondrousova M.</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>5955.0</td>
      <td>3839.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.36</td>
      <td>3.46</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.43</td>
      <td>3.46</td>
      <td>1.35</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>23318</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-04</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Jabeur O.</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>7795.0</td>
      <td>3695.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.14</td>
      <td>5.50</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.15</td>
      <td>6.53</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.15</td>
      <td>6.85</td>
      <td>1.13</td>
      <td>5.89</td>
    </tr>
    <tr>
      <th>23319</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-04</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Semifinals</td>
      <td>3</td>
      <td>Pegula J.</td>
      <td>Gauff C.</td>
      <td>5.0</td>
      <td>3.0</td>
      <td>4895.0</td>
      <td>5955.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.73</td>
      <td>2.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.78</td>
      <td>2.16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.92</td>
      <td>2.16</td>
      <td>1.76</td>
      <td>2.06</td>
    </tr>
    <tr>
      <th>23320</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-05</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>The Final</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Sabalenka A.</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>7795.0</td>
      <td>8425.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.33</td>
      <td>3.25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.36</td>
      <td>3.44</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.40</td>
      <td>3.65</td>
      <td>1.35</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>23321</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-06</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>The Final</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Pegula J.</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>7795.0</td>
      <td>4895.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.40</td>
      <td>3.25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.41</td>
      <td>3.40</td>
      <td>1.36</td>
      <td>3.14</td>
    </tr>
  </tbody>
</table>
<p>23322 rows × 38 columns</p>
</div>



### Other sanity checking


```python
#checking if there are whole columns or rows of missing values 
pd.isnull(wta)
wta.dropna(how = 'all')
wta.dropna(axis = 1, how = 'all')

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
      <th>WTA</th>
      <th>Location</th>
      <th>Tournament</th>
      <th>Date</th>
      <th>Tier</th>
      <th>Court</th>
      <th>Surface</th>
      <th>Round</th>
      <th>Best of</th>
      <th>Winner</th>
      <th>Loser</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>L3</th>
      <th>Wsets</th>
      <th>Lsets</th>
      <th>Comment</th>
      <th>B365W</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>Shenzhen</td>
      <td>Shenzhen Open</td>
      <td>2013-12-29</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Soler Espinosa S.</td>
      <td>Tsurenko L.</td>
      <td>82.0</td>
      <td>68.0</td>
      <td>773.0</td>
      <td>872.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.40</td>
      <td>1.55</td>
      <td>2.38</td>
      <td>1.53</td>
      <td>2.43</td>
      <td>1.63</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.50</td>
      <td>1.65</td>
      <td>2.35</td>
      <td>1.57</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Svitolina E.</td>
      <td>Lepchenko V.</td>
      <td>45.0</td>
      <td>54.0</td>
      <td>1160.0</td>
      <td>1087.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.83</td>
      <td>1.83</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.06</td>
      <td>1.84</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.10</td>
      <td>1.89</td>
      <td>2.00</td>
      <td>1.77</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Lisicki S.</td>
      <td>Rybarikova M.</td>
      <td>15.0</td>
      <td>37.0</td>
      <td>2920.0</td>
      <td>1450.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.28</td>
      <td>3.50</td>
      <td>1.35</td>
      <td>3.10</td>
      <td>1.36</td>
      <td>3.00</td>
      <td>1.34</td>
      <td>3.49</td>
      <td>1.36</td>
      <td>2.88</td>
      <td>1.40</td>
      <td>3.55</td>
      <td>1.33</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Voegele S.</td>
      <td>Keys M.</td>
      <td>50.0</td>
      <td>38.0</td>
      <td>1130.0</td>
      <td>1380.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>3.25</td>
      <td>1.33</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.83</td>
      <td>1.48</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>3.25</td>
      <td>1.50</td>
      <td>2.64</td>
      <td>1.46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Petkovic A.</td>
      <td>Mattek-Sands B.</td>
      <td>43.0</td>
      <td>48.0</td>
      <td>1217.0</td>
      <td>1144.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.00</td>
      <td>1.45</td>
      <td>2.70</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.46</td>
      <td>2.90</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>3.00</td>
      <td>1.44</td>
      <td>2.71</td>
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
      <th>23317</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-03</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Gauff C.</td>
      <td>Vondrousova M.</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>5955.0</td>
      <td>3839.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.36</td>
      <td>3.46</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.43</td>
      <td>3.46</td>
      <td>1.35</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>23318</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-04</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Jabeur O.</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>7795.0</td>
      <td>3695.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.14</td>
      <td>5.50</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.15</td>
      <td>6.53</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.15</td>
      <td>6.85</td>
      <td>1.13</td>
      <td>5.89</td>
    </tr>
    <tr>
      <th>23319</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-04</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Semifinals</td>
      <td>3</td>
      <td>Pegula J.</td>
      <td>Gauff C.</td>
      <td>5.0</td>
      <td>3.0</td>
      <td>4895.0</td>
      <td>5955.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.73</td>
      <td>2.00</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.78</td>
      <td>2.16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.92</td>
      <td>2.16</td>
      <td>1.76</td>
      <td>2.06</td>
    </tr>
    <tr>
      <th>23320</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-05</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>The Final</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Sabalenka A.</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>7795.0</td>
      <td>8425.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.33</td>
      <td>3.25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.36</td>
      <td>3.44</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.40</td>
      <td>3.65</td>
      <td>1.35</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>23321</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-06</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>The Final</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Pegula J.</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>7795.0</td>
      <td>4895.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.20</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.40</td>
      <td>3.25</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.41</td>
      <td>3.40</td>
      <td>1.36</td>
      <td>3.14</td>
    </tr>
  </tbody>
</table>
<p>23322 rows × 38 columns</p>
</div>




```python
#plotting to see how distribution of missing values looks like in each column
missing_values = wta.isnull().sum()
mv = sns.barplot(x = missing_values.index, y = missing_values.values)
plt.xticks(rotation = 45, fontsize = 10)
plt.title('Missing Values in Each Column')
mv
```




    <Axes: title={'center': 'Missing Values in Each Column'}>




    
![png](output_16_1.png)
    



```python
#replacing all missing values with 0
wta.fillna(0, inplace = True)
```


```python
#checking how distribution of all features looks like to see if there're potential issues
#pd.plotting.scatter_matrix(wta, figsize = (20, 15), diagonal = 'hist')
#plt.xticks(rotation = 45, fontsize = 10)
#plt.yticks(rotation = 0, fontsize = 10)

```


```python
#checking for duplicates
wta.drop_duplicates()
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
      <th>WTA</th>
      <th>Location</th>
      <th>Tournament</th>
      <th>Date</th>
      <th>Tier</th>
      <th>Court</th>
      <th>Surface</th>
      <th>Round</th>
      <th>Best of</th>
      <th>Winner</th>
      <th>Loser</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>L3</th>
      <th>Wsets</th>
      <th>Lsets</th>
      <th>Comment</th>
      <th>B365W</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3</td>
      <td>Shenzhen</td>
      <td>Shenzhen Open</td>
      <td>2013-12-29</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Soler Espinosa S.</td>
      <td>Tsurenko L.</td>
      <td>82.0</td>
      <td>68.0</td>
      <td>773.0</td>
      <td>872.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.40</td>
      <td>1.55</td>
      <td>2.38</td>
      <td>1.53</td>
      <td>2.43</td>
      <td>1.63</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2.50</td>
      <td>1.65</td>
      <td>2.35</td>
      <td>1.57</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Svitolina E.</td>
      <td>Lepchenko V.</td>
      <td>45.0</td>
      <td>54.0</td>
      <td>1160.0</td>
      <td>1087.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.83</td>
      <td>1.83</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.06</td>
      <td>1.84</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.10</td>
      <td>1.89</td>
      <td>2.00</td>
      <td>1.77</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Lisicki S.</td>
      <td>Rybarikova M.</td>
      <td>15.0</td>
      <td>37.0</td>
      <td>2920.0</td>
      <td>1450.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.28</td>
      <td>3.50</td>
      <td>1.35</td>
      <td>3.10</td>
      <td>1.36</td>
      <td>3.00</td>
      <td>1.34</td>
      <td>3.49</td>
      <td>1.36</td>
      <td>2.88</td>
      <td>1.40</td>
      <td>3.55</td>
      <td>1.33</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Voegele S.</td>
      <td>Keys M.</td>
      <td>50.0</td>
      <td>38.0</td>
      <td>1130.0</td>
      <td>1380.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>3.25</td>
      <td>1.33</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>2.83</td>
      <td>1.48</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>3.25</td>
      <td>1.50</td>
      <td>2.64</td>
      <td>1.46</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Petkovic A.</td>
      <td>Mattek-Sands B.</td>
      <td>43.0</td>
      <td>48.0</td>
      <td>1217.0</td>
      <td>1144.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>7.0</td>
      <td>5.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.00</td>
      <td>1.45</td>
      <td>2.70</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.46</td>
      <td>2.90</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.50</td>
      <td>3.00</td>
      <td>1.44</td>
      <td>2.71</td>
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
      <th>23317</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-03</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Gauff C.</td>
      <td>Vondrousova M.</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>5955.0</td>
      <td>3839.0</td>
      <td>5.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.20</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.36</td>
      <td>3.46</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.43</td>
      <td>3.46</td>
      <td>1.35</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>23318</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-04</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Jabeur O.</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>7795.0</td>
      <td>3695.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.14</td>
      <td>5.50</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.15</td>
      <td>6.53</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.15</td>
      <td>6.85</td>
      <td>1.13</td>
      <td>5.89</td>
    </tr>
    <tr>
      <th>23319</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-04</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Semifinals</td>
      <td>3</td>
      <td>Pegula J.</td>
      <td>Gauff C.</td>
      <td>5.0</td>
      <td>3.0</td>
      <td>4895.0</td>
      <td>5955.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.73</td>
      <td>2.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.78</td>
      <td>2.16</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.92</td>
      <td>2.16</td>
      <td>1.76</td>
      <td>2.06</td>
    </tr>
    <tr>
      <th>23320</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-05</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>The Final</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Sabalenka A.</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>7795.0</td>
      <td>8425.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.33</td>
      <td>3.25</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.36</td>
      <td>3.44</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.40</td>
      <td>3.65</td>
      <td>1.35</td>
      <td>3.20</td>
    </tr>
    <tr>
      <th>23321</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-06</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>The Final</td>
      <td>3</td>
      <td>Swiatek I.</td>
      <td>Pegula J.</td>
      <td>2.0</td>
      <td>5.0</td>
      <td>7795.0</td>
      <td>4895.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.36</td>
      <td>3.20</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.40</td>
      <td>3.25</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.41</td>
      <td>3.40</td>
      <td>1.36</td>
      <td>3.14</td>
    </tr>
  </tbody>
</table>
<p>23322 rows × 38 columns</p>
</div>




```python
#checking if data types are correct for correspondent values
wta.dtypes.head(40)
```




    WTA                    int64
    Location              object
    Tournament            object
    Date          datetime64[ns]
    Tier                  object
    Court                 object
    Surface               object
    Round                 object
    Best of                int64
    Winner                object
    Loser                 object
    WRank                float64
    LRank                float64
    WPts                 float64
    LPts                 float64
    W1                   float64
    L1                   float64
    W2                   float64
    L2                   float64
    W3                   float64
    L3                   float64
    Wsets                float64
    Lsets                float64
    Comment               object
    B365W                float64
    B365L                float64
    EXW                  float64
    EXL                  float64
    LBW                  float64
    LBL                  float64
    PSW                  float64
    PSL                  float64
    SJW                  float64
    SJL                  float64
    MaxW                 float64
    MaxL                 float64
    AvgW                 float64
    AvgL                 float64
    dtype: object




```python
#checking the general info and summary to detect unusual values
wta.describe()
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
      <th>WTA</th>
      <th>Date</th>
      <th>Best of</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>L3</th>
      <th>Wsets</th>
      <th>Lsets</th>
      <th>B365W</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>23322.000000</td>
      <td>23322</td>
      <td>23322.0</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
      <td>23322.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>27.445159</td>
      <td>2018-10-19 10:04:24.882943232</td>
      <td>3.0</td>
      <td>62.481863</td>
      <td>92.071606</td>
      <td>1848.878098</td>
      <td>1290.383758</td>
      <td>5.665509</td>
      <td>3.644284</td>
      <td>5.598534</td>
      <td>3.443658</td>
      <td>2.050896</td>
      <td>1.043607</td>
      <td>1.946746</td>
      <td>0.333848</td>
      <td>1.851617</td>
      <td>3.002778</td>
      <td>0.977570</td>
      <td>1.496448</td>
      <td>0.947866</td>
      <td>1.486847</td>
      <td>1.923412</td>
      <td>3.192806</td>
      <td>0.191541</td>
      <td>0.318251</td>
      <td>1.995458</td>
      <td>3.371329</td>
      <td>1.859484</td>
      <td>2.967204</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>2013-12-29 00:00:00</td>
      <td>3.0</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>14.000000</td>
      <td>2016-04-13 00:00:00</td>
      <td>3.0</td>
      <td>19.000000</td>
      <td>34.000000</td>
      <td>760.250000</td>
      <td>595.000000</td>
      <td>6.000000</td>
      <td>2.000000</td>
      <td>6.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>1.300000</td>
      <td>1.660000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.330000</td>
      <td>1.750000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.360000</td>
      <td>1.800000</td>
      <td>1.310000</td>
      <td>1.710000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>27.000000</td>
      <td>2018-07-25 00:00:00</td>
      <td>3.0</td>
      <td>44.000000</td>
      <td>66.000000</td>
      <td>1215.000000</td>
      <td>912.000000</td>
      <td>6.000000</td>
      <td>4.000000</td>
      <td>6.000000</td>
      <td>3.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
      <td>1.570000</td>
      <td>2.370000</td>
      <td>1.110000</td>
      <td>1.300000</td>
      <td>1.060000</td>
      <td>1.200000</td>
      <td>1.610000</td>
      <td>2.440000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.660000</td>
      <td>2.530000</td>
      <td>1.580000</td>
      <td>2.360000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>41.000000</td>
      <td>2021-07-01 00:00:00</td>
      <td>3.0</td>
      <td>83.000000</td>
      <td>110.000000</td>
      <td>2335.000000</td>
      <td>1480.750000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>5.000000</td>
      <td>6.000000</td>
      <td>2.000000</td>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>2.100000</td>
      <td>3.500000</td>
      <td>1.600000</td>
      <td>2.400000</td>
      <td>1.570000</td>
      <td>2.380000</td>
      <td>2.180000</td>
      <td>3.620000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.250000</td>
      <td>3.760000</td>
      <td>2.110000</td>
      <td>3.420000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>60.000000</td>
      <td>2023-11-06 00:00:00</td>
      <td>3.0</td>
      <td>1208.000000</td>
      <td>1876.000000</td>
      <td>13260.000000</td>
      <td>13260.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>7.000000</td>
      <td>15.000000</td>
      <td>13.000000</td>
      <td>2.000000</td>
      <td>3.000000</td>
      <td>17.000000</td>
      <td>34.000000</td>
      <td>12.000000</td>
      <td>15.000000</td>
      <td>15.000000</td>
      <td>34.000000</td>
      <td>52.000000</td>
      <td>42.200000</td>
      <td>13.000000</td>
      <td>23.000000</td>
      <td>127.000000</td>
      <td>67.000000</td>
      <td>14.370000</td>
      <td>23.260000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>15.839186</td>
      <td>NaN</td>
      <td>0.0</td>
      <td>72.209202</td>
      <td>110.804692</td>
      <td>1720.797951</td>
      <td>1232.178928</td>
      <td>1.380546</td>
      <td>1.941614</td>
      <td>1.482457</td>
      <td>1.987056</td>
      <td>2.940082</td>
      <td>1.772328</td>
      <td>0.282249</td>
      <td>0.471869</td>
      <td>0.952372</td>
      <td>2.274801</td>
      <td>1.113925</td>
      <td>1.873735</td>
      <td>1.133916</td>
      <td>2.024505</td>
      <td>1.089578</td>
      <td>2.566850</td>
      <td>0.659623</td>
      <td>1.186476</td>
      <td>1.363717</td>
      <td>2.921186</td>
      <td>0.908524</td>
      <td>2.040345</td>
    </tr>
  </tbody>
</table>
</div>



The summary of dataset's values didn't show any susoicious outliers or abnormality. Missing values histogram 
plot indicate that columns W3, L3, EXW, EXL, LBW, LBL, SJW, SJL have a significant amount of missing values, which 
is explained by the fact that those columns weren't present in different time periods. However, no completely
missing rows or columns were found. Data types match their correspondent data values and no duplicated data were 
detected. Dataset is ready for analysis.

### EDA


```python
#scatter plot of correlation between Rank and Entry Points

plt.scatter(wta.WRank, wta.WPts, s=20, alpha=0.5, label = 'Winner')
plt.scatter(wta.LRank, wta.LPts, s=20, alpha=0.5, label = 'Loser')

plt.yscale('log')
plt.legend()
plt.xlabel('Entry ranking')
plt.ylabel('Entry points (log)')
plt.title('Correlation between Rank and Entry Points')
```




    Text(0.5, 1.0, 'Correlation between Rank and Entry Points')




    
![png](output_24_1.png)
    


As expected, generally, highest ranking players have highest number of entry points, regardless of the match result. Also worth noting that there are a number of players who lost the match and have a lower rank despite having a considerably hign number of entry points.


```python
set1 = wta[['W1', 'L1']].groupby(['W1', 'L1']).size().reset_index()
set2 = wta[['W2', 'L2']].groupby(['W2', 'L2']).size().reset_index()
set2.columns = ['W1', 'L1', 0]
set3 = wta[['W3', 'L3']].groupby(['W3', 'L3']).size().reset_index()
set3.columns = ['W1', 'L1', 0]

sets_scores = pd.concat([set1, set2, set3]).groupby(['W1', 'L1']).sum().sort_values(by = 0, ascending = False)
sets_scores.iloc[1:11].plot(kind = 'bar', figsize = (10, 6))
plt.title('10 most frequent Combinations of Set Scores')
plt.xlabel('Set Scores Combination')
plt.ylabel('Count')
```




    Text(0, 0.5, 'Count')




    
![png](output_26_1.png)
    


The most frequent combinations of set scores are those where the Winner get 6 points against 3 or 4, which seems pretty reasonable distribution of scores. We also got an impressive detail that there is a 6:0 combo in Top 10 combinations, which means that sets with an absolute win happen quite often. 


```python
wta['Year'] = pd.to_datetime(wta['Date']).dt.year
surface_by_year = wta.groupby(['Year', 'Surface']).size().reset_index(name = 'Count')

sns.lineplot(data = surface_by_year, x = 'Year', y = 'Count', hue = 'Surface', palette = 'tab10')
plt.title('How often different types of Surface are used by Year')
plt.xlabel('Year')
plt.ylabel('Count')
```




    Text(0, 0.5, 'Count')




    
![png](output_28_1.png)
    


Hard surface is the most popular type of surface used on tennis matches, probably explained by the fact that using an indoor spaces are more convinient due to weather dependency. Games on fresh air got their benefit once COVID-19 hit in 2020, which dramatically affected all other court surfaces usage except for the grass court. However, once lockdown got was over, indoor games went back to usual numbers.

Note, that set scores 0:0 weren't plotted since they don't represent the real set scores and only replace missing values for scores.


```python
sns.kdeplot(data = wta, x = 'Date', fill = True)
plt.title('Frequency of Matches throughout the Years')
plt.xlabel('Year')
plt.ylabel('Frequency')
```




    Text(0, 0.5, 'Frequency')




    
![png](output_30_1.png)
    


There is some seasonal trends that are probably explained by the certain times of the year when tennis tournaments are held. We can also notice a small decline of the frequency of mathes throughout the whole period of time. The most obvious drop in numbers of matches happenede in 2020-2021, which can be explained by COVID-19 lockdown period. 


```python
sns.boxplot(x = 'Round', y = 'WRank', data = wta, width=0.5).invert_yaxis()
plt.title('Winners entry Ranking by Round')
plt.xlabel('Round')
plt.ylabel('Entry Ranking')
plt.xticks(rotation = 45)
plt.grid(axis = 'y')
```


    
![png](output_32_0.png)
    


Boxplot shows the ranking of winners by each round. Although 3rd Round have some outliers, the graph clearly demonstrates that each next round have higher rank players left.


```python
sns.histplot(wta['WRank'], bins = 30, kde = True)
plt.title('Frequency of winners Rank')
plt.xlabel('Rank')
plt.ylabel('Frequency')
plt.grid(axis = 'y')
```


    
![png](output_34_0.png)
    


The histogram shows the distribution of winner's rank. We can see that the half of winners' rank lies in the range of 0-80, and almost fades away after around 300. 

# Analysis and plotting


### Top 10 players by total wins


```python
top10 = wta['Winner'].value_counts().head(10)
top10
```




    Winner
    Pliskova Ka.    373
    Halep S.        353
    Svitolina E.    324
    Kvitova P.      320
    Garcia C.       306
    Kerber A.       302
    Muguruza G.     287
    Wozniacki C.    251
    Keys M.         250
    Kasatkina D.    249
    Name: count, dtype: int64




```python
top10.plot(kind = 'bar', color = sns.color_palette("Dark2"))
plt.title('Top 10 Players by the number of Wins')
plt.xlabel('Name')
plt.ylabel('Number of wins')
plt.xticks(rotation = 45)

```




    (array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]),
     [Text(0, 0, 'Pliskova Ka.'),
      Text(1, 0, 'Halep S.'),
      Text(2, 0, 'Svitolina E.'),
      Text(3, 0, 'Kvitova P.'),
      Text(4, 0, 'Garcia C.'),
      Text(5, 0, 'Kerber A.'),
      Text(6, 0, 'Muguruza G.'),
      Text(7, 0, 'Wozniacki C.'),
      Text(8, 0, 'Keys M.'),
      Text(9, 0, 'Kasatkina D.')])




    
![png](output_39_1.png)
    


The number of winning games between top 10 players decreases considerably smoothly, which tells us that the competition is pretty tense and it's hard to point on an absolute champion. However, players on 8th to 10th have pretty tight number of wins, which might signify the potential change of their position in top 10 in foreseeable futre.

### Top 10 players by the largest number of First Round tournament losses across all 10 years


```python
top10L = wta[wta['Round'] == '1st Round']['Loser'].value_counts().head(10)
#sort_values(by = 0, ascending = False).
top10L
```




    Loser
    Zhang S.          105
    Siniakova K.       98
    Mladenovic K.      89
    Watson H.          85
    Babos T.           84
    Doi M.             82
    Schmiedlova A.     79
    Begu I.            78
    Maria T.           78
    Riske A.           78
    Name: count, dtype: int64




```python
top10L.plot(kind = 'bar', color = sns.color_palette("Dark2"))
plt.title('Top 10 players by number of First Round tournament losses')
plt.xlabel('Name')
plt.ylabel('Number of First Round tournament losses')
plt.xticks(rotation = 45, ha = 'right')

```




    (array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9]),
     [Text(0, 0, 'Zhang S.'),
      Text(1, 0, 'Siniakova K.'),
      Text(2, 0, 'Mladenovic K.'),
      Text(3, 0, 'Watson H.'),
      Text(4, 0, 'Babos T.'),
      Text(5, 0, 'Doi M.'),
      Text(6, 0, 'Schmiedlova A.'),
      Text(7, 0, 'Begu I.'),
      Text(8, 0, 'Maria T.'),
      Text(9, 0, 'Riske A.')])




    
![png](output_43_1.png)
    


According to the largest number of First Round tournament losses across all 10 years, player Zhang S. has the most losses (105 throughout 10 years of tournaments). The following players in the list have gradually decreasing number of losses, until it evens out for the last 4 players in the list, which might indicate that having around 78 lost games in the First Round is a comparatively average index.

### 5 biggest upsets for each year in the dataset based on ranking differentials


```python
wta['Rank_diff'] = wta['WRank'] - wta['LRank']
wta['Set_score'] = wta['Wsets'].astype(int).astype(str) + '-' + wta['Lsets'].astype(int).astype(str)
wta_year = wta.groupby('Year')

top5_upsets = pd.DataFrame(columns=['Year', 'Winner', 'WRank', 'Loser', 'LRank', 'Set_score', 'Tournament', 'Rank_diff'])

for year, group in wta_year:
    sort_match = group.sort_values(by = 'Rank_diff', ascending = False).head(5)
    top5_upsets = pd.concat([top5_upsets, 
                            sort_match[['Year', 'Winner', 'WRank', 'Loser', 'LRank', 'Set_score', 'Tournament', 
                                        'Rank_diff']]], 
                            ignore_index=True)

top5_upsets
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
      <th>Year</th>
      <th>Winner</th>
      <th>WRank</th>
      <th>Loser</th>
      <th>LRank</th>
      <th>Set_score</th>
      <th>Tournament</th>
      <th>Rank_diff</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2013</td>
      <td>Konjuh A.</td>
      <td>259.0</td>
      <td>Vinci R.</td>
      <td>14.0</td>
      <td>2-1</td>
      <td>ASB Classic</td>
      <td>245.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2013</td>
      <td>Chan Y.J.</td>
      <td>257.0</td>
      <td>Zhang S.</td>
      <td>52.0</td>
      <td>2-1</td>
      <td>Shenzhen Open</td>
      <td>205.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2013</td>
      <td>Barty A.</td>
      <td>190.0</td>
      <td>Hantuchova D.</td>
      <td>33.0</td>
      <td>2-0</td>
      <td>Brisbane International</td>
      <td>157.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2013</td>
      <td>Fichman S.</td>
      <td>116.0</td>
      <td>Cirstea S.</td>
      <td>22.0</td>
      <td>2-0</td>
      <td>ASB Classic</td>
      <td>94.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013</td>
      <td>Friedsam A.L.</td>
      <td>122.0</td>
      <td>Jovanovski B.</td>
      <td>36.0</td>
      <td>2-0</td>
      <td>Shenzhen Open</td>
      <td>86.0</td>
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
    </tr>
    <tr>
      <th>50</th>
      <td>2023</td>
      <td>Brady J.</td>
      <td>1055.0</td>
      <td>Kalinina A.</td>
      <td>28.0</td>
      <td>2-0</td>
      <td>Citi Open</td>
      <td>1027.0</td>
    </tr>
    <tr>
      <th>51</th>
      <td>2023</td>
      <td>Strycova B.</td>
      <td>1103.0</td>
      <td>Zanevska M.</td>
      <td>77.0</td>
      <td>2-1</td>
      <td>Internazionali BNL d'Italia</td>
      <td>1026.0</td>
    </tr>
    <tr>
      <th>52</th>
      <td>2023</td>
      <td>Williams V.</td>
      <td>1003.0</td>
      <td>Volynets K.</td>
      <td>113.0</td>
      <td>2-0</td>
      <td>ASB Classic</td>
      <td>890.0</td>
    </tr>
    <tr>
      <th>53</th>
      <td>2023</td>
      <td>Routliffe E.</td>
      <td>858.0</td>
      <td>Hsieh S.W.</td>
      <td>23.0</td>
      <td>2-1</td>
      <td>Internationaux de Strasbourg</td>
      <td>835.0</td>
    </tr>
    <tr>
      <th>54</th>
      <td>2023</td>
      <td>Jones F.</td>
      <td>817.0</td>
      <td>Parrizas Diaz N.</td>
      <td>80.0</td>
      <td>2-0</td>
      <td>Copa Colsanitas</td>
      <td>737.0</td>
    </tr>
  </tbody>
</table>
<p>55 rows × 8 columns</p>
</div>



### Top 10 players at year-end in 2018 and how have their rankings changed over the period of 2014 to 2023


```python
wta2018 = wta[wta['Year'] == 2018]
top10_2018 = wta2018.groupby('Winner')['WRank'].mean().sort_values().head(10)
top10_2018_full = wta[wta['Winner'].isin(top10_2018.index)]
top10_2018_full
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
      <th>WTA</th>
      <th>Location</th>
      <th>Tournament</th>
      <th>Date</th>
      <th>Tier</th>
      <th>Court</th>
      <th>Surface</th>
      <th>Round</th>
      <th>Best of</th>
      <th>Winner</th>
      <th>Loser</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>...</th>
      <th>Wsets</th>
      <th>Lsets</th>
      <th>Comment</th>
      <th>B365W</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
      <th>Year</th>
      <th>Rank_diff</th>
      <th>Set_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2013-12-29</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Svitolina E.</td>
      <td>Lepchenko V.</td>
      <td>45.0</td>
      <td>54.0</td>
      <td>1160.0</td>
      <td>1087.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.83</td>
      <td>1.83</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.06</td>
      <td>1.84</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.10</td>
      <td>1.89</td>
      <td>2.00</td>
      <td>1.77</td>
      <td>2013</td>
      <td>-9.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Muguruza G.</td>
      <td>Mchale C.</td>
      <td>63.0</td>
      <td>66.0</td>
      <td>933.0</td>
      <td>882.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.20</td>
      <td>1.61</td>
      <td>2.45</td>
      <td>1.52</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.43</td>
      <td>1.62</td>
      <td>2.25</td>
      <td>1.57</td>
      <td>2.58</td>
      <td>1.67</td>
      <td>2.34</td>
      <td>1.57</td>
      <td>2013</td>
      <td>-3.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Williams V.</td>
      <td>Hlavackova A.</td>
      <td>47.0</td>
      <td>134.0</td>
      <td>1147.0</td>
      <td>470.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.16</td>
      <td>4.50</td>
      <td>1.22</td>
      <td>4.00</td>
      <td>1.25</td>
      <td>3.75</td>
      <td>1.20</td>
      <td>5.07</td>
      <td>1.22</td>
      <td>3.75</td>
      <td>1.25</td>
      <td>5.10</td>
      <td>1.20</td>
      <td>4.41</td>
      <td>2013</td>
      <td>-87.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2013-12-30</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Pliskova Ka.</td>
      <td>Ormaechea P.</td>
      <td>71.0</td>
      <td>62.0</td>
      <td>839.0</td>
      <td>954.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.57</td>
      <td>2.25</td>
      <td>1.62</td>
      <td>2.25</td>
      <td>1.57</td>
      <td>2.25</td>
      <td>1.61</td>
      <td>2.46</td>
      <td>1.62</td>
      <td>2.20</td>
      <td>1.67</td>
      <td>2.46</td>
      <td>1.60</td>
      <td>2.30</td>
      <td>2013</td>
      <td>9.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>62</th>
      <td>1</td>
      <td>Auckland</td>
      <td>ASB Classic</td>
      <td>2014-01-01</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>2nd Round</td>
      <td>3</td>
      <td>Muguruza G.</td>
      <td>Fichman S.</td>
      <td>63.0</td>
      <td>116.0</td>
      <td>933.0</td>
      <td>576.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.57</td>
      <td>2.25</td>
      <td>1.62</td>
      <td>2.25</td>
      <td>1.53</td>
      <td>2.38</td>
      <td>1.61</td>
      <td>2.49</td>
      <td>1.57</td>
      <td>2.38</td>
      <td>1.70</td>
      <td>2.55</td>
      <td>1.59</td>
      <td>2.31</td>
      <td>2014</td>
      <td>-53.0</td>
      <td>2-1</td>
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
      <th>23090</th>
      <td>49</td>
      <td>Beijing</td>
      <td>China Open</td>
      <td>2023-10-03</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>2nd Round</td>
      <td>3</td>
      <td>Garcia C.</td>
      <td>Putintseva Y.</td>
      <td>10.0</td>
      <td>75.0</td>
      <td>3335.0</td>
      <td>877.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.62</td>
      <td>2.30</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.61</td>
      <td>2.45</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.64</td>
      <td>2.46</td>
      <td>1.58</td>
      <td>2.37</td>
      <td>2023</td>
      <td>-65.0</td>
      <td>2-1</td>
    </tr>
    <tr>
      <th>23095</th>
      <td>49</td>
      <td>Beijing</td>
      <td>China Open</td>
      <td>2023-10-04</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>3rd Round</td>
      <td>3</td>
      <td>Ostapenko J.</td>
      <td>Pegula J.</td>
      <td>17.0</td>
      <td>4.0</td>
      <td>2505.0</td>
      <td>5955.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.50</td>
      <td>1.53</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2.69</td>
      <td>1.53</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2.75</td>
      <td>1.59</td>
      <td>2.60</td>
      <td>1.49</td>
      <td>2023</td>
      <td>13.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>23102</th>
      <td>49</td>
      <td>Beijing</td>
      <td>China Open</td>
      <td>2023-10-05</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>3rd Round</td>
      <td>3</td>
      <td>Garcia C.</td>
      <td>Kalinina A.</td>
      <td>10.0</td>
      <td>28.0</td>
      <td>3335.0</td>
      <td>1572.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>1.73</td>
      <td>2.10</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.67</td>
      <td>2.35</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.74</td>
      <td>2.44</td>
      <td>1.66</td>
      <td>2.21</td>
      <td>2023</td>
      <td>-18.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>23295</th>
      <td>56</td>
      <td>Zhuhai</td>
      <td>WTA Elite Trophy</td>
      <td>2023-10-25</td>
      <td>Tour Championships</td>
      <td>Indoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Garcia C.</td>
      <td>Keys M.</td>
      <td>20.0</td>
      <td>11.0</td>
      <td>2035.0</td>
      <td>2737.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>0.0</td>
      <td>Completed</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.97</td>
      <td>1.89</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2.00</td>
      <td>1.99</td>
      <td>1.92</td>
      <td>1.87</td>
      <td>2023</td>
      <td>9.0</td>
      <td>2-0</td>
    </tr>
    <tr>
      <th>23299</th>
      <td>56</td>
      <td>Zhuhai</td>
      <td>WTA Elite Trophy</td>
      <td>2023-10-26</td>
      <td>Tour Championships</td>
      <td>Indoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Ostapenko J.</td>
      <td>Vekic D.</td>
      <td>13.0</td>
      <td>24.0</td>
      <td>2615.0</td>
      <td>1815.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>Completed</td>
      <td>1.53</td>
      <td>2.50</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.55</td>
      <td>2.56</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.63</td>
      <td>2.60</td>
      <td>1.53</td>
      <td>2.46</td>
      <td>2023</td>
      <td>-11.0</td>
      <td>2-1</td>
    </tr>
  </tbody>
</table>
<p>2505 rows × 41 columns</p>
</div>




```python
plot_data = top10_2018_full[top10_2018_full['WRank'] > 0].copy()

sns.lineplot(
    x = 'Year', 
    y = np.log(plot_data['WRank']), 
    hue = 'Winner', 
    data = plot_data, 
    palette = 'Dark2'
).invert_yaxis()

plt.title('Ranking progress of Top 10 players in 2018 (2014 - 2023)')
plt.xlabel('Year')
plt.ylabel('Ranking')
plt.legend(title = 'Players:',  bbox_to_anchor=(1, 1))

```




    <matplotlib.legend.Legend at 0x13ff1d710>




    
![png](output_49_1.png)
    


Line graph demonstrates that all the players had the closest points to the highest rank in 2018 - year, according to which we assembled the list. It is reasonable to say that the overall pattern of rank prgress is comapratively similar among those 10 players: in the beginning of 2014 their rank was a bit lower, was going up by 2018 and going down again in the rest of the time period. However, some players had significant rank drops that probably was determined by inactiveyears: Stephens S. in 2017, Williams V.	and Wozniacki C. after 2021, Svitolina E. after 2022.

Note: Ranking was log transformed, so that significantly low ranks of some players wouldn't deform the rest of the graph and the lines could be clearly interpreted. The y-axs was also inverted for better readability of ranks, being 0 as the highest rank.


```python
#from bokeh.plotting import figure, output_notebook, show
#output_notebook()
#p2 = figure(title = '')
#p2.scatter(top10_2018_full['Year'], np.log(wta['WRank']))
           #, color = 'Dark2') 
        #line = 'Winner'
#show(p2)
```

# Advanced analysis


### Tournaments have had on average the most upsets


```python
wta['Upset'] = wta['WRank'] > wta['LRank']

top10_tours = wta.groupby('Tournament')['Upset'].mean().sort_values(ascending = False).head(10)
top10_tours
```




    Tournament
    Sony Ericsson Championships    0.533333
    Ladies Open Biel Bienne        0.516129
    MUSC Health Women's Open       0.516129
    Moscow River Cup               0.516129
    BNP Paribas WTA Finals         0.511111
    Copa Colsanitas                0.494624
    Baltic Open                    0.483871
    Tashkent Open                  0.483871
    Budapest Open                  0.483871
    Viking International           0.483871
    Name: Upset, dtype: float64




```python
top10_tours.plot(kind = 'barh', color = sns.color_palette("rocket")).invert_yaxis()
plt.title('Top 10 Tournaments with the most Upsets on Average')
plt.ylabel('Tournament')
plt.xlabel('Upset Rate')
plt.xticks(rotation = 45)

```




    (array([0. , 0.1, 0.2, 0.3, 0.4, 0.5, 0.6]),
     [Text(0.0, 0, '0.0'),
      Text(0.1, 0, '0.1'),
      Text(0.2, 0, '0.2'),
      Text(0.30000000000000004, 0, '0.3'),
      Text(0.4, 0, '0.4'),
      Text(0.5, 0, '0.5'),
      Text(0.6000000000000001, 0, '0.6')])




    
![png](output_55_1.png)
    


### Top 10 ranked players (by ranking) at the end of 2023.


```python
top10_2023 = wta[wta['Year'] == 2023].groupby('Winner')['WRank'].last().nsmallest(10)
top10_2023_match = wta[(wta['Year'] == 2023) 
                       & (wta['Winner'].isin(top10_2023.index)
                          & (wta['Loser'].isin(top10_2023.index)))]
win_loss = pd.DataFrame(index = top10_2023.index, columns = top10_2023.index, data = 0)

for index, match in top10_2023_match.iterrows():
    winner = match['Winner']
    loser = match['Loser']
    win_loss.at[winner, loser] += 1
    win_loss.at[loser, winner] -= 1

win_loss
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
      <th>Winner</th>
      <th>Sabalenka A.</th>
      <th>Swiatek I.</th>
      <th>Gauff C.</th>
      <th>Rybakina E.</th>
      <th>Pegula J.</th>
      <th>Sakkari M.</th>
      <th>Jabeur O.</th>
      <th>Vondrousova M.</th>
      <th>Krejcikova B.</th>
      <th>Muchova K.</th>
    </tr>
    <tr>
      <th>Winner</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Sabalenka A.</th>
      <td>0</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>-1</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>-2</td>
    </tr>
    <tr>
      <th>Swiatek I.</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>-3</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>-1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Gauff C.</th>
      <td>0</td>
      <td>-3</td>
      <td>0</td>
      <td>1</td>
      <td>-1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Rybakina E.</th>
      <td>0</td>
      <td>3</td>
      <td>-1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Pegula J.</th>
      <td>1</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>-1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>Sakkari M.</th>
      <td>-3</td>
      <td>0</td>
      <td>-2</td>
      <td>-1</td>
      <td>-2</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-2</td>
    </tr>
    <tr>
      <th>Jabeur O.</th>
      <td>0</td>
      <td>-2</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>-2</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Vondrousova M.</th>
      <td>-1</td>
      <td>-2</td>
      <td>-2</td>
      <td>-1</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>-1</td>
    </tr>
    <tr>
      <th>Krejcikova B.</th>
      <td>-2</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>Muchova K.</th>
      <td>2</td>
      <td>-2</td>
      <td>-2</td>
      <td>-1</td>
      <td>-1</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



Some players have more positive results than others, which indicates that they have more wins than loses with top 10 players of the year. Others show the opposite: e.g. player Sakkari M. has either negative results or 0, which says that she couldn't overplay any of the top 10 players that year.

Also there is a lot of zero or close to zero results, which can be interpreted as nearly the same level of excellence between the best 10 players of 2023. 

### Top 5 players with the longest winning streaks between 2014 – 2023


```python
streaks = {}

for player in wta['Winner'].unique():
    matches = wta[wta['Winner'] == player]
    matches = matches.sort_values(by = 'Date')
    
    streakstart = 0
    streakfull = 0
    start = None
    end = None
    
    for index, match in matches.iterrows():
        year = match['Year']
        if end is None or year == end + 1:
            streakstart += 1
        else:
            streakstart = 1  
            start = year
        
        if streakstart > streakfull:
            streakfull = streakstart
            end = year  
        
        end = year
    
    streaks[player] = (streakfull, start, end)

players = sorted(streaks.items(), key = lambda x: x[1][0], reverse = True)[:5]

for player, (streak, start, end) in players:
    print(f"Player: {player}, Streak: {streak}, Started: {start}, Ended: {end}")

```

    Player: Stosur S., Streak: 4, Started: 2019, Ended: 2022
    Player: Tomova V., Streak: 4, Started: 2023, Ended: 2023
    Player: Peng S., Streak: 3, Started: 2020, Ended: 2020
    Player: King V., Streak: 3, Started: 2016, Ended: 2018
    Player: Williams V., Streak: 3, Started: 2023, Ended: 2023


### Top 10 players by the percentage of tiebreaks won


```python
wta['S1'] = wta['W1'].astype(int).astype(str) + '-' + wta['L1'].astype(int).astype(str)
wta['S2'] = wta['W2'].astype(int).astype(str) + '-' + wta['L2'].astype(int).astype(str)
wta['S3'] = wta['W3'].astype(int).astype(str) + '-' + wta['L3'].astype(int).astype(str)

tb_match1 = wta[wta['S1'].str.contains('6-7', '7-6')]
tb_match2 = wta[wta['S2'].str.contains('6-7', '7-6')]
tb_match3 = wta[wta['S3'].str.contains('6-7', '7-6')]

tb_match = pd.concat([tb_match1, tb_match2, tb_match3])
tb_match
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
      <th>WTA</th>
      <th>Location</th>
      <th>Tournament</th>
      <th>Date</th>
      <th>Tier</th>
      <th>Court</th>
      <th>Surface</th>
      <th>Round</th>
      <th>Best of</th>
      <th>Winner</th>
      <th>Loser</th>
      <th>WRank</th>
      <th>LRank</th>
      <th>WPts</th>
      <th>LPts</th>
      <th>W1</th>
      <th>L1</th>
      <th>W2</th>
      <th>L2</th>
      <th>W3</th>
      <th>...</th>
      <th>B365L</th>
      <th>EXW</th>
      <th>EXL</th>
      <th>LBW</th>
      <th>LBL</th>
      <th>PSW</th>
      <th>PSL</th>
      <th>SJW</th>
      <th>SJL</th>
      <th>MaxW</th>
      <th>MaxL</th>
      <th>AvgW</th>
      <th>AvgL</th>
      <th>Year</th>
      <th>Rank_diff</th>
      <th>Set_score</th>
      <th>Upset</th>
      <th>S1</th>
      <th>S2</th>
      <th>S3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>78</th>
      <td>2</td>
      <td>Brisbane</td>
      <td>Brisbane International</td>
      <td>2014-01-02</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Quarterfinals</td>
      <td>3</td>
      <td>Jankovic J.</td>
      <td>Kerber A.</td>
      <td>8.0</td>
      <td>9.0</td>
      <td>4170.0</td>
      <td>3965.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.61</td>
      <td>2.20</td>
      <td>1.65</td>
      <td>2.10</td>
      <td>1.67</td>
      <td>2.56</td>
      <td>1.59</td>
      <td>2.20</td>
      <td>1.67</td>
      <td>2.66</td>
      <td>1.70</td>
      <td>2.26</td>
      <td>1.61</td>
      <td>2014</td>
      <td>-1.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-7</td>
      <td>6-3</td>
      <td>6-1</td>
    </tr>
    <tr>
      <th>95</th>
      <td>5</td>
      <td>Sydney</td>
      <td>Apai International</td>
      <td>2014-01-06</td>
      <td>Premier</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Mchale C.</td>
      <td>Cornet A.</td>
      <td>65.0</td>
      <td>26.0</td>
      <td>882.0</td>
      <td>1840.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>7.0</td>
      <td>...</td>
      <td>1.72</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>2.20</td>
      <td>1.62</td>
      <td>2.16</td>
      <td>1.76</td>
      <td>2.00</td>
      <td>1.80</td>
      <td>2.20</td>
      <td>1.87</td>
      <td>2.02</td>
      <td>1.76</td>
      <td>2014</td>
      <td>39.0</td>
      <td>2-1</td>
      <td>True</td>
      <td>6-7</td>
      <td>6-2</td>
      <td>7-5</td>
    </tr>
    <tr>
      <th>106</th>
      <td>4</td>
      <td>Hobart</td>
      <td>Hobart International</td>
      <td>2014-01-06</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Stosur S.</td>
      <td>Brengle M.</td>
      <td>17.0</td>
      <td>147.0</td>
      <td>2675.0</td>
      <td>424.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>7.0</td>
      <td>...</td>
      <td>6.00</td>
      <td>1.15</td>
      <td>5.25</td>
      <td>1.12</td>
      <td>5.50</td>
      <td>1.15</td>
      <td>6.40</td>
      <td>1.13</td>
      <td>6.00</td>
      <td>1.15</td>
      <td>7.00</td>
      <td>1.13</td>
      <td>5.74</td>
      <td>2014</td>
      <td>-130.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-7</td>
      <td>6-1</td>
      <td>7-6</td>
    </tr>
    <tr>
      <th>111</th>
      <td>4</td>
      <td>Hobart</td>
      <td>Hobart International</td>
      <td>2014-01-06</td>
      <td>International</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Mladenovic K.</td>
      <td>Soler Espinosa S.</td>
      <td>54.0</td>
      <td>79.0</td>
      <td>1075.0</td>
      <td>787.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>2.20</td>
      <td>1.65</td>
      <td>2.20</td>
      <td>1.67</td>
      <td>2.10</td>
      <td>1.77</td>
      <td>2.16</td>
      <td>1.73</td>
      <td>2.10</td>
      <td>1.77</td>
      <td>2.25</td>
      <td>1.68</td>
      <td>2.14</td>
      <td>2014</td>
      <td>-25.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-7</td>
      <td>6-4</td>
      <td>6-4</td>
    </tr>
    <tr>
      <th>193</th>
      <td>6</td>
      <td>Melbourne</td>
      <td>Australian Open</td>
      <td>2014-01-14</td>
      <td>Grand Slam</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Jovanovski B.</td>
      <td>Cepelova J.</td>
      <td>34.0</td>
      <td>69.0</td>
      <td>1475.0</td>
      <td>849.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>1.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>2.62</td>
      <td>1.50</td>
      <td>2.50</td>
      <td>1.53</td>
      <td>2.38</td>
      <td>1.45</td>
      <td>2.97</td>
      <td>1.53</td>
      <td>2.50</td>
      <td>1.56</td>
      <td>2.97</td>
      <td>1.49</td>
      <td>2.58</td>
      <td>2014</td>
      <td>-35.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-7</td>
      <td>6-1</td>
      <td>6-3</td>
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
      <th>23172</th>
      <td>50</td>
      <td>Hong Kong</td>
      <td>Hong Kong Tennis Open</td>
      <td>2023-10-12</td>
      <td>WTA250</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>2nd Round</td>
      <td>3</td>
      <td>Trevisan M.</td>
      <td>Frech M.</td>
      <td>42.0</td>
      <td>70.0</td>
      <td>1227.0</td>
      <td>924.0</td>
      <td>6.0</td>
      <td>3.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.91</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.95</td>
      <td>1.93</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2.00</td>
      <td>1.95</td>
      <td>1.90</td>
      <td>1.88</td>
      <td>2023</td>
      <td>-28.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-3</td>
      <td>6-7</td>
      <td>6-3</td>
    </tr>
    <tr>
      <th>23205</th>
      <td>54</td>
      <td>Monastir</td>
      <td>Jasmin Open</td>
      <td>2023-10-16</td>
      <td>WTA250</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Errani S.</td>
      <td>Zidansek T.</td>
      <td>108.0</td>
      <td>96.0</td>
      <td>652.0</td>
      <td>725.0</td>
      <td>6.0</td>
      <td>2.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>1.44</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>2.95</td>
      <td>1.44</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>3.00</td>
      <td>1.44</td>
      <td>2.83</td>
      <td>1.41</td>
      <td>2023</td>
      <td>12.0</td>
      <td>2-1</td>
      <td>True</td>
      <td>6-2</td>
      <td>6-7</td>
      <td>6-3</td>
    </tr>
    <tr>
      <th>23210</th>
      <td>53</td>
      <td>Cluj-Napoca</td>
      <td>Transylvania Open</td>
      <td>2023-10-16</td>
      <td>WTA250</td>
      <td>Indoor</td>
      <td>Hard</td>
      <td>1st Round</td>
      <td>3</td>
      <td>Tig P.M.</td>
      <td>Matoula M.</td>
      <td>462.0</td>
      <td>327.0</td>
      <td>116.0</td>
      <td>190.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>2.63</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.49</td>
      <td>2.76</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.50</td>
      <td>3.00</td>
      <td>1.47</td>
      <td>2.65</td>
      <td>2023</td>
      <td>135.0</td>
      <td>2-1</td>
      <td>True</td>
      <td>6-4</td>
      <td>6-7</td>
      <td>6-3</td>
    </tr>
    <tr>
      <th>23297</th>
      <td>56</td>
      <td>Zhuhai</td>
      <td>WTA Elite Trophy</td>
      <td>2023-10-25</td>
      <td>Tour Championships</td>
      <td>Indoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Zheng Q.</td>
      <td>Vekic D.</td>
      <td>18.0</td>
      <td>24.0</td>
      <td>2275.0</td>
      <td>1815.0</td>
      <td>6.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>3.75</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.33</td>
      <td>3.58</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.36</td>
      <td>3.75</td>
      <td>1.29</td>
      <td>3.53</td>
      <td>2023</td>
      <td>-6.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-4</td>
      <td>6-7</td>
      <td>6-4</td>
    </tr>
    <tr>
      <th>23312</th>
      <td>57</td>
      <td>Cancun</td>
      <td>WTA Finals</td>
      <td>2023-11-01</td>
      <td>Tour Championships</td>
      <td>Outdoor</td>
      <td>Hard</td>
      <td>Round Robin</td>
      <td>3</td>
      <td>Rybakina E.</td>
      <td>Sakkari M.</td>
      <td>4.0</td>
      <td>9.0</td>
      <td>5865.0</td>
      <td>3245.0</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>7.0</td>
      <td>...</td>
      <td>3.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.44</td>
      <td>3.05</td>
      <td>0.00</td>
      <td>0.00</td>
      <td>1.47</td>
      <td>3.32</td>
      <td>1.41</td>
      <td>2.90</td>
      <td>2023</td>
      <td>-5.0</td>
      <td>2-1</td>
      <td>False</td>
      <td>6-0</td>
      <td>6-7</td>
      <td>7-6</td>
    </tr>
  </tbody>
</table>
<p>1130 rows × 45 columns</p>
</div>




```python
tb_all = tb_match['Winner'].value_counts() + tb_match['Loser'].value_counts()
tb_win = tb_match['Winner'].value_counts()
tb_perc = (tb_win / tb_all) * 100
top10_tb = tb_perc.nlargest(10)
top10_tb
```




    Peterson R.        88.888889
    Arruabarrena L.    87.500000
    Diyas Z.           87.500000
    Osaka N.           83.333333
    Pliskova Ka.       81.081081
    Bacsinszky T.      80.000000
    Garcia C.          80.000000
    Jovanovski B.      80.000000
    Krunic A.          80.000000
    Wickmayer Y.       80.000000
    Name: count, dtype: float64



### The most common set score combinations in matches with upsets


```python
upsets = wta[wta['Upset']]
ss_count = pd.DataFrame()

for ss_col in ['S1', 'S2', 'S3']:
    ss_count[ss_col] = upsets[ss_col].value_counts()

ss_count['Total'] = ss_count.sum(axis = 1)
ss_count = ss_count.sort_values(by = 'Total', ascending = False).head(11)
ss_count = ss_count.drop('0-0', errors = 'ignore')

for ss_col in ss_count.columns[:-1]:
    plt.bar(ss_count.index, ss_count[ss_col], label = ss_col)

plt.xlabel('Set score')
plt.title('Most common Set Scores in matches with Upsets')
plt.legend(title='Sets')
```




    <matplotlib.legend.Legend at 0x14b1259d0>




    
![png](output_65_1.png)
    


Despite expecting that upset matches won't show much dramatic set results, we got 3 out 10 the most popular set scores combinations, where the player with lower rank won set with big difference (6-2, 6-1, 6-0). Other set score combinations seem pretty common not only for upset matches (6-4, 6-3, 7-6, 7-5). And we also have set outcomeswhere the winner of the match lost (4-6, 3-6, 2-6), which  didn't stop them to turn the tables.

### Locations that have the most tournaments a year


```python
loc_count = wta['Location'].value_counts()
big_locs = loc_count[loc_count >= 50]
big_match = wta[wta['Location'].isin(big_locs.index)]
big_match_piv = big_match.pivot_table(index = 'Location', columns = wta['Year'], aggfunc = 'size')

big_match_piv.sum(axis = 1).sort_values(ascending = False).head(10).plot(kind = 'bar', color = sns.color_palette("Dark2"))
plt.title('Number of matches by Location')
plt.xlabel('Location')
plt.ylabel('Number of matches')
plt.xticks(rotation = 45)
plt.grid(True)
```


    
![png](output_68_0.png)
    


Melbourne, New York, Paris, London are the most popular location for tennis mathes having over 1000 mathes a year. Other popular locations are: Indian Wells, Miami, Rome, Charleston and Cincinnati.

### Top-10 players' performance on different types of surfaces


```python
top10_rank = wta.groupby('Winner')['WRank'].mean().nsmallest(10).index
top10_wins = wta[wta['Winner'].isin(top10_rank)]
top10_losses = wta[wta['Loser'].isin(top10_rank)]

wins_surf = top10_wins.groupby(['Winner', 'Surface']).size().unstack()
loss_surf = top10_losses.groupby(['Loser', 'Surface']).size().unstack()
total_surf = wins_surf + loss_surf

winrate_surf = (wins_surf / total_surf * 100).round(2)

for player in top10_rank:
    player_winrate_surf = winrate_surf.loc[player]
    plt.plot(player_winrate_surf.index, player_winrate_surf.values, label = player)

plt.title('Winning rate of Top 10 players on different Surfaces')
plt.xlabel('Surface')
plt.ylabel('Winning rate')
plt.legend(bbox_to_anchor = (1.05, 1), loc = 'upper left')
plt.grid(True)
```


    
![png](output_71_0.png)
    


I got an assumption that players might have "bias" towards certain type of court surface. On the grapph we can see that there's comparatively even distribution of players who:
    1) perform almost the same on all surfaces (Pliskova Ka., Ivanovic A., Halep S.);
    2) perform better on clay and hard surfaces (Muguruza G., Sabalenka A., Swiatek I.);
    3) perform better on grass (Radwanska A., Kvitova P., Kerber A.)
       And perfomance of Li N. drastically improves on hard surfaces, but considerably the same on clay and grass.
Not only this assumption can be considered as proven right, but also a certain correlation between surface types might be discovered. It is more common for players who perform better on clay/hard surfaces lose their dominance on grass surface, and vice versa.


```python

```
