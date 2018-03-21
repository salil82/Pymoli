

```python
# Observed trend 1: Most of the purchasers are between the ages of 15 and 30 with the largest bin being 20-24
# Observed trend 2: Retribution Axe is the most profitable item and one of the most popular items
# Observed trend 3: There is not much overlap between the most profitable and most popular items. None of the top 5 most popular
# are among the top 5 most profitable items.
```


```python
import pandas as pd
import numpy as np
```


```python
house_data = "purchase_data.json"
```


```python
house_data_df = pd.read_json(house_data)
house_data_df.head()
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
  </tbody>
</table>
</div>




```python
house_data_df['SN'].count()
```




    780




```python
# Total number of players

purchasers = house_data_df.groupby('SN')['SN'].nunique()
player_count = pd.DataFrame({"Total Players":[purchasers.count()]})
player_count
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
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Number of unique items
unique_items = house_data_df.groupby('Item ID')['Item ID'].nunique()
unique_count = unique_items.count()
unique_count
```




    183




```python
# Total Number of Purchases
total_purchases = house_data_df['SN'].count()
total_purchases
```




    780




```python
# Total Revenue

total_revenue = house_data_df['Price'].sum()
total_revenue
```




    2286.33




```python
# Average purchase price

avg_purchase_price = total_revenue / total_purchases
avg_purchase_price
```




    2.9311923076923074




```python
purchasing_analysis = pd.DataFrame({"Number of Unique Items":[unique_count], 
                                    "Average Price":[avg_purchase_price], 
                                    "Number of Purchases":[total_purchases],
                                    "Total Revenue":[total_revenue]
                                   })
purchasing_analysis2 = purchasing_analysis[["Number of Unique Items",
                                            "Average Price",
                                            "Number of Purchases",
                                            "Total Revenue"
                                           ]] 

purchasing_analysis2["Average Price"] = purchasing_analysis2["Average Price"].map("${:.2f}".format)
purchasing_analysis2["Total Revenue"] = purchasing_analysis2["Total Revenue"].map("${:.2f}".format)
purchasing_analysis2.head()
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
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>183</td>
      <td>$2.93</td>
      <td>780</td>
      <td>$2286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
gender_data = house_data_df.groupby('Gender')['SN'].nunique()
gender_data
```




    Gender
    Female                   100
    Male                     465
    Other / Non-Disclosed      8
    Name: SN, dtype: int64




```python
# Percentage and count of male players
males = gender_data[1]
perc_males = males/gender_data.sum() * 100
print("Percentage of male players: " + str(perc_males))
print("Count of males: " + str(males))
```

    Percentage of male players: 81.15183246073299
    Count of males: 465
    


```python
# Percentage and count of female players
females = gender_data[0]
perc_females = females/gender_data.sum() * 100
print("Percentage of female players: " + str(perc_females))
print("Count of females: " + str(females))
```

    Percentage of female players: 17.452006980802793
    Count of females: 100
    


```python
# Percentage and count of female players
females = gender_data[0]
perc_females = females/gender_data.sum() * 100
print("Percentage of female players: " + str(perc_females))
print("Count of females: " + str(females))
```

    Percentage of female players: 17.452006980802793
    Count of females: 100
    


```python
# Percentage and count of Other/Non-disclosed players
other = gender_data[2]
perc_other = other/gender_data.sum() * 100
print("Percentage of Other/Non-disclosed players: " + str(perc_other))
print("Count of Other/Non-disclosed: " + str(other))
```

    Percentage of Other/Non-disclosed players: 1.3961605584642234
    Count of Other/Non-disclosed: 8
    


```python
gender_demo = pd.DataFrame({"Total Count":gender_data
                     })
percentages =  (gender_demo["Total Count"]/gender_data.sum()) * 100
gender_demo["Percentage of Players"] = percentages

gender_demo = pd.DataFrame({"Total Count":gender_data,
                            "Percentage of Players":percentages
                     })

gender_demo["Percentage of Players"] = gender_demo["Percentage of Players"].map("{:.2f}".format)

gender_demo
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
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>17.45</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>81.15</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
gender_groups = house_data_df.groupby(['Gender'])

gender_purchase = gender_groups["SN"].count()

avg_purchase_price = gender_groups["Price"].mean()

