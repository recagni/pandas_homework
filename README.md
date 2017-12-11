

```python
                   # INITIAL IMPORTS AND UPLOADS

import pandas as pd
import os
```


```python
# The path to our json file
# Read data into pandas
df1=pd.read_json("purchase_data.json")
# df1.head (10)
```


```python
                    # HEROES OF PYMOLI DATA ANALYSIS
    
# Although the second data sample contains much fewer data points than the first one, most conclusions are similar.
# For both samples, about 80% of the players are male and the average item is priced at around $3.
# The larger sample indicates that the average spend per capita is about $4, vs. 3 of the second one. 
# Either way, this means that this population is a very long tail on one-item purchasers.
# Even those players who purchased more than one items, didn't purchase more than ten.
# Thus, for this particular population it is more important to address the needs of the many, as opposed to those 
# of the few top players.
# Both files show that most of the revenue (improperly called profitability in the homework instructions) comes
# from the 11 to 20 year old players, and particularly those aged between 15 and 20 years.
# However, the larger sample indicates that all the age ranges up to 35 years should be addressed, as relevant sales
# are made also in these ranges.
# Similarly to the distribution of player purchases, the one for single item sales is also very much flat, with
# the top five items, out of all the almost 200 sold, representing only 3% of total sales.
# So, for items as well, the emphasis should be on improving all products, as opposed to try to perfect a few.
```


```python
                    # CHECK FOR POSSIBLE DATA CLEANUP

# Checking whether the dataframe needs to be cleaned and apparently in this case we don't need to
# While testing, the two lines below should be commented in to conduct the test
# print("Table below checks that the numbers of rows without NaN or null values is the same for all columns.")
# df1.count()
```


```python
                    # PLAYER COUNT

# Total Number of Players
Players=len(df1["SN"].value_counts())

print("PLAYER COUNT")
print("The total number of players was " + str(Players) + ".")
```

    PLAYER COUNT
    The total number of players was 573.
    


```python
                    # PURCHASING ANALYSIS (TOTAL)

# Number of Unique Items
uniqueItems=len(df1["Item ID"].value_counts())

print("----------------------------------------------------------------------")
print("PURCHASING ANALYSIS (TOTAL)")
print("The number of unique items was " + str(uniqueItems) + ".")
```

    ----------------------------------------------------------------------
    PURCHASING ANALYSIS (TOTAL)
    The number of unique items was 183.
    


```python
# Average Purchase Price
avgPrice=round(df1["Price"].mean(),2)
print("The average purchase price was $" + str(avgPrice) + ".")
```

    The average purchase price was $2.93.
    


```python
# Total Number of Purchases
# The count is made on screen names (SN) because they are not unique
Transactions=df1["SN"].count()
print("The total number of purchases was " + str(Transactions) + ".")
```

    The total number of purchases was 780.
    


```python
# Total Revenue
Revenue=round(df1["Price"].sum(),2)
print("The total revenue was $" + str(Revenue) + ".")
```

    The total revenue was $2286.33.
    


```python
                    # GENDER DEMOGRAPHICS

# First we need to drop player duplicates, by removing duplications from the "SN" column of the dataframe.
# "Last" in the formula means that we are keeping only the last element of those duplicated in that column.
player_df = df1.drop_duplicates(['SN'], keep ='last')

# Determine the number of male, female, and other players
malePlayer_df=player_df.loc[player_df["Gender"]=="Male"]
malePlayers=len(malePlayer_df)
femalePlayer_df=player_df.loc[player_df["Gender"]=="Female"]
femalePlayers=len(femalePlayer_df)
otherPlayer_df=player_df.loc[player_df["Gender"]=="Other / Non-Disclosed"]
otherPlayers=len(otherPlayer_df)

print("----------------------------------------------------------------------")
print("GENDER DEMOGRAPHICS")
print("There were",malePlayers,"male,",femalePlayers,"female, and",otherPlayers,"other (non-disclosed) players.")
print("They were ",str(100*round(malePlayers/Players,4)),"%,",str(100*round(femalePlayers/Players,4)),"%, and",
      str(100*round(otherPlayers/Players,4)),"% of total players, respectively.")
```

    ----------------------------------------------------------------------
    GENDER DEMOGRAPHICS
    There were 465 male, 100 female, and 8 other (non-disclosed) players.
    They were  81.15 %, 17.45 %, and 1.4000000000000001 % of total players, respectively.
    


