### Note
* Instructions have been included for each segment. You do not have to follow them exactly, but they are included to help you think through the steps.


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load (Remember to Change These)
file = "/Users/kristenhanold/Desktop/GaTechBootCamp/Pandas/purchase_data.csv"

# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file)
purchase_data.head()
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
      <th>Purchase ID</th>
      <th>SN</th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Lisim78</td>
      <td>20</td>
      <td>Male</td>
      <td>108</td>
      <td>Extraction, Quickblade Of Trembling Hands</td>
      <td>3.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Lisovynya38</td>
      <td>40</td>
      <td>Male</td>
      <td>143</td>
      <td>Frenzied Scimitar</td>
      <td>1.56</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Ithergue48</td>
      <td>24</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>4.88</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Chamassasya86</td>
      <td>24</td>
      <td>Male</td>
      <td>100</td>
      <td>Blindscythe</td>
      <td>3.27</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Iskosia90</td>
      <td>23</td>
      <td>Male</td>
      <td>131</td>
      <td>Fury</td>
      <td>1.44</td>
    </tr>
  </tbody>
</table>
</div>



## Player Count

* Display the total number of players



```python
players = len(purchase_data['SN'].value_counts())
total_players_df = pd.DataFrame({
    'Total Players': [players]})

total_players_df
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
      <td>576</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Total)

* Run basic calculations to obtain number of unique items, average price, etc.


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame



```python
num_unique_items = len(purchase_data['Item ID'].unique())
avg_price = purchase_data['Price'].mean()
num_purchases = purchase_data['Purchase ID'].count()
total_revenue = purchase_data['Price'].sum()

purchase_analysis_df = pd.DataFrame({
    'Number of Unique Items': [num_unique_items],
    'Average Price': [avg_price],
    'Number of Purchases': [num_purchases],
    'Total Revenue': [total_revenue]})

# formatting selected columns using .map()
purchase_analysis_df['Average Price'] = purchase_analysis_df['Average Price'].map('${:.2f}'.format)
purchase_analysis_df['Total Revenue'] = purchase_analysis_df['Total Revenue'].map('${:,.2f}'.format)
                                                                                  
purchase_analysis_df
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
      <td>179</td>
      <td>$3.05</td>
      <td>780</td>
      <td>$2,379.77</td>
    </tr>
  </tbody>
</table>
</div>



## Gender Demographics

* Percentage and Count of Male Players


* Percentage and Count of Female Players


* Percentage and Count of Other / Non-Disclosed





```python
# locate only the columns SN and Gender
gender_df = purchase_data.iloc[:, [1,3]]

# drop duplicates of players listed in SN so we can get an accurate count for each gender
new_gender_df = gender_df.drop_duplicates('SN')

# count each gender
gender_count = new_gender_df['Gender'].value_counts()

# find percentage of each gender
male_percent = round((gender_count['Male']/players)*100, 2)
female_percent = round((gender_count['Female']/players)*100, 2)
other_percent = round((gender_count['Other / Non-Disclosed']/players)*100, 2)

# create new data frame for gender analysis
gender_analysis = pd.DataFrame({
    " ":['Male', 'Female', 'Other / Non-Disclosed'],\
    'Count Per Gender': [gender_count[0], gender_count[1], gender_count[2]],\
    'Percentage of Players': [male_percent, female_percent, other_percent]
})

gender_analysis['Percentage of Players'] = gender_analysis['Percentage of Players'].map('{:.2f}%'.format)

gender_analysis
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
      <th>Count Per Gender</th>
      <th>Percentage of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Male</td>
      <td>484</td>
      <td>84.03%</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Female</td>
      <td>81</td>
      <td>14.06%</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Other / Non-Disclosed</td>
      <td>11</td>
      <td>1.91%</td>
    </tr>
  </tbody>
</table>
</div>




## Purchasing Analysis (Gender)

* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender




* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
purchase_by_gender = purchase_data.groupby('Gender')

purchase_count = purchase_by_gender['Purchase ID'].count()
avg_purchase_price = purchase_by_gender['Price'].mean().map('${:.2f}'.format)
total_purchase_value = purchase_by_gender['Price'].sum().map('${:,.2f}'.format)
avg_per_person = (purchase_by_gender['Price'].sum() / gender_count).map('${:.2f}'.format)

purchasing_analysis = pd.DataFrame({
    'Purchase Count': purchase_count,
    'Average Purchase Price': avg_purchase_price,
    'Total Purchase Value': total_purchase_value,
    'Avg Total Purchase per Person': avg_per_person
})

purchasing_analysis
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
      <th>Avg Total Purchase per Person</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Female</th>
      <td>113</td>
      <td>$3.20</td>
      <td>$361.94</td>
      <td>$4.47</td>
    </tr>
    <tr>
      <th>Male</th>
      <td>652</td>
      <td>$3.02</td>
      <td>$1,967.64</td>
      <td>$4.07</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>15</td>
      <td>$3.35</td>
      <td>$50.19</td>
      <td>$4.56</td>
    </tr>
  </tbody>
</table>
</div>



## Age Demographics

* Establish bins for ages


* Categorize the existing players using the age bins. Hint: use pd.cut()


* Calculate the numbers and percentages by age group


* Create a summary data frame to hold the results


* Optional: round the percentage column to two decimal points


* Display Age Demographics Table



```python
age_bins = [0, 9.99, 14.99, 19.99, 24.99, 29.99, 34.99, 39.99, 9999]
group_names = ('<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+')