total_purchase_value = gender_groups["Price"].sum()

norm_totals = total_purchase_value/gender_data

gender_pa = pd.DataFrame({"Purchase Count":gender_purchase,
                            "Average Purchase Price":avg_purchase_price,
                            "Total Purchase Value":total_purchase_value,
                            "Normalized Totals":norm_totals,
                     })

gender_pa["Average Purchase Price"] = gender_pa["Average Purchase Price"].map("${:.2f}".format)
gender_pa["Total Purchase Value"] = gender_pa["Total Purchase Value"].map("${:.2f}".format)
gender_pa["Normalized Totals"] = gender_pa["Normalized Totals"].map("${:.2f}".format)

#Reorganizing Columns
gender_pa2 = gender_pa[["Purchase Count",
                                       "Average Purchase Price",
                                       "Total Purchase Value",
                                       "Normalized Totals"
                                       ]]


gender_pa2
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>136</td>
      <td>$2.82</td>
      <td>$382.91</td>
      <td>$3.83</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>633</td>
      <td>$2.95</td>
      <td>$1867.68</td>
      <td>$4.02</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>11</td>
      <td>$3.25</td>
      <td>$35.74</td>
      <td>$4.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Age Demographics

bins = [0,10,14,19,24,29,34,39,100]
bin_names = ['<10','10-14','15-19','20-24','25-29','30-34','35-39','40+']
house_data_df[' '] = pd.cut(house_data_df["Age"], bins, labels=bin_names)
df_age_group = house_data_df.groupby(' ')


purchase_count = df_age_group["Age"].count()

avg_purchase_price = df_age_group["Price"].mean()

total_purchase_value = df_age_group["Price"].sum()

unique_age_count =df_age_group[" "].count()
norm_age = total_purchase_value/unique_age_count

df_age_group1 = pd.DataFrame({"Purchase Count":purchase_count,
                            "Average Purchase Price":avg_purchase_price,
                            "Total Purchase Value":total_purchase_value,
                            "Normalized Totals":norm_age
                     })
 
df_age_group1["Average Purchase Price"] = df_age_group1["Average Purchase Price"].map("${:.2f}".format)
df_age_group1["Total Purchase Value"] = df_age_group1["Total Purchase Value"].map("${:.2f}".format)
df_age_group1["Normalized Totals"] = df_age_group1["Normalized Totals"].map("${:.2f}".format)

df_age_group2 = df_age_group1[["Purchase Count",
                            "Average Purchase Price",
                            "Total Purchase Value",
                            "Normalized Totals"
                           ]]


df_age_group2
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>32</td>
      <td>$3.02</td>
      <td>$96.62</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>31</td>
      <td>$2.70</td>
      <td>$83.79</td>
      <td>$2.70</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>133</td>
      <td>$2.91</td>
      <td>$386.42</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>336</td>
      <td>$2.91</td>
      <td>$978.77</td>
      <td>$2.91</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>125</td>
      <td>$2.96</td>
      <td>$370.33</td>
      <td>$2.96</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>64</td>
      <td>$3.08</td>
      <td>$197.25</td>
      <td>$3.08</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>42</td>
      <td>$2.84</td>
      <td>$119.40</td>
      <td>$2.84</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>17</td>
      <td>$3.16</td>
      <td>$53.75</td>
      <td>$3.16</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders


top_spenders = house_data_df.groupby(['SN'])

purchase_count_sn = top_spenders["SN"].count()

avg_purchase_price = top_spenders["Price"].mean()

total_purchase_value = top_spenders["Price"].sum()

top_spender_summary = pd.DataFrame({"Purchase Count":purchase_count_sn,
                            "Average Purchase Price":avg_purchase_price,
                            "Total Purchase Value":total_purchase_value
                     })

top_spender_summary["Average Purchase Price"] = top_spender_summary["Average Purchase Price"].map("${:.2f}".format)
top_spender_summary["Total Purchase Value"] = top_spender_summary["Total Purchase Value"].map("${:.2f}".format)

top_spender_summary2 = top_spender_summary[["Purchase Count",
                                       "Average Purchase Price",
                                       "Total Purchase Value",
                                       ]]