```python
                    # PURCHASING ANALYSIS (GENDER)

# Purchase Count by Gender
# Needs to separate the dataframe across genders
# The obtained object can be visualized only by counting or adding, etc. its elements.
# Count how many purchases have occured within each gender

# Male purchases first
male_df=df1.loc[df1["Gender"]=="Male"]
malePurchCount=male_df["Item ID"].count()

print("----------------------------------------------------------------------")
print("PURCHASING ANALYSIS (GENDER)")
print("The total purchases made by male players were " + str(malePurchCount) + ".")
```

    ----------------------------------------------------------------------
    PURCHASING ANALYSIS (GENDER)
    The total purchases made by male players were 633.
    


```python
# Female purchases
female_df=df1.loc[df1["Gender"]=="Female"]
femalePurchCount=female_df["Item ID"].count()
print("The total purchases made by female players were " + str(femalePurchCount) + ".")
```

    The total purchases made by female players were 136.
    


```python
# Other purchases
other_df=df1.loc[df1["Gender"]=="Other / Non-Disclosed"]
otherPurchCount=other_df["Item ID"].count()
print("The total purchases made by other players were " + str(otherPurchCount) + ".")
```

    The total purchases made by other players were 11.
    


```python
# Average Male Purchase Price by Gender
male_df=df1.loc[df1["Gender"]=="Male"]
maleAvgPrice=round(male_df["Price"].mean(),2)
print("The average purchase price for male customers was $" + str(maleAvgPrice) + ".")
```

    The average purchase price for male customers was $2.95.
    


```python
# Average Female Purchase Price by Gender
female_df=df1.loc[df1["Gender"]=="Female"]
femaleAvgPrice=round(female_df["Price"].mean(),2)
print("The average purchase price for female customers was $" + str(femaleAvgPrice) + ".")
```

    The average purchase price for female customers was $2.82.
    


```python
# Average Other Purchase Price by Gender
other_df=df1.loc[df1["Gender"]=="Other / Non-Disclosed"]
otherAvgPrice=round(other_df["Price"].mean(),2)
print("The average purchase price for other customers was $" + str(otherAvgPrice) + ".")
```

    The average purchase price for other customers was $3.25.
    


```python
# Total Purchase Value by Gender

# For males
maleTotPrice=round(male_df["Price"].sum(),2)
print("The total value purchased by male customers was $" + str(maleTotPrice) + ".")
```

    The total value purchased by male customers was $1867.68.
    


```python
# For females
femaleTotPrice=round(female_df["Price"].sum(),2)
print("The total value purchased by female customers was $" + str(femaleTotPrice) + ".")
```

    The total value purchased by female customers was $382.91.
    


```python
# For others
otherTotPrice=round(other_df["Price"].sum(),2)
print("The total value purchased by other customers was $" + str(otherTotPrice) + ".")
```

    The total value purchased by other customers was $35.74.
    


```python
# Normalized Totals (normalizing for the number of people in each gender group)
# Use normalized dataframe "player_df", created before

# Male Normalized Totals
malePlayer_df=player_df.loc[player_df["Gender"]=="Male"]
malePlayers=len(malePlayer_df)
normMaleTotPrice=round(maleTotPrice/malePlayers,2)
print("The average male players purchased a total of $" + str(normMaleTotPrice) + ".")
```

    The average male players purchased a total of $4.02.
    


