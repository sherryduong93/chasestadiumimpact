# Impact of the Chase Stadium Opening in San Francisco

**Exploring the impact of events held at the Chase Center on Crime & Fire Department notices in the Mission Bay/Dogpatch Area.**

## Datasets & Exploratory Analysis

## Dataset 1: Fire Department Calls for Service**
<br>Data: 5243306 entries, 44 columns
<br>Null Values: 14297 entries in “zipcode”: Added 600 values for zipcode 94158
<br>DtTm for specific incidents of the call are also missing, this may be interesting to look into if time permits, but not entirely necessary

<br>**Data Cleaning**
<br>Zipcodes of interest ‘94107’,'94103', ‘94158’, ‘94105’
<br>Call Types of interest: 'Marine Fire','Train / Rail Incident', 'Odor (Strange / Unknown)', 'Explosion','Traffic Collision','Alarms', 'Structure Fire', 'Other', 'Medical Incident’

<br>**New Dataframe:**
<br>835599 entries, 7 columns

<br>**Interesting Finds**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Fire_Calls_2000-2020.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/2019firecallsofinterest.png)


## Dataset 2: Police Incidents
### Report 1: From 2003 to 20018
### Report 2: From 2018 to Present
**Report (2003 - 2018)**
<br> 2215024 entries, 33 columns
<br>Do not have zip code or neighborhood, only address, Latitude & Longitude

<br>**Report (2018 - Present)**
<br>332828 entries, 36 columns
<br> categories_of_interest = ['Non-Criminal','Stolen Property','Miscellaneous Investigation','Other Miscellaneous’, 'Assault', 'Larceny Theft', 'Malicious Mischief', 'Disorderly Conduct', ‘Other', 'Suspicious Occ', 'Disorderly Conduct','Traffic Collision', 'Liquor Laws', 'Fire Report', 'Suspicious']

<br>**Data Cleaning**
<br>Calculated the harversine distance between the latitude/longitude of all rows in data and the known latitude/longitude of all zipcodes in SF. Was able to estimate the zipcode.

<br>**Interesting Finds**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Normalized_Crime_DPMS.png)



## Assumptions Made....
<br>Data was normalized using annual population for SF as a whole, not drilled down to zipcode.

## If Time Permits...
<br>Would be interesting to look into time of day to see trends in crime/fire, as well as Day Of Week.
<br>What exactly happened on 12/21/2019 & 1/11/2020 that drove both crime & fire so high? Would be fascinating to look into it.
<br>Originally intended to join both tables to see the trend over time, but the 2018 to present data had a significant drop off in rates that I did not have time to explore.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Normalized_Police_Calls_2003-2018.png)