purchase_data.loc[:, ['SN', 'Age']] = loc_age_groups
new_age_groups = loc_age_groups.drop_duplicates('SN')
new_age_groups["Age Groups"] = pd.cut(new_age_groups["Age"], age_bins, labels = group_names)

# Creating a group based off of the bins
age_groups_df = new_age_groups.groupby("Age Groups")

total_count = age_groups_df["Age Groups"].count()
percent_of_players = round((total_count / players)*100, 2).map('{:.2f}%'.format)


age_demographics_df = pd.DataFrame({
    'Total Count': total_count,
    'Percentage of Players': percent_of_players
})

age_demographics_df
```

    <ipython-input-45-f6529bbccd9c>:6: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      new_age_groups["Age Groups"] = pd.cut(new_age_groups["Age"], age_bins, labels = group_names)





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
      <th>Total Count</th>
      <th>Percentage of Players</th>
    </tr>
    <tr>
      <th>Age Groups</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>17</td>
      <td>2.95%</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>22</td>
      <td>3.82%</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>107</td>
      <td>18.58%</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>258</td>
      <td>44.79%</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>77</td>
      <td>13.37%</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>52</td>
      <td>9.03%</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>31</td>
      <td>5.38%</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>12</td>
      <td>2.08%</td>
    </tr>
  </tbody>
</table>
</div>



## Purchasing Analysis (Age)

* Bin the purchase_data data frame by age


* Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below


* Create a summary data frame to hold the results


* Optional: give the displayed data cleaner formatting


* Display the summary data frame


```python
purchasing_analysis_df = purchase_data.loc[:, ['Purchase ID', 'SN', 'Age', 'Price']]
purchasing_analysis_df['Age Ranges'] = pd.cut(purchasing_analysis_df['Age'], age_bins, labels = group_names)
purchasing_grouped = purchasing_analysis_df.groupby('Age Ranges')

total_count_df = purchase_data.loc[:, ['SN', 'Age']]
total_count_df_new = total_count_df.drop_duplicates('SN')
total_count_df_new["Age Ranges"] = pd.cut(total_count_df_new["Age"], age_bins, labels = group_names)
purchasing_grouped_2 = total_count_df_new.groupby('Age Ranges')
purchasing_count = purchasing_grouped_2['Age Ranges'].count()

purchase_analysis_count = purchasing_grouped['Purchase ID'].count()
avg_purchase_price_analysis = purchasing_grouped['Price'].mean().map('${:.2f}'.format)
total_purchase_value_analysis = purchasing_grouped['Price'].sum().map('${:.2f}'.format)
avg_purchase_per_person = (purchasing_grouped['Price'].sum() / purchasing_count).map('${:.2f}'.format)

purchasing_analysis = pd.DataFrame({
    'Purchase Count': purchase_analysis_count,
    'Average Purchase Price': avg_purchase_price_analysis,
    'Total Purchase Value': total_purchase_value_analysis,
    'Avg Total Purchase Per Person': avg_purchase_per_person
})