```python
# Female Normalized Totals

femalePlayer_df=player_df.loc[player_df["Gender"]=="Female"]
femalePlayers=len(femalePlayer_df)
normFemaleTotPrice=round(femaleTotPrice/femalePlayers,2)
normFemaleTotPrice
print("The average female players purchased a total of $" + str(normFemaleTotPrice) + ".")
```

    The average female players purchased a total of $3.83.
    


```python
# Female Normalized Totals
otherPlayer_df=player_df.loc[player_df["Gender"]=="Other / Non-Disclosed"]
otherPlayers=len(otherPlayer_df)
normOtherTotPrice=round(otherTotPrice/otherPlayers,2)
print("The average other players purchased a total of $" + str(normOtherTotPrice) + ".")
```

    The average other players purchased a total of $4.47.
    


```python
                    # AGE DEMOGRAPHICS

# The items below each broken into bins of 4 years (i.e. <10, 10-14, 15-19, etc.)

# Purchase Count
# Average Purchase Price
# Total Purchase Value
# Normalized Totals (normalizing for the # of people in each age group)

# First, evaluate appropriate bins
minAge=df1["Age"].min()

print("----------------------------------------------------------------------")
print("AGE DEMOGRAPHICS")
print("The youngest player was ",str(minAge)," year old.")
```

    ----------------------------------------------------------------------
    AGE DEMOGRAPHICS
    The youngest player was  7  year old.
    


```python
maxAge=df1["Age"].max()
print("The oldest player was ",str(maxAge)," year old.")
```

    The oldest player was  45  year old.
    


```python
# So, perhaps the bins can be set like this:
bins=[10,15,20,25,30,35,40,120]
group_labels=["10 and younger", "11 to 15", "16 to 20", "21 to 25", "30 to 35", "35 to 40", "41 and older"]

# Add "Age Range" column to the both dataframes (with and without duplications)
df1["Age Range"]=pd.cut(df1["Age"],bins,labels=group_labels)
player_df["Age Range"]=pd.cut(df1["Age"],bins,labels=group_labels)
```

    C:\Users\mrecagni\AppData\Local\Continuum\anaconda\lib\site-packages\ipykernel_launcher.py:7: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      import sys
    


```python
# Group the dataframe df1 (the one with duplicated players) by the the values in its "Age Range" column
obj1=df1.groupby("Age Range")

# Convert the groupby object into a dataframe

# Purchase Count by Age Range
# The count is made on screen names (SN) because they are not unique
count_in_obj1=obj1["SN"].count()
count_table=pd.DataFrame({"Purchase Count":count_in_obj1})

# Average Price by Age Range
avgPrice_in_obj1=round(obj1["Price"].mean(),2)
avgPrice_table=pd.DataFrame({"Average Price ($)":avgPrice_in_obj1})

# Purchase Value by Age Range
totPrice_in_obj1=obj1["Price"].sum()
totPrice_table=pd.DataFrame({"Purchase Value ($)":totPrice_in_obj1})

# Merge the three tables (each with only one column, besides the common Age Range column) into one
age_df=pd.merge(count_table,avgPrice_table,left_index=True,right_index=True).merge(totPrice_table,left_index=True,
            right_index=True)
```


