# Impact of the Chase Stadium Opening in San Francisco

**Exploring the impact of events held at the Chase Center on Crime & Fire Department notices in the Mission Bay/Dogpatch Area.**

## Datasets & Exploratory Analysis

### Dataset 1: Fire Department Calls for Service**
<br>Data: 5243306 entries, 44 columns
<br>Null Values: 14297 entries in “zipcode”: Added 600 values for zipcode 94158
<br>DtTm for specific incidents of the call are also missing, this may be interesting to look into if time permits, but not entirely necessary
<br>**Data Cleaning**
<br>Zipcodes of interest ‘94107’,'94103', ‘94158’, ‘94105’
<br>Call Types of interest: 'Marine Fire','Train / Rail Incident', 'Odor (Strange / Unknown)', 'Explosion','Traffic Collision','Alarms', 'Structure Fire', 'Other', 'Medical Incident’
<br>**New Dataframe:**
<br>956419 entries, 10 columns
<br>**Interesting Finds**
In general, the calls to the Fire Department have been increasing year over year, even when normalizing for population growth.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Fire_Calls_2000-2020.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/2019firecallsofinterest.png)
There is also some seasonality of the data, with peaks at the beginning of each season and lows mid-season. 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCalls2015-2020.png)
However, when looking into 2020, the call volume seems to have dropped, but this is also due to the fact that it is the "low" part of the seasonal volume, and we would expect to see an uptick in April based on current trend. Because of this, I will proceed with hypothesis testing only comparing dates after the stadium opened.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCallsinmbdp2019-2020.png)


### Dataset 2: Police Incidents
**Report 1 (2003 - 2018)**
<br> 2215024 entries, 33 columns
<br>Do not have zip code or neighborhood, only address, Latitude & Longitude
<br>In general, crime seems to be flat and actually decreasing since the Chase Stadium Opened.
<br>![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Total_Police_Calls_2003-2020.png)
<br>**Report 2 (2018 - Present)**
<br>332828 entries, 36 columns
<br> categories_of_interest = ['Non-Criminal','Stolen Property','Miscellaneous Investigation','Other Miscellaneous’, 'Assault', 'Larceny Theft', 'Malicious Mischief', 'Disorderly Conduct', ‘Other', 'Suspicious Occ', 'Disorderly Conduct','Traffic Collision', 'Liquor Laws', 'Fire Report', 'Suspicious']
<br>**Data Cleaning**
<br>Calculated the harversine distance between the latitude/longitude of all rows in data and the known latitude/longitude of all zipcodes in SF. Was able to estimate the zipcode.
<br>**New Dataframe:**
<br>37354 entries, 14 columns
<br>**Interesting Finds**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/2019_Crime_DPMS.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Normalized_Crime_DPMS.png)


### Dataset 3: Events at Chase Center Stadium Data
**Dataset was scraped from ChaseCenter.com/events in the form of a json file**
<br>92 rows, 25 columns
<br>Used pandas to cut the json file down 77 rows & 5 columns

<br>
## Hypothesis Testing
<br>**Null Hypothesis: Fire Department Calls & Police Incidents during event dates will be the same as non-event dates
<br> Alternative Hypothesis: Fire Department Calls & Police Incidents during event dates will be higher than non-event dates
<br> Alpha: 0.05**
<br> Methodology: Conducted MannWhitneyU Test and T-Test on both sample populations (#Calls/Incidents on Event Dates vs. #Calls/Incidents not on event dates). 
In the data, there were outliers in both datasets on 12/20/2019, 1/11/2020. Removed these points from the data. 
<br>![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonScatter_fire.png)
<br>![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonBox_Police.png)
<br><br>
### Results: Fire Department Calls
<br>MannWhitneyU Test Result : pvalue = 0.069 -> Not-Significant
<br>T-Test Statistic & Distribution: pvalue = 0.044 -> Significant 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_Fire.png)
<br><br>
### Results: Police Department Calls
MannWhitneyU Test Result : pvalue = 0.02 -> Significant
<br>T-Test Statistic & Distribution: pvalue = 0.0069 -> Significant
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_Police.png)



## Assumptions Made & Caveats....
<br>Data was normalized using annual population for SF as a whole, not drilled down to zipcode.
<br>The event dates were slightly biased with 20% of events on Saturdays as compared to 10-15% for the remaining days. Would like to look into this in the future

## If Time Permits...
<br>Would be interesting to look into time of day to see trends in crime/fire, as well as further look into Day Of Week.
<br>What exactly happened on 12/21/2019 & 1/11/2020 that drove both crime & fire so high? Would be fascinating to look into it.
<br>2018 Also had some extreme uptick in the summer, I would be curious to see what drove it.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/2018allfirecalls.png)
