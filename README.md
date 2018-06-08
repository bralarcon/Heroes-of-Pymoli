# Heroes-of-Pymoli

```python
#Observable trend 1: The data clearly shows that men play much more than women, at a rate of 4 to 1.
```


```python
#Observable trend 2:  The normalized data demonstates the other/non-disclosed players have a higher spending price mean than both the 
#women and men categories, but the number of other/non-disclosed is vastly lower than both.
```


```python
#Observable trend 3: The age group is dominated by the 20-24 age range by a significant amount meaning the game is primarily
#played by men in their early twenties.
```


```python
import json

import pandas as pd

```


```python
df = pd.read_json('purchase_data.json')
df.head(10)
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
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
    <tr>
      <th>5</th>
      <td>20</td>
      <td>Male</td>
      <td>10</td>
      <td>Sleepwalker</td>
      <td>1.73</td>
      <td>Tanimnya91</td>
    </tr>
    <tr>
      <th>6</th>
      <td>20</td>
      <td>Male</td>
      <td>153</td>
      <td>Mercenary Sabre</td>
      <td>4.57</td>
      <td>Undjaskla97</td>
    </tr>
    <tr>
      <th>7</th>
      <td>29</td>
      <td>Female</td>
      <td>169</td>
      <td>Interrogator, Blood Blade of the Queen</td>
      <td>3.32</td>
      <td>Iathenudil29</td>
    </tr>
    <tr>
      <th>8</th>
      <td>25</td>
      <td>Male</td>
      <td>118</td>
      <td>Ghost Reaver, Longsword of Magic</td>
      <td>2.77</td>
      <td>Sondenasta63</td>
    </tr>
    <tr>
      <th>9</th>
      <td>31</td>
      <td>Male</td>
      <td>99</td>
      <td>Expiration, Warscythe Of Lost Worlds</td>
      <td>4.53</td>
      <td>Hilaerin92</td>
    </tr>
  </tbody>
</table>
</div>




```python
#total players
df['SN'].shape
```




    (780,)




```python
#unique players
df['SN'].unique().shape
```




    (573,)




```python
#purchasing analysis
#number of unique items
df['Item Name'].nunique()
```




    179




```python
#avg purchase price
df['Price'].mean()
```




    2.931192307692303




```python
#total # of purchases
df['SN'].count()
```




    780




```python
#total rev
df['Price'].sum()
```




    2286.33




```python
#Gender Demographics
#***count of male, female, and other***
df.groupby(['SN','Gender']).count().reset_index()['Gender'].value_counts()

```




    Male                     465
    Female                   100
    Other / Non-Disclosed      8
    Name: Gender, dtype: int64




```python
#percentage of male, female, and other
df.groupby(['SN','Gender']).count().reset_index()['Gender'].value_counts(normalize=True)*100
```




    Male                     81.151832
    Female                   17.452007
    Other / Non-Disclosed     1.396161
    Name: Gender, dtype: float64




```python
#table of gender demographics
norm_count = df.groupby(['SN','Gender']).count().reset_index()['Gender'].value_counts(normalize=False)
norm_percent = df.groupby(['SN','Gender']).count().reset_index()['Gender'].value_counts(normalize=True)*100
pd.concat([norm_count, norm_percent], axis = 1)
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
      <th>Gender</th>
      <th>Gender</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>465</td>
      <td>81.151832</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>100</td>
      <td>17.452007</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>8</td>
      <td>1.396161</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Purchasing Analysis (Gender)
#The below each broken by gender
#Purchase Count
#Average Purchase Price=total purchase value/purchase ct
#Total Purchase Value
#Normalized Totals=total purchase value/unique users
#grouped = df.groupby('Gender').mean().reset_index()
#grouped

#df.groupby(['SN','Gender']).count().reset_index()['Gender'].value_counts()

purch_gender = df.groupby('Gender').agg(['sum','mean','count'])

level0 = purch_gender.columns.get_level_values(0)
level1 = purch_gender.columns.get_level_values(1)
purch_gender.columns = level0 + '_' + level1
purch_gender = purch_gender[['Price_sum', 'Price_mean', 'Price_count']]
purch_gender
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
      <th>Price_sum</th>
      <th>Price_mean</th>
      <th>Price_count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>382.91</td>
      <td>2.815515</td>
      <td>136</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>1867.68</td>
      <td>2.950521</td>
      <td>633</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>35.74</td>
      <td>3.249091</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Normalized Totals=total purchase value/unique users
purch_gender = pd.concat([purch_gender, norm_count], axis=1)
purch_gender['Normalized'] = purch_gender.Price_sum / purch_gender.Gender
purch_gender
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
      <th>Price_sum</th>
      <th>Price_mean</th>
      <th>Price_count</th>
      <th>Gender</th>
      <th>Normalized</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>382.91</td>
      <td>2.815515</td>
      <td>136</td>
      <td>100</td>
      <td>3.829100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>1867.68</td>
      <td>2.950521</td>
      <td>633</td>
      <td>465</td>
      <td>4.016516</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>35.74</td>
      <td>3.249091</td>
      <td>11</td>
      <td>8</td>
      <td>4.467500</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Age Demographics
#Below broken into 4 bins of 4 years
age_bin = df[['Age', 'SN']].drop_duplicates()

ages = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 9999]
a_groups = ["<10", "10-14","15-19","20-24","25-29", "30-34","35-39","40+"]