top_spender_summary3 = top_spender_summary2.sort_values('Total Purchase Value', ascending=False)
top_spender_summary3.head()
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
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
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
      <th>Qarwen67</th>
      <td>4</td>
      <td>$2.49</td>
      <td>$9.97</td>
    </tr>
    <tr>
      <th>Sondim43</th>
      <td>3</td>
      <td>$3.13</td>
      <td>$9.38</td>
    </tr>
    <tr>
      <th>Tillyrin30</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Lisistaya47</th>
      <td>3</td>
      <td>$3.06</td>
      <td>$9.19</td>
    </tr>
    <tr>
      <th>Tyisriphos58</th>
      <td>2</td>
      <td>$4.59</td>
      <td>$9.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Most Popular Items

pop_items = house_data_df.groupby(['Item ID','Item Name'])

purchase_count_it = pop_items["SN"].count()

avg_purchase_price = pop_items["Price"].mean()

total_purchase_value = pop_items["Price"].sum()

pop_items_table = pd.DataFrame({"Purchase Count":purchase_count_it,
                            "Item Price":avg_purchase_price,
                            "Total Purchase Value":total_purchase_value
                     })
 
pop_items_table["Item Price"] = pop_items_table["Item Price"].map("${:.2f}".format)
pop_items_table["Total Purchase Value"] = pop_items_table["Total Purchase Value"].map("${:.2f}".format)

pop_items_table2 = pop_items_table[["Purchase Count",
                                       "Item Price",
                                       "Total Purchase Value",
                                       ]]

