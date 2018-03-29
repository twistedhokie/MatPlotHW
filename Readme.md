

```python
#Import dependencies
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import csv
```


```python
#Read CSVs
ride_data = "ride_data.csv"
city_data = "city_data.csv"

ride_df = pd.read_csv(ride_data)
ride_df.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Test to make sure they read in correctly
city_df = pd.read_csv(city_data)
city_df.head()
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
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Merge data and test output again
all_data = pd.merge(ride_df, city_df, how="left", on=["city", "city"])
all_data.head()
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
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sarabury</td>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>South Roy</td>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
      <td>35</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Wiseborough</td>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
      <td>55</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Spencertown</td>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
      <td>68</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nguyenbury</td>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Create Dataframes for each city type for analysis
urban_df = all_data[all_data["type"]=="Urban"]
suburban_df = all_data[all_data["type"]=="Suburban"]
rural_df = all_data[all_data["type"]=="Rural"]
```


```python
#Calculate stats for Urban data
urban_drivers = urban_df.groupby(["city"])["driver_count"].mean()
urban_rides = urban_df.groupby(["city"]).count()["ride_id"]
urban_avg = urban_df.groupby(["city"]).mean()["fare"]

urban_stats = pd.DataFrame({"Average Fare ($)": urban_avg,
                            "Total Rides": urban_rides,
                            "Total Number of Drivers": urban_drivers})

urban_stats ["City Type"] = "Urban"
urban_stats.head()                                 
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
      <th>Average Fare ($)</th>
      <th>Total Number of Drivers</th>
      <th>Total Rides</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.928710</td>
      <td>21</td>
      <td>31</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>67</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>21</td>
      <td>22</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>49</td>
      <td>19</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Arnoldview</th>
      <td>25.106452</td>
      <td>41</td>
      <td>31</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculate stats for Suburban data
suburban_drivers = suburban_df.groupby(["city"])["driver_count"].mean()
suburban_rides = suburban_df.groupby(["city"]).count()["ride_id"]
suburban_avg = suburban_df.groupby(["city"]).mean()["fare"]

suburban_stats = pd.DataFrame({"Average Fare ($)": suburban_avg,
                            "Total Rides": suburban_rides,
                            "Total Number of Drivers": suburban_drivers})

suburban_stats ["City Type"] = "Suburban"
suburban_stats.head()
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
      <th>Average Fare ($)</th>
      <th>Total Number of Drivers</th>
      <th>Total Rides</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Anitamouth</th>
      <td>37.315556</td>
      <td>16</td>
      <td>9</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Campbellport</th>
      <td>33.711333</td>
      <td>26</td>
      <td>15</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Carrollbury</th>
      <td>36.606000</td>
      <td>4</td>
      <td>10</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Clarkstad</th>
      <td>31.051667</td>
      <td>21</td>
      <td>12</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Conwaymouth</th>
      <td>34.591818</td>
      <td>18</td>
      <td>11</td>
      <td>Suburban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculate stats for Rural data
rural_drivers = rural_df.groupby(["city"])["driver_count"].mean()
rural_rides = rural_df.groupby(["city"]).count()["ride_id"]
rural_avg = rural_df.groupby(["city"]).mean()["fare"]

rural_stats = pd.DataFrame({"Average Fare ($)": rural_avg,
                            "Total Rides": rural_rides,
                            "Total Number of Drivers": rural_drivers})

rural_stats ["City Type"] = "Rural"
rural_stats.head()
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
      <th>Average Fare ($)</th>
      <th>Total Number of Drivers</th>
      <th>Total Rides</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>East Leslie</th>
      <td>33.660909</td>
      <td>9</td>
      <td>11</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>East Stephen</th>
      <td>39.053000</td>
      <td>6</td>
      <td>10</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>East Troybury</th>
      <td>33.244286</td>
      <td>3</td>
      <td>7</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Erikport</th>
      <td>30.043750</td>
      <td>3</td>
      <td>8</td>
      <td>Rural</td>
    </tr>
    <tr>
      <th>Hernandezshire</th>
      <td>32.002222</td>
      <td>10</td>
      <td>9</td>
      <td>Rural</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Bring everything together for use in bubble plot
total_stats = urban_stats
total_stats = total_stats.append(suburban_stats) 
total_stats = total_stats.append(rural_stats) 
total_stats.head()
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
      <th>Average Fare ($)</th>
      <th>Total Number of Drivers</th>
      <th>Total Rides</th>
      <th>City Type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.928710</td>
      <td>21</td>
      <td>31</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>67</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>21</td>
      <td>22</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>49</td>
      <td>19</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Arnoldview</th>
      <td>25.106452</td>
      <td>41</td>
      <td>31</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Calculate percentages of drivers in each city type
total_urban_drivers = urban_df["driver_count"].sum()
total_suburban_drivers = suburban_df["driver_count"].sum()
total_rural_drivers = rural_df["driver_count"].sum()

total_drivers = total_stats["Total Number of Drivers"].sum()

urban_driver_percentage = round(((total_urban_drivers/total_drivers)* 100),1)
suburban_driver_percentage = round(((total_suburban_drivers/total_drivers)* 100),1)
rural_driver_percentage = round(((total_rural_drivers/total_drivers)* 100),1)
```


