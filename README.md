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
There is also some seasonality of the data, with peaks at the beginning of each season and lows mid-season. 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCalls2015-2020.png)
However, when looking into 2020, the call volume seems to have dropped, but this is also due to the fact that it is the "low" part of the seasonal volume, and we would expect to see an uptick in April based on current trend. Because of this, I will proceed with hypothesis testing only comparing dates after the stadium opened.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCallsinmbdp2019-2020.png)


### Dataset 2: Police Incidents
**Report 1 (2003 - 2018)**
<br> 2215024 entries, 33 columns
<br>Do not have zip code or neighborhood, only address, Latitude & Longitude

<br>**Report 2 (2018 - Present)**
<br>332828 entries, 36 columns
<br> categories_of_interest = ['Non-Criminal','Stolen Property','Miscellaneous Investigation','Other Miscellaneous’, 'Assault', 'Larceny Theft', 'Malicious Mischief', 'Disorderly Conduct', ‘Other', 'Suspicious Occ', 'Disorderly Conduct','Traffic Collision', 'Liquor Laws', 'Fire Report', 'Suspicious']
<br>**Data Cleaning**
<br>Calculated the harversine distance between the latitude/longitude of all rows in data and the known latitude/longitude of all zipcodes in SF. Was able to estimate the zipcode in order to narrow down to the same zipcodes used for the Fire Department Analysis.
<br>**New Dataframe:**
<br>37354 entries, 14 columns
<br>**General EDA on Police Incidents as a whole**
<br>In general, crime experienced an extreme upswing since 2011, but has been relatively flat since 2013 and actually decreasing since the Chase Stadium Opened. Similar seasonality seen with Fire Department Calls, though it general with normalizing it looks like crime went down.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Total_Police_Calls_2003-2020.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Incidents2003-2019.png)
<br>**Dogpatch/Mission Bay Data from before and after Stadium opening**
<br>Similar to the Fire Service Call Data, when looking into 2020, the volume seems to have dropped, but unlike the Fire Service Call data, there is not an obvious uptick in the winter, and instead is a gradual dropping of crime.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/policeincidentmbdp2019-2020.png)




### Dataset 3: Events at Chase Center Stadium Data
**Dataset was scraped from ChaseCenter.com/events in the form of a json file**
<br>Data: 92 rows, 25 columns
<br>Contained the dates & names for each event held at the Chase Stadium since opening.
<br>Used pandas to cut the json file down to 77 rows & 5 columns of relevant information.


## Hypothesis Testing
<br>**Null Hypothesis: Fire Department Calls & Police Incidents during event dates will be the same as non-event dates
<br> Alternative Hypothesis: Fire Department Calls & Police Incidents during event dates will be higher than non-event dates
<br> Alpha: 0.05**
<br> Methodology: Conducted MannWhitneyU Test and T-Test on both sample populations (#Calls/Incidents on Event Dates vs. #Calls/Incidents not on event dates). 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/firecallsdistribution.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/policeincidentdistribution.png)
<br><br>Distribution of Fire Service Calls & Police Incidents were roughly normally distributed, but indicated some outliers.
Below are the distributions prior to removing the outliers. 2 extreme outliers on 12/21/2019 & 1/11/2020 were removed from the datasets before additional analysis.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonScatter_Police.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonBox_fire.png)
<br><br>
### Results: Fire Department Calls
<br>**Number of Event Dates: 75 Number of Non-Event Dates: 134
<br>Incidents on Event Dates: 15761, Incidents not on Event Dates: 24776**
<br>MannWhitneyU Test Result : pvalue = 0.069 -> Not-Significant
<br>T-Test Statistic & Distribution: pvalue = 0.044 -> Significant 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_Fire.png)
<br><br>
### Results: Police Department Calls
<br>**Number of Event Dates: 75 Number of Non-Event Dates: 138
<br>Incidents on Event Dates: 3337, Incidents not on Event Dates: 5483**
<br>MannWhitneyU Test Result : pvalue = 0.02 -> Significant
<br>T-Test Statistic & Distribution: pvalue = 0.0069 -> Significant
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_Police.png)
<br> 
### What about effects of Shelter In Place?
**The data was captured from 9/6/2019 - 3/31/2020. Major Tech companies started having employees work from home around early March, and official Shelter In Place measures were enacted 3/19/2020. How did that impact calls/incidents?**
<br>MannWhitneyU Test Result (Fire Service): pvalue = 0.15 -> Not Significant
<br>MannWhitneyU Test Result (Police Incidents): pvalue = 0.13 -> Not Significant
<br>T-Test Statistic & Distribution (Fire Service) : pvalue = 0.11 -> Not Significant 
<br>T-Test Statistic & Distribution (Police Incidents) : pvalue = 0.098 -> Not Significant 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_FireWOSIP.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_PoliceWOSIP.png)

## Bonus: Basketball, or Concerts - Which causes more trouble?
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/firecallsbballconcertdist.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/policesbballconcertdist.png)
<br>MannWhitneyU Test Result (Fire Service): pvalue = 0.078 -> Not Significant
<br>MannWhitneyU Test Result (Police Incidents): pvalue = 0.18 -> Not Significant
<br>T-Test Statistic & Distribution (Fire Service) : pvalue = 0.10 -> Not Significant 
<br>T-Test Statistic & Distribution (Police Incidents) : pvalue = 0.26 -> Not Significant 




## Assumptions Made & Caveats....
<br>Data was normalized using annual population for SF as a whole, not drilled down to zipcode.
<br> **Looked into the week days of events versus non events in case the distribution of events was majority on weekends, which I hypothesize would have higher incidents/fire calls in general. Events were biased towards Saturdays with 25% of Events on Saturdays.**
<br>Day of Week
<br>Monday: 0.13
<br>Tuesday: 0.12
<br>Wednesday: 0.13
<br>Thursday: 0.16
<br>Friday: 0.12
<br>Saturday: 0.26
<br>Sunday: 0.09

## Maybe One Day.....
<br>Look into Saturdays in particular to compare events versus non-events.
<br>In terms of incidents or calls overall, would be interesting to look into:
<br>-Time of day or day of week
<br>-What exactly happened on 12/21/2019 & 1/11/2020 that drove both crime & fire so high? 
<br>-Why was crime so low in 2010-2012?
<br>-General trends prior to 2018 with the 2003 - 2018 report