assert( len(ages)> len(a_groups))

df['Age_Group'] = pd.cut(df['Age'], ages, labels=a_groups)
age_bin['Age_Group'] = pd.cut(age_bin['Age'], ages, labels=a_groups)
age_out = pd.concat([age_bin.Age_Group.value_counts(normalize=True),age_bin.Age_Group.value_counts()],axis=1)
age_out

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
      <th>Age_Group</th>
      <th>Age_Group</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>20-24</th>
      <td>0.452007</td>
      <td>259</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>0.174520</td>
      <td>100</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>0.151832</td>
      <td>87</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>0.082024</td>
      <td>47</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>0.047120</td>
      <td>27</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>0.040140</td>
      <td>23</td>
    </tr>
    <tr>
      <th>&lt;10</th>
      <td>0.033159</td>
      <td>19</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>0.019197</td>
      <td>11</td>
    </tr>
  </tbody>
</table>
</div>




```python
age_norm = df.groupby('Age_Group').agg(['sum', 'mean','count'])['Price']
age_norm.reset_index(inplace=True)
age_norm['unique_buyers'] = age_norm['Age_Group'].map(lambda x: age_out.to_dict()['Age_Group'].get(x)) 
age_norm['normed_mean'] = age_norm['unique_buyers'].astype('float')
age_norm
```

    C:\Users\Josue\Anaconda3.1\lib\site-packages\ipykernel_launcher.py:3: UserWarning: DataFrame columns are not unique, some columns will be omitted.
      This is separate from the ipykernel package so we can avoid doing imports until
    




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
      <th>Age_Group</th>
      <th>sum</th>
      <th>mean</th>
      <th>count</th>
      <th>unique_buyers</th>
      <th>normed_mean</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>&lt;10</td>
      <td>83.46</td>
      <td>2.980714</td>
      <td>28</td>
      <td>19</td>
      <td>19.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10-14</td>
      <td>96.95</td>
      <td>2.770000</td>
      <td>35</td>
      <td>23</td>
      <td>23.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>15-19</td>
      <td>386.42</td>
      <td>2.905414</td>
      <td>133</td>
      <td>100</td>
      <td>100.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>20-24</td>
      <td>978.77</td>
      <td>2.913006</td>
      <td>336</td>
      <td>259</td>
      <td>259.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>25-29</td>
      <td>370.33</td>
      <td>2.962640</td>
      <td>125</td>
      <td>87</td>
      <td>87.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>30-34</td>
      <td>197.25</td>
      <td>3.082031</td>
      <td>64</td>
      <td>47</td>
      <td>47.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>35-39</td>
      <td>119.40</td>
      <td>2.842857</td>
      <td>42</td>
      <td>27</td>
      <td>27.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>40+</td>
      <td>53.75</td>
      <td>3.161765</td>
      <td>17</td>
      <td>11</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Top spenders
#Top 5 by total purchase value
#SN
#Purchase ct
#Avg purchase price
#Total purchase value
df.groupby('SN')['Price'].agg(['sum','mean','count']).sort_values(by='sum',ascending=False).head(5)
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
      <th>sum</th>
      <th>mean</th>
      <th>count</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>17.06</td>
      <td>3.412000</td>
      <td>5</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>13.56</td>
      <td>3.390000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>12.74</td>
      <td>3.185000</td>
      <td>4</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>12.73</td>
      <td>4.243333</td>
      <td>3</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>11.58</td>
      <td>3.860000</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Pop items
#the 5 most pop items by purchase ct, then list item id, item name, purch ct, item price total purch value
#items_counts = df['Item Name'].value_counts().reset_index()
#items_counts.head(5)
df.groupby('Item Name')['Price'].agg(['sum','mean','count']).sort_values(by='count',ascending=False).head(5)

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
      <th>sum</th>
      <th>mean</th>
      <th>count</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>38.60</td>
      <td>2.757143</td>
      <td>14</td>
    </tr>
    <tr>
      <th>Arcane Gem</th>
      <td>24.53</td>
      <td>2.230000</td>
      <td>11</td>
    </tr>
    <tr>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>25.85</td>
      <td>2.350000</td>
      <td>11</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>34.65</td>
      <td>3.465000</td>
      <td>10</td>
    </tr>
    <tr>
      <th>Woeful Adamantite Claymore</th>
      <td>11.16</td>
      <td>1.240000</td>
      <td>9</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Profitable Items
#Identify the 5 most profitable items by total purchase value
#Item Name
#Purchase ct
#Item price
#total purchase value
df.groupby('Item Name')['Price'].agg(['sum','mean','count']).sort_values(by='sum',ascending=False).head(5)



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
      <th>sum</th>
      <th>mean</th>
      <th>count</th>
    </tr>
    <tr>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Final Critic</th>
      <td>38.60</td>
      <td>2.757143</td>
      <td>14</td>
    </tr>
    <tr>
      <th>Retribution Axe</th>
      <td>37.26</td>
      <td>4.140000</td>
      <td>9</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>34.65</td>
      <td>3.465000</td>
      <td>10</td>
    </tr>
    <tr>
      <th>Spectral Diamond Doomblade</th>
      <td>29.75</td>
      <td>4.250000</td>
      <td>7</td>
    </tr>
    <tr>
      <th>Orenmir</th>
      <td>29.70</td>
      <td>4.950000</td>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>