```python
# Group the dataframe player_df (the one without duplicated players) by the the values in its "Age Range" column
objAge=player_df.groupby("Age Range")

# Convert the non-duplicated groupby object into a dataframe

# Purchase Count by Age Range and Player
count_in_objAge=objAge["Age"].count()
player_count_table=pd.DataFrame({"Purchase Count":count_in_objAge})

# Purchase Value by Age Range and Player
totPrice_in_objAge=objAge["Price"].sum()
player_totPrice_table=pd.DataFrame({"Purchase Value ($)":totPrice_in_objAge})

# Add Normalized Value by Age Range to the table that contains non-normalized information
age_df["Normalized Price ($)"]=round((player_totPrice_table["Purchase Value ($)"]/
                                             player_count_table["Purchase Count"]),2)
age_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Price ($)</th>
      <th>Purchase Value ($)</th>
      <th>Normalized Price ($)</th>
    </tr>
    <tr>
      <th>Age Range</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10 and younger</th>
      <td>78</td>
      <td>2.87</td>
      <td>224.15</td>
      <td>2.84</td>
    </tr>
    <tr>
      <th>11 to 15</th>
      <td>184</td>
      <td>2.87</td>
      <td>528.74</td>
      <td>2.86</td>
    </tr>
    <tr>
      <th>16 to 20</th>
      <td>305</td>
      <td>2.96</td>
      <td>902.61</td>
      <td>2.90</td>
    </tr>
    <tr>
      <th>21 to 25</th>
      <td>76</td>
      <td>2.89</td>
      <td>219.82</td>
      <td>2.90</td>
    </tr>
    <tr>
      <th>30 to 35</th>
      <td>58</td>
      <td>3.07</td>
      <td>178.26</td>
      <td>2.99</td>
    </tr>
    <tr>
      <th>35 to 40</th>
      <td>44</td>
      <td>2.90</td>
      <td>127.49</td>
      <td>2.74</td>
    </tr>
    <tr>
      <th>41 and older</th>
      <td>3</td>
      <td>2.88</td>
      <td>8.64</td>
      <td>2.88</td>
    </tr>
  </tbody>
</table>
</div>




```python
                      # TOP SPENDERS
    
# Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
    # SN
    # Purchase Count
    # Average Purchase Price
    # Total Purchase Value
    
# Group the dataframe player_df (the one with duplicated players, because we need to sum all of the values
# for each player) by the the values in its "SN" (screen name) column
objSN=df1.groupby("SN")

# Convert the non-duplicated groupby object into a dataframe

# Purchase Count by Player
count_in_objSN=objSN["Age"].count()
SN_count_table=pd.DataFrame({"Purchase Count":count_in_objSN})

# Average Price by Player
avgPrice_in_objSN=round(objSN["Price"].mean(),2)
SN_avgPrice_table=pd.DataFrame({"Average Price ($)":avgPrice_in_objSN})

# Purchase Price by Player
totPrice_in_objSN=objSN["Price"].sum()
SN_totPrice_table=pd.DataFrame({"Total Purchase Value ($)":totPrice_in_objSN})
                            
# Merge the three tables (each with only one column, besides the common SN column) into one
SN_df=pd.merge(SN_count_table,SN_avgPrice_table,left_index=True,right_index=True).merge(
    SN_totPrice_table,left_index=True,right_index=True)

# Sort the just created table by the total purchases made by the players, then show only the top five

sorted_SN=SN_df.sort_values(by="Total Purchase Value ($)",ascending=False)