pop_items_table3 = pop_items_table2.sort_values('Purchase Count', ascending=False)
pop_items_table3.head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>9</td>
      <td>$2.07</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>9</td>
      <td>$1.24</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>9</td>
      <td>$1.49</td>
      <td>$13.41</td>
    </tr>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>65</th>
      <th>Conqueror Adamantite Mace</th>
      <td>8</td>
      <td>$1.96</td>
      <td>$15.68</td>
    </tr>
    <tr>
      <th>152</th>
      <th>Darkheart</th>
      <td>8</td>
      <td>$3.15</td>
      <td>$25.20</td>
    </tr>
    <tr>
      <th>44</th>
      <th>Bonecarvin Battle Axe</th>
      <td>8</td>
      <td>$2.46</td>
      <td>$19.68</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
    <tr>
      <th>106</th>
      <th>Crying Steel Sickle</th>
      <td>8</td>
      <td>$2.29</td>
      <td>$18.32</td>
    </tr>
    <tr>
      <th>92</th>
      <th>Final Critic</th>
      <td>8</td>
      <td>$1.36</td>
      <td>$10.88</td>
    </tr>
    <tr>
      <th>158</th>
      <th>Darkheart, Butcher of the Champion</th>
      <td>7</td>
      <td>$3.56</td>
      <td>$24.92</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>7</td>
      <td>$3.39</td>
      <td>$23.73</td>
    </tr>
    <tr>
      <th>66</th>
      <th>Victor Iron Spikes</th>
      <td>7</td>
      <td>$3.55</td>
      <td>$24.85</td>
    </tr>
    <tr>
      <th>79</th>
      <th>Alpha, Oath of Zeal</th>
      <td>7</td>
      <td>$2.88</td>
      <td>$20.16</td>
    </tr>
    <tr>
      <th>154</th>
      <th>Feral Katana</th>
      <td>7</td>
      <td>$2.19</td>
      <td>$15.33</td>
    </tr>
    <tr>
      <th>130</th>
      <th>Alpha</th>
      <td>7</td>
      <td>$1.56</td>
      <td>$10.92</td>
    </tr>
    <tr>
      <th>172</th>
      <th>Blade of the Grave</th>
      <td>7</td>
      <td>$1.69</td>
      <td>$11.83</td>
    </tr>
    <tr>
      <th>18</th>
      <th>Torchlight, Bond of Storms</th>
      <td>7</td>
      <td>$1.77</td>
      <td>$12.39</td>
    </tr>
    <tr>
      <th>179</th>
      <th>Wolf, Promise of the Moonwalker</th>
      <td>7</td>
      <td>$1.88</td>
      <td>$13.16</td>
    </tr>
    <tr>
      <th>11</th>
      <th>Brimstone</th>
      <td>7</td>
      <td>$2.52</td>
      <td>$17.64</td>
    </tr>
    <tr>
      <th>91</th>
      <th>Celeste</th>
      <td>6</td>
      <td>$3.71</td>
      <td>$22.26</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>47</th>
      <th>Alpha, Reach of Ending Hope</th>
      <td>6</td>
      <td>$1.55</td>
      <td>$9.30</td>
    </tr>
    <tr>
      <th>7</th>
      <th>Thorn, Satchel of Dark Souls</th>
      <td>6</td>
      <td>$4.51</td>
      <td>$27.06</td>
    </tr>
    <tr>
      <th>8</th>
      <th>Purgatory, Gem of Regret</th>
      <td>6</td>
      <td>$3.91</td>
      <td>$23.46</td>
    </tr>
    <tr>
      <th>10</th>
      <th>Sleepwalker</th>
      <td>6</td>
      <td>$1.73</td>
      <td>$10.38</td>
    </tr>
    <tr>
      <th>85</th>
      <th>Malificent Bag</th>
      <td>6</td>
      <td>$2.17</td>
      <td>$13.02</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>116</th>
      <th>Renewed Skeletal Katana</th>
      <td>2</td>
      <td>$2.37</td>
      <td>$4.74</td>
    </tr>
    <tr>
      <th>96</th>
      <th>Blood-Forged Skeletal Spine</th>
      <td>2</td>
      <td>$4.77</td>
      <td>$9.54</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>2</td>
      <td>$2.41</td>
      <td>$4.82</td>
    </tr>
    <tr>
      <th>19</th>
      <th>Pursuit, Cudgel of Necromancy</th>
      <td>2</td>
      <td>$3.98</td>
      <td>$7.96</td>
    </tr>
    <tr>
      <th>62</th>
      <th>Piece Maker</th>
      <td>2</td>
      <td>$4.36</td>
      <td>$8.72</td>
    </tr>
    <tr>
      <th>55</th>
      <th>Vindictive Glass Edge</th>
      <td>2</td>
      <td>$4.26</td>
      <td>$8.52</td>
    </tr>
    <tr>
      <th>24</th>
      <th>Warped Fetish</th>
      <td>2</td>
      <td>$2.41</td>
      <td>$4.82</td>
    </tr>
    <tr>
      <th>80</th>
      <th>Dreamsong</th>
      <td>2</td>
      <td>$3.81</td>
      <td>$7.62</td>
    </tr>
    <tr>
      <th>150</th>
      <th>Deathraze</th>
      <td>2</td>
      <td>$4.54</td>
      <td>$9.08</td>
    </tr>
    <tr>
      <th>149</th>
      <th>Tranquility, Razor of Black Magic</th>
      <td>2</td>
      <td>$2.47</td>
      <td>$4.94</td>
    </tr>
    <tr>
      <th>58</th>
      <th>Freak's Bite, Favor of Holy Might</th>
      <td>2</td>
      <td>$3.03</td>
      <td>$6.06</td>
    </tr>
    <tr>
      <th>146</th>
      <th>Warped Iron Scimitar</th>
      <td>2</td>
      <td>$4.08</td>
      <td>$8.16</td>
    </tr>
    <tr>
      <th>64</th>
      <th>Fusion Pummel</th>
      <td>2</td>
      <td>$3.58</td>
      <td>$7.16</td>
    </tr>
    <tr>
      <th>132</th>
      <th>Persuasion</th>
      <td>2</td>
      <td>$3.90</td>
      <td>$7.80</td>
    </tr>
    <tr>
      <th>56</th>
      <th>Foul Titanium Battle Axe</th>
      <td>2</td>
      <td>$4.33</td>
      <td>$8.66</td>
    </tr>
    <tr>
      <th>89</th>
      <th>Blazefury, Protector of Delusions</th>
      <td>2</td>
      <td>$1.50</td>
      <td>$3.00</td>
    </tr>
    <tr>
      <th>156</th>
      <th>Soul-Forged Steel Shortsword</th>
      <td>1</td>
      <td>$1.16</td>
      <td>$1.16</td>
    </tr>
    <tr>
      <th>109</th>
      <th>Downfall, Scalpel Of The Emperor</th>
      <td>1</td>
      <td>$3.20</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>2</th>
      <th>Verdict</th>
      <td>1</td>
      <td>$3.40</td>
      <td>$3.40</td>
    </tr>
    <tr>
      <th>136</th>
      <th>Ghastly Adamantite Protector</th>
      <td>1</td>
      <td>$3.30</td>
      <td>$3.30</td>
    </tr>
    <tr>
      <th>3</th>
      <th>Phantomlight</th>
      <td>1</td>
      <td>$1.79</td>
      <td>$1.79</td>
    </tr>
    <tr>
      <th>4</th>
      <th>Bloodlord's Fetish</th>
      <td>1</td>
      <td>$2.28</td>
      <td>$2.28</td>
    </tr>
    <tr>
      <th>126</th>
      <th>Exiled Mithril Longsword</th>
      <td>1</td>
      <td>$3.25</td>
      <td>$3.25</td>
    </tr>
    <tr>
      <th>43</th>
      <th>Foul Edge</th>
      <td>1</td>
      <td>$2.38</td>
      <td>$2.38</td>
    </tr>
    <tr>
      <th>28</th>
      <th>Flux, Destroyer of Due Diligence</th>
      <td>1</td>
      <td>$3.04</td>
      <td>$3.04</td>
    </tr>
    <tr>
      <th>147</th>
      <th>Hellreaver, Heirloom of Inception</th>
      <td>1</td>
      <td>$3.59</td>
      <td>$3.59</td>
    </tr>
    <tr>
      <th>168</th>
      <th>Sun Strike, Jaws of Twisted Visions</th>
      <td>1</td>
      <td>$2.64</td>
      <td>$2.64</td>
    </tr>
    <tr>
      <th>164</th>
      <th>Exiled Doomblade</th>
      <td>1</td>
      <td>$1.92</td>
      <td>$1.92</td>
    </tr>
    <tr>
      <th>59</th>
      <th>Lightning, Etcher of the King</th>
      <td>1</td>
      <td>$1.65</td>
      <td>$1.65</td>
    </tr>
    <tr>
      <th>0</th>
      <th>Splinter</th>
      <td>1</td>
      <td>$1.82</td>
      <td>$1.82</td>
    </tr>
  </tbody>
