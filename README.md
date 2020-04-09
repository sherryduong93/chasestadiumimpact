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
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCallsinmbdp2019-2020.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/2019firecallsofinterest.png)


## Dataset 2: Police Incidents
### Report 1: From 2003 to 20018
### Report 2: From 2018 to Present
**Report (2003 - 2018)**
<br> 2215024 entries, 33 columns
<br>Do not have zip code or neighborhood, only address, Latitude & Longitude
<br>In general, crime seems to be flat and actually decreasing since the Chase Stadium Opened.
<br>![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Total_Police_Calls_2003-2020.png)

<br>**Report (2018 - Present)**
<br>332828 entries, 36 columns
<br> categories_of_interest = ['Non-Criminal','Stolen Property','Miscellaneous Investigation','Other Miscellaneous’, 'Assault', 'Larceny Theft', 'Malicious Mischief', 'Disorderly Conduct', ‘Other', 'Suspicious Occ', 'Disorderly Conduct','Traffic Collision', 'Liquor Laws', 'Fire Report', 'Suspicious']

<br>**Data Cleaning**
<br>Calculated the harversine distance between the latitude/longitude of all rows in data and the known latitude/longitude of all zipcodes in SF. Was able to estimate the zipcode.

<br>**Interesting Finds**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/2019_Crime_DPMS.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Normalized_Crime_DPMS.png)

## Dataset 3: Events at Chase Center Stadium Data
**Dataset was scraped from ChaseCenter.com/events in the form of a json file**
<br>92 rows, 25 columns
<br>Used pandas to cut the json file down 77 rows & 5 columns

## Hypothesis Testing
<br>**Null Hypothesis: Fire Department Calls & Police Incidents during event dates will be the same as non-event dates
<br> Alternative Hypothesis: Fire Department Calls & Police Incidents during event dates will be higher than non-event dates
<br> Alpha: 0.05**
<br> Methodology: Conducted MannWhitneyU Test as well as taking 1000 bootstrapped samples of 100 observations each from the samples of event date versus non-event date calls. In the data, there were outliers in both datasets on 12/20/2019, 1/11/2020. Removed these points from the data. 
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
<br>Originally intended to join both tables to see the trend over time, but the 2018 to present data had a significant drop off in rates that I did not have time to explore.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Normalized_Police_Calls_2003-2018.png)