purchasing_analysis
```

    <ipython-input-43-d0fd7ff0b43b>:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: https://pandas.pydata.org/pandas-docs/stable/user_guide/indexing.html#returning-a-view-versus-a-copy
      total_count_df_new["Age Ranges"] = pd.cut(total_count_df_new["Age"], age_bins, labels = group_names)





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
      <th>Avg Total Purchase Per Person</th>
    </tr>
    <tr>
      <th>Age Ranges</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>23</td>
      <td>$3.35</td>
      <td>$77.13</td>
      <td>$4.54</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>28</td>
      <td>$2.96</td>
      <td>$82.78</td>
      <td>$3.76</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>136</td>
      <td>$3.04</td>
      <td>$412.89</td>
      <td>$3.86</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>365</td>
      <td>$3.05</td>
      <td>$1114.06</td>
      <td>$4.32</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>101</td>
      <td>$2.90</td>
      <td>$293.00</td>
      <td>$3.81</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>73</td>
      <td>$2.93</td>
      <td>$214.00</td>
      <td>$4.12</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>41</td>
      <td>$3.60</td>
      <td>$147.67</td>
      <td>$4.76</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>13</td>
      <td>$2.94</td>
      <td>$38.24</td>
      <td>$3.19</td>
    </tr>
  </tbody>
</table>
</div>



## Top Spenders

* Run basic calculations to obtain the results in the table below


* Create a summary data frame to hold the results


* Sort the total purchase value column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
top_spender_data = purchase_data.groupby('SN')

spender_purchase_count = top_spender_data['Purchase ID'].count()
spender_avg_purchase_price = top_spender_data['Price'].mean()
spender_total_purchase_value = spender_purchase_count * spender_avg_purchase_price

top_spenders = pd.DataFrame({
    'Purchase Count': spender_purchase_count,
    'Average Purchase Price': spender_avg_purchase_price,
    'Total Purchase Value': spender_total_purchase_value})

sorted_top_spenders = top_spenders.sort_values('Total Purchase Value', ascending = False)

sorted_top_spenders['Average Purchase Price'] = sorted_top_spenders['Average Purchase Price'].map('${:.2f}'.format)
sorted_top_spenders['Total Purchase Value'] = sorted_top_spenders['Total Purchase Value'].map('${:.2f}'.format)

sorted_top_spenders.head()
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
      <th>Lisosia93</th>
      <td>5</td>
      <td>$3.79</td>
      <td>$18.96</td>
    </tr>
    <tr>
      <th>Idastidru52</th>
      <td>4</td>
      <td>$3.86</td>
      <td>$15.45</td>
    </tr>
    <tr>
      <th>Chamjask73</th>
      <td>3</td>
      <td>$4.61</td>
      <td>$13.83</td>
    </tr>
    <tr>
      <th>Iral74</th>
      <td>4</td>
      <td>$3.40</td>
      <td>$13.62</td>
    </tr>
    <tr>
      <th>Iskadarya95</th>
      <td>3</td>
      <td>$4.37</td>
      <td>$13.10</td>
    </tr>
  </tbody>
</table>
</div>



## Most Popular Items

* Retrieve the Item ID, Item Name, and Item Price columns


* Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value


* Create a summary data frame to hold the results


* Sort the purchase count column in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the summary data frame




```python
popular_items_df = purchase_data.loc[:, ['Item ID', 'Item Name', 'Price']]

popular_items_grouped = popular_items_df.groupby(['Item ID', 'Item Name'])

popular_item_count = popular_items_grouped['Item ID'].count()
popular_avg_purchase_price = popular_items_grouped['Price'].mean()
popular_total_purchase_value = popular_item_count * popular_avg_purchase_price

popular_items = pd.DataFrame({
    'Purchase Count': popular_item_count,
    'Average Purchase Price': popular_avg_purchase_price,
    'Total Purchase Value': popular_total_purchase_value})

sorted_popular_items = popular_items.sort_values('Purchase Count', ascending = False)

sorted_popular_items['Average Purchase Price'] = sorted_popular_items['Average Purchase Price'].map('${:.2f}'.format)
sorted_popular_items['Total Purchase Value'] = sorted_popular_items['Total Purchase Value'].map('${:.2f}'.format)

sorted_popular_items.head()
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
      <th>Average Purchase Price</th>
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>132</th>
      <th>Persuasion</th>
      <td>9</td>
      <td>$3.22</td>
      <td>$28.99</td>
    </tr>
    <tr>
      <th>108</th>
      <th>Extraction, Quickblade Of Trembling Hands</th>
      <td>9</td>
      <td>$3.53</td>
      <td>$31.77</td>
    </tr>
  </tbody>
</table>
</div>



## Most Profitable Items

* Sort the above table by total purchase value in descending order


* Optional: give the displayed data cleaner formatting


* Display a preview of the data frame




```python
popular_items_df = purchase_data.loc[:, ['Item ID', 'Item Name', 'Price']]

popular_items_grouped = popular_items_df.groupby(['Item ID', 'Item Name'])

popular_item_count = popular_items_grouped['Item ID'].count()
popular_avg_purchase_price = popular_items_grouped['Price'].mean()
popular_total_purchase_value = popular_item_count * popular_avg_purchase_price

popular_items = pd.DataFrame({
    'Purchase Count': popular_item_count,
    'Average Purchase Price': popular_avg_purchase_price,
    'Total Purchase Value': popular_total_purchase_value})

sorted_popular_items = popular_items.sort_values('Total Purchase Value', ascending = False)

sorted_popular_items['Average Purchase Price'] = sorted_popular_items['Average Purchase Price'].map('${:.2f}'.format)
sorted_popular_items['Total Purchase Value'] = sorted_popular_items['Total Purchase Value'].map('${:.2f}'.format)

sorted_popular_items.head()
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
      <th>Average Purchase Price</th>
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
      <th>92</th>
      <th>Final Critic</th>
      <td>13</td>
      <td>$4.61</td>
      <td>$59.99</td>
    </tr>
    <tr>
      <th>178</th>
      <th>Oathbreaker, Last Hope of the Breaking Storm</th>
      <td>12</td>
      <td>$4.23</td>
      <td>$50.76</td>
    </tr>
    <tr>
      <th>82</th>
      <th>Nirvana</th>
      <td>9</td>
      <td>$4.90</td>
      <td>$44.10</td>
    </tr>
    <tr>
      <th>145</th>
      <th>Fiery Glass Crusader</th>
      <td>9</td>
      <td>$4.58</td>
      <td>$41.22</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>8</td>
      <td>$4.35</td>
      <td>$34.80</td>
    </tr>
  </tbody>
</table>
</div>