print("----------------------------------------------------------------------")
print("TOP SPENDERS")
sorted_SN.head()
```

    ----------------------------------------------------------------------
    TOP SPENDERS
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Price ($)</th>
      <th>Total Purchase Value ($)</th>
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
      <td>5</td>
      <td>3.41</td>
      <td>17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>4</td>
      <td>3.39</td>
      <td>13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>4</td>
      <td>3.18</td>
      <td>12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>3</td>
      <td>4.24</td>
      <td>12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>3</td>
      <td>3.86</td>
      <td>11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
                        # MOST POPULAR ITEMS
    
# Identify the 5 most popular items by purchase count, then list (in a table):
    # Item ID
    # Item Name
    # Purchase Count
    # Item Price
    # Total Purchase Value
    
# Group the dataframe player_df (the one with duplicated players, because we are focused on the items, not on 
# the players) by the the values in its "Item ID" column
objID=df1.groupby("Item ID")

# Convert the groupby object into a dataframe

# Purchase Count by Item
count_in_objID=objID["Item ID"].count()
ID_count_table=pd.DataFrame({"Purchase Count":count_in_objID})

# Average Price by Item
avgPrice_in_objID=round(objID["Price"].mean(),2)
ID_avgPrice_table=pd.DataFrame({"Item Price ($)":avgPrice_in_objID})

# Purchase Price by Item
totPrice_in_objID=objID["Price"].sum()
ID_totPrice_table=pd.DataFrame({"Item Total Sales ($)":totPrice_in_objID})

# Now we need to drop ID item duplicates, by removing duplications from the "Item ID" column of the dataframe.
# "Last" in the formula means that we are keeping only the last element of those duplicated in that column.
singleID_df = df1.drop_duplicates(['Item ID'], keep ='last')

# From the dataframe just created, derive a dataframe containing only the columns "Item ID" and "Item Name"
reduced_singleID_df=singleID_df.loc[:,["Item ID","Item Name"]]

# Merge the four columns (Item ID, Item Name, Purchase Count, Item Price, and Item Total Sales) into one table
ID_df=pd.merge(reduced_singleID_df,ID_count_table,left_index=True,right_index=True).merge(ID_avgPrice_table,
               left_index=True,right_index=True).merge(ID_totPrice_table,left_index=True,right_index=True)

# Make the Item ID column the index in a newly created dataframe
indexed_ID_df=ID_df.set_index("Item ID")

# Sort this final dataframe by the most popular items by the players, then show only the top five
sorted_ID=indexed_ID_df.sort_values(by="Purchase Count",ascending=False)

print("----------------------------------------------------------------------")
print("MOST POPULAR ITEMS")
sorted_ID.head()
```

    ----------------------------------------------------------------------
    MOST POPULAR ITEMS
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price ($)</th>
      <th>Item Total Sales ($)</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Splinter</td>
      <td>5</td>
      <td>4.83</td>
      <td>24.15</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Flux, Destroyer of Due Diligence</td>
      <td>4</td>
      <td>1.27</td>
      <td>5.08</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Exiled Mithril Longsword</td>
      <td>4</td>
      <td>1.55</td>
      <td>6.20</td>
    </tr>
    <tr>
      <th>155</th>
      <td>War-Forged Gold Deflector</td>
      <td>4</td>
      <td>4.89</td>
      <td>19.56</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lightning, Etcher of the King</td>
      <td>3</td>
      <td>3.47</td>
      <td>10.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
                        # MOST PROFITABLE ITEMS
    
# Identify the 5 most popular items by purchase value, then list (in a table):
    # Item ID
    # Item Name
    # Purchase Count
    # Item Price
    # Total Purchase Value

# Sort the final dataframe of the previous section by the total purchases made by the players, then show only the 
# top five 
sorted_ID=indexed_ID_df.sort_values(by="Item Total Sales ($)",ascending=False)

print("----------------------------------------------------------------------")
print("MOST PROFITABLE ITEMS")
sorted_ID.head()
```

    ----------------------------------------------------------------------
    MOST PROFITABLE ITEMS
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item Name</th>
      <th>Purchase Count</th>
      <th>Item Price ($)</th>
      <th>Item Total Sales ($)</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Splinter</td>
      <td>5</td>
      <td>4.83</td>
      <td>24.15</td>
    </tr>
    <tr>
      <th>155</th>
      <td>War-Forged Gold Deflector</td>
      <td>4</td>
      <td>4.89</td>
      <td>19.56</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Lightning, Etcher of the King</td>
      <td>3</td>
      <td>3.47</td>
      <td>10.41</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Phantomlight</td>
      <td>3</td>
      <td>3.27</td>
      <td>9.81</td>
    </tr>
    <tr>
      <th>132</th>
      <td>Persuasion</td>
      <td>2</td>
      <td>4.10</td>
      <td>8.20</td>
    </tr>
  </tbody>
</table>
</div>