</table>
<p>183 rows × 3 columns</p>
</div>




```python
most_profitable = house_data_df.groupby(['Item ID','Item Name'])

purchase_count_prof = most_profitable["SN"].count()

avg_purchase_price = most_profitable["Price"].mean()

total_purchase_value = most_profitable["Price"].sum()

most_profitable_table = pd.DataFrame({"Purchase Count":purchase_count_prof,
                            "Item Price":avg_purchase_price,
                            "Total Purchase Value":total_purchase_value
                     })


most_profitable_table2 = most_profitable_table[["Purchase Count",
                                       "Item Price",
                                       "Total Purchase Value",
                                       ]]

most_profitable_table3 = most_profitable_table2.sort_values('Total Purchase Value', ascending=False)
most_profitable_table3.head()

most_profitable_table3["Item Price"] = most_profitable_table3["Item Price"].map("${:.2f}".format)
most_profitable_table3["Total Purchase Value"] = most_profitable_table3["Total Purchase Value"].map("${:.2f}".format)
most_profitable_table3.head()
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
      <th></th>
      <th>Purchase Count</th>
      <th>Item Price</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>9</td>
      <td>$4.14</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>7</td>
      <td>$4.25</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>6</td>
      <td>$4.95</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>6</td>
      <td>$4.87</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>8</td>
      <td>$3.61</td>
      <td>$28.88</td>
    </tr>
    <tr>
      <th>101</th>
      <th>Final Critic</th>
      <td>6</td>
      <td>$4.62</td>
      <td>$27.72</td>
    </tr>
    <tr>
      <th>7</th>
      <th>Thorn, Satchel of Dark Souls</th>
      <td>6</td>
      <td>$4.51</td>
      <td>$27.06</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>6</td>
      <td>$4.45</td>
      <td>$26.70</td>
    </tr>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>11</td>
      <td>$2.35</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>152</th>
      <th>Darkheart</th>
      <td>8</td>
      <td>$3.15</td>
      <td>$25.20</td>
    </tr>
    <tr>
      <th>102</th>
      <th>Avenger</th>
      <td>6</td>
      <td>$4.16</td>
      <td>$24.96</td>
    </tr>
    <tr>
      <th>158</th>
      <th>Darkheart, Butcher of the Champion</th>
      <td>7</td>
      <td>$3.56</td>
      <td>$24.92</td>
    </tr>
    <tr>
      <th>66</th>
      <th>Victor Iron Spikes</th>
      <td>7</td>
      <td>$3.55</td>
      <td>$24.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>11</td>
      <td>$2.23</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>173</th>
      <th>Stormfury Longsword</th>
      <td>5</td>
      <td>$4.83</td>
      <td>$24.15</td>
    </tr>
    <tr>
      <th>128</th>
      <th>Blazeguard, Reach of Eternity</th>
      <td>6</td>
      <td>$4.00</td>
      <td>$24.00</td>
    </tr>
    <tr>
      <th>148</th>
      <th>Warmonger, Gift of Suffering's End</th>
      <td>6</td>
      <td>$3.96</td>
      <td>$23.76</td>
    </tr>
    <tr>
      <th>46</th>
      <th>Hopeless Ebon Dualblade</th>
      <td>5</td>
      <td>$4.75</td>
      <td>$23.75</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>7</td>
      <td>$3.39</td>
      <td>$23.73</td>
    </tr>
    <tr>
      <th>8</th>
      <th>Purgatory, Gem of Regret</th>
      <td>6</td>
      <td>$3.91</td>
      <td>$23.46</td>
    </tr>
    <tr>
      <th>153</th>
      <th>Mercenary Sabre</th>
      <td>5</td>
      <td>$4.57</td>
      <td>$22.85</td>
    </tr>
    <tr>
      <th>73</th>
      <th>Ritual Mace</th>
      <td>6</td>
      <td>$3.74</td>
      <td>$22.44</td>
    </tr>
    <tr>
      <th>91</th>
      <th>Celeste</th>
      <td>6</td>
      <td>$3.71</td>
      <td>$22.26</td>
    </tr>
    <tr>
      <th>48</th>
      <th>Rage, Legacy of the Lone Victor</th>
      <td>5</td>
      <td>$4.32</td>
      <td>$21.60</td>
    </tr>
    <tr>
      <th>12</th>
      <th>Dawne</th>
      <td>5</td>
      <td>$4.30</td>
      <td>$21.50</td>
    </tr>
    <tr>
      <th>22</th>
      <th>Amnesia</th>
      <td>6</td>
      <td>$3.57</td>
      <td>$21.42</td>
    </tr>
    <tr>
      <th>49</th>
      <th>The Oculus, Token of Lost Worlds</th>
      <td>5</td>
      <td>$4.23</td>
      <td>$21.15</td>
    </tr>
    <tr>
      <th>30</th>
      <th>Stormcaller</th>
      <td>5</td>
      <td>$4.15</td>
      <td>$20.75</td>
    </tr>
    <tr>
      <th>121</th>
      <th>Massacre</th>
      <td>6</td>
      <td>$3.42</td>
      <td>$20.52</td>
    </tr>
    <tr>
      <th>81</th>
      <th>Dreamkiss</th>
      <td>5</td>
      <td>$4.06</td>
      <td>$20.30</td>
    </tr>
    <tr>
      <th>...</th>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>149</th>
      <th>Tranquility, Razor of Black Magic</th>
      <td>2</td>
      <td>$2.47</td>
      <td>$4.94</td>
    </tr>
    <tr>
      <th>24</th>
      <th>Warped Fetish</th>
      <td>2</td>
      <td>$2.41</td>
      <td>$4.82</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>2</td>
      <td>$2.41</td>
      <td>$4.82</td>
    </tr>
    <tr>
      <th>167</th>
      <th>Malice, Legacy of the Queen</th>
      <td>2</td>
      <td>$2.38</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>116</th>
      <th>Renewed Skeletal Katana</th>
      <td>2</td>
      <td>$2.37</td>
      <td>$4.74</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>4</td>
      <td>$1.11</td>
      <td>$4.44</td>
    </tr>
    <tr>
      <th>25</th>
      <th>Hero Cane</th>
      <td>4</td>
      <td>$1.03</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>9</th>
      <th>Thorn, Conqueror of the Corrupted</th>
      <td>2</td>
      <td>$2.04</td>
      <td>$4.08</td>
    </tr>
    <tr>
      <th>5</th>
      <th>Putrid Fan</th>
      <td>3</td>
      <td>$1.32</td>
      <td>$3.96</td>
    </tr>
    <tr>
      <th>122</th>
      <th>Unending Tyranny</th>
      <td>3</td>
      <td>$1.21</td>
      <td>$3.63</td>
    </tr>
    <tr>
      <th>6</th>
      <th>Rusty Skull</th>
      <td>3</td>
      <td>$1.20</td>
      <td>$3.60</td>
    </tr>
    <tr>
      <th>147</th>
      <th>Hellreaver, Heirloom of Inception</th>
      <td>1</td>
      <td>$3.59</td>
      <td>$3.59</td>
    </tr>
    <tr>
      <th>41</th>
      <th>Orbit</th>
      <td>3</td>
      <td>$1.16</td>
      <td>$3.48</td>
    </tr>
    <tr>
      <th>2</th>
      <th>Verdict</th>
      <td>1</td>
      <td>$3.40</td>
      <td>$3.40</td>
    </tr>
    <tr>
      <th>136</th>
      <th>Ghastly Adamantite Protector</th>
      <td>1</td>
      <td>$3.30</td>
      <td>$3.30</td>
    </tr>
    <tr>
      <th>126</th>
      <th>Exiled Mithril Longsword</th>
      <td>1</td>
      <td>$3.25</td>
      <td>$3.25</td>
    </tr>
    <tr>
      <th>109</th>
      <th>Downfall, Scalpel Of The Emperor</th>
      <td>1</td>
      <td>$3.20</td>
      <td>$3.20</td>
    </tr>
    <tr>
      <th>69</th>
      <th>Frenzy, Defender of the Harvest</th>
      <td>3</td>
      <td>$1.06</td>
      <td>$3.18</td>
    </tr>
    <tr>
      <th>28</th>
      <th>Flux, Destroyer of Due Diligence</th>
      <td>1</td>
      <td>$3.04</td>
      <td>$3.04</td>
    </tr>
    <tr>
      <th>89</th>
      <th>Blazefury, Protector of Delusions</th>
      <td>2</td>
      <td>$1.50</td>
      <td>$3.00</td>
    </tr>
    <tr>
      <th>72</th>
      <th>Winter's Bite</th>
      <td>2</td>
      <td>$1.39</td>
      <td>$2.78</td>
    </tr>
    <tr>
      <th>168</th>
      <th>Sun Strike, Jaws of Twisted Visions</th>
      <td>1</td>
      <td>$2.64</td>
      <td>$2.64</td>
    </tr>
    <tr>
      <th>43</th>
      <th>Foul Edge</th>
      <td>1</td>
      <td>$2.38</td>
      <td>$2.38</td>
    </tr>
    <tr>
      <th>4</th>
      <th>Bloodlord's Fetish</th>
      <td>1</td>
      <td>$2.28</td>
      <td>$2.28</td>
    </tr>
    <tr>
      <th>74</th>
      <th>Yearning Crusher</th>
      <td>2</td>
      <td>$1.06</td>
      <td>$2.12</td>
    </tr>
    <tr>
      <th>164</th>
      <th>Exiled Doomblade</th>
      <td>1</td>
      <td>$1.92</td>
      <td>$1.92</td>
    </tr>
    <tr>
      <th>0</th>
      <th>Splinter</th>
      <td>1</td>
      <td>$1.82</td>
      <td>$1.82</td>
    </tr>
    <tr>
      <th>3</th>
      <th>Phantomlight</th>
      <td>1</td>
      <td>$1.79</td>
      <td>$1.79</td>
    </tr>
    <tr>
      <th>59</th>
      <th>Lightning, Etcher of the King</th>
      <td>1</td>
      <td>$1.65</td>
      <td>$1.65</td>
    </tr>
    <tr>
      <th>156</th>
      <th>Soul-Forged Steel Shortsword</th>
      <td>1</td>
      <td>$1.16</td>
      <td>$1.16</td>
    </tr>
  </tbody>
</table>
<p>183 rows × 3 columns</p>
</div>