```python
# Create bubble plot using seaborn
sns.set()

main_plot = urban_stats.plot(kind='scatter', x= 'Total Rides', 
                         y= "Average Fare ($)", color = "lightcoral",edgecolors="black", 
                         grid=True,  figsize=(20,10), s=total_stats["Total Number of Drivers"]*15,  
                         legend = True, label = "Urban")
                        

suburban_stats.plot(kind='scatter', x= 'Total Rides', 
                         y= "Average Fare ($)", color = "lightskyblue",edgecolors="black", 
                         grid=True, figsize=(20,10),  s=total_stats["Total Number of Drivers"]*15,  
                         legend = True, label = "Suburban", ax = main_plot)
                        

rural_stats.plot(kind='scatter', x= 'Total Rides', 
                         y= "Average Fare ($)", color = "gold",edgecolors="black", 
                         grid=True, figsize=(20,10),  s=total_stats["Total Number of Drivers"]*15,  
                         legend = True, label = "Rural", ax = main_plot)


plt.xlim(0,35)
plt.xlabel('Total Number of Rides Per City', fontsize = 15)
plt.ylabel("Average Fare ($) Per City", fontsize = 15)
plt.title("Pyber Ride Share Stats", fontsize = 20)

plt.annotate(s='Circle size correlates with driver count per city',xy= (0,30), xytext=(20,45), fontsize = 18)

plt.show()
```


![png](output_10_0.png)



```python
#Make calculations for fare percentage pie chart
urban_total_fare = urban_df["fare"].sum()
suburban_total_fare = suburban_df["fare"].sum()
rural_total_fare = rural_df["fare"].sum()
agg_fare = all_data["fare"].sum()


urban_percentage_fare = round(((urban_total_fare/agg_fare)*100),1)
suburban_percentage_fare = round(((suburban_total_fare/agg_fare)*100),1)
rural_percentage_fare = round(((rural_total_fare/agg_fare)*100),1)
```


```python
# Create pie chart for Percentage of Total Fares by City Type

labels = ["Urban", "Suburban", "Rural"]

sizes = [urban_percentage_fare, suburban_percentage_fare, rural_percentage_fare]

colors = ["lightcoral", "lightskyblue", "gold"]

#set which piece to explode
explode = (0, 0.1, 0)

plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=210)

plt.title("Percentage of Total Fares by City Type")

plt.axis("equal")

plt.show()
```


![png](output_12_0.png)



```python
#make calculations for ride percentage pie chart
urban_rides = urban_df["ride_id"].count()
suburban_rides = suburban_df["ride_id"].count()
rural_rides = rural_df["ride_id"].count()

agg_rides = all_data["ride_id"].count()

urban_ride_percentage = round(((urban_rides/agg_rides)*100))
suburban_ride_percentage = round(((suburban_rides/agg_rides)*100))
rural_ride_percentage = round(((rural_rides/agg_rides)*100))
```


```python
# Create pie chart for Percentage of Total Rides by City Type

labels = ["Urban", "Suburban", "Rural"]

sizes = [urban_ride_percentage, suburban_ride_percentage, rural_ride_percentage]

colors = ["lightcoral", "lightskyblue", "gold"]

#set which piece to explode
explode = (0, 0.1, 0)


plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=190)


plt.title("Percentage of Total Rides by City Type")


plt.axis("equal")

plt.show()
```


![png](output_14_0.png)



```python
#make calculations for driver percentage pie chart
total_urban_drivers = urban_df["driver_count"].sum()
total_suburban_drivers = suburban_df["driver_count"].sum()
total_rural_drivers = rural_df["driver_count"].sum()

total_drivers = total_stats["Total Number of Drivers"].sum()

urban_driver_percentage = round(((total_urban_drivers/total_drivers)* 100),1)
suburban_driver_percentage = round(((total_suburban_drivers/total_drivers)* 100),1)
rural_driver_percentage = round(((total_rural_drivers/total_drivers)* 100),1)
```


```python
# Create pie chart for Percentage of Total Drivers by City Type

labels = ["Urban", "Suburban", "Rural"]

sizes = [urban_driver_percentage, suburban_driver_percentage, rural_driver_percentage]

colors = ["lightcoral", "lightskyblue", "gold"]

# Set which section(s) to explode out
explode = (0, 0.2, 0)

plt.pie(sizes, explode=explode, labels=labels, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=170)

plt.title("Percentage of Total Drivers by City Type")

plt.axis("equal")

plt.show()
```


![png](output_16_0.png)


Observations:

1 - There are 86 times more pyber drivers in urban areas than there are in rural areas

2 - Despite only having 1% of the drivers, the rural pyber fares make up 6% of the fares, indicating that the ride times and/or availability or drivers in rural areas have an effect on fare amount

3 - Overall, assuming that population increases by city type (rural > suburban > urban), average fare decreases as ride frequency increases
