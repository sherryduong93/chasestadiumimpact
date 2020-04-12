# Impact of the Chase Stadium on Mission Bay/Dogpatch Crime Incidents and Fire Department Call Volume
![image](https://i.insider.com/5c9cf8cfee52ef3be3791303?width=1100&format=jpeg)

## Are Sports Fans Causing an Uptick in Police Incidents or Fire Department Call Volume?
San Francisco has been home to the AT&T Stadium for many years now, and while the stadium is a great source of joy for many, it is also a source of pain for locals.

For example, have you ever been late to an engagement because you were stuck on an overcrowded, tardy, and broken down Caltrain/Muni, filled with the light scent of alcohol and a sea of orange/black jerseys?

If so, you’ve likely been impacted by a Giants game! Frequent and powerful, Giants games have the power to bring many folks together, and also, hold them together on public transit. It has even gotten to a point where a website has been created to inform you whether or not there is a Giants game today: www.isthereagiantsgametoday.com.

Because of this, I was curious to see if this surge of sports fans had an impact on more than just my daily commute, and I decided to look into this with a smaller subset of data: the recent opening of the Chase Center Stadium - home to the Golden State Warriors.

## Datasets & Exploratory Analysis

### Dataset 1: Fire Department Calls for Service
<pre>-Data: 5243306 entries, 44 columns
<br>Null Values: 
<br>-14297 entries in “zipcode”
<br>-DtTm for specific incidents of the call are also missing, this may be interesting to look into if time permits, but not entirely necessary</pre>
<br>**Data Cleaning**
<pre>Zipcodes of interest ‘94107’,'94103', ‘94158’, ‘94105’
<br>Added 600 values for zipcode 94158
<br>Call Types of interest: 'Train / Rail Incident', 'Odor (Strange / Unknown)', 'Explosion','Traffic Collision','Alarms', 'Structure Fire', 'Other', 'Medical Incident’</pre>
<br>**Interesting Finds**
In general, the calls to the Fire Department have been increasing year over year, even when normalizing for population growth.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Fire_Calls_2000-2020.png)
There is also some seasonality of the data, with peaks at the beginning of each season and lows mid-season. 
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCalls2015-2020.png)
However, when looking into 2020, the call volume seems to have dropped, but this is also due to the fact that it is the "low" part of the seasonal volume, and we would expect to see an uptick in April based on current trend. Because of this, I will proceed with hypothesis testing only comparing dates after the stadium opened.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/FireCallsinmbdp2019-2020.png)


### Dataset 2: Police Incidents
**Report 1 (2003 - 2018)**
<pre> -2215024 entries, 33 columns
<br>-Do not have zip code or neighborhood, only address, Latitude & Longitude</pre>

<br>**Report 2 (2018 - Present)**
<pre>-332828 entries, 36 columns
<br>-Categories of Interest: 'Non-Criminal','Stolen Property','Miscellaneous Investigation','Other Miscellaneous’, 'Assault', 'Larceny Theft', 'Malicious Mischief', 'Disorderly Conduct', ‘Other', 'Suspicious Occ', 'Disorderly Conduct','Traffic Collision', 'Liquor Laws', 'Fire Report', 'Suspicious'</pre>
<br>**Data Cleaning**
<pre>Calculated the harversine distance between the latitude/longitude of all rows in data and the known latitude/longitude of all zipcodes in SF. Was able to estimate the zipcode in order to narrow down to the same zipcodes used for the Fire Department Analysis.</pre>
<br>**General EDA on Police Incidents as a whole**
<br>In general, crime experienced a steep decline up until 2011, where it has gradually increased and is relatively flat since 2013 and actually decreasing since the Chase Stadium Opened. Similar seasonality seen with Fire Department Calls, though it general with normalizing it looks like crime went down.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Total_Police_Calls_2003-2020.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/Incidents2003-2019.png)
<br>**Dogpatch/Mission Bay Data from before and after Stadium opening**
<br>Similar to the Fire Service Call Data, when looking into 2020, the volume seems to have dropped, but unlike the Fire Service Call data, there is not an obvious uptick in the winter, and instead is a gradual dropping of crime.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/policeincidentmbdp2019-2020.png)



### Dataset 3: Events at Chase Center Stadium Data
**Dataset was scraped from ChaseCenter.com/events in the form of a json file**
<pre>-Data: 92 rows, 25 columns
<br>-Contained the dates & names for each event held at the Chase Stadium since opening.
<br>-Used pandas to cut the json file down to 77 rows & 5 columns of relevant information.</pre>



## Hypothesis Testing
<br>**Null Hypothesis:** Fire Department Calls & Police Incidents during event dates = non-event dates
<br> **Alternative Hypothesis:** Fire Department Calls & Police Incidents during event dates > non-event dates
<br>**Alpha:** 0.05
<br> Methodology: Conducted MannWhitneyU Test and T-Test on both sample populations (#Calls/Incidents on Event Dates vs. #Calls/Incidents not on event dates), due to different sample sizes & standard deviations.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/firecallsdistribution.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/policeincidentdistribution.png)
<br><br>Distribution of Fire Service Calls & Police Incidents were roughly normally distributed, but indicated some outliers.
Below are the distributions prior to removing the outliers. 2 extreme outliers on 12/21/2019 & 1/11/2020 were removed from the datasets before additional analysis.
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonScatter_Police.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonBox_fire.png)
<br><br>
### Results: Fire Department Calls
**Number of Event Dates: 75, Number of Non-Event Dates: 134
<br>Incidents on Event Dates: 15761, Incidents not on Event Dates: 24776**
<br>-MannWhitneyU Test Result : pvalue = 0.069 -> Not-Significant
<br>-T-Test Statistic & Distribution: pvalue = 0.044 -> Significant 
<br>**Based on the T-Test, we can conclude that Event Dates have a higher number of daily Fire Department Service Calls than Non-Event Dates.**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_Fire.png)
<br><br>
### Results: Police Incidents
**Number of Event Dates: 75, Number of Non-Event Dates: 138
<br>Incidents on Event Dates: 3337, Incidents not on Event Dates: 5483**
<br>-MannWhitneyU Test Result : pvalue = 0.0262 -> Significant
<br>-T-Test Statistic & Distribution: pvalue = 0.0096 -> Significant
<br>**Based on the T-Test, we can conclude that Event Dates have a higher number of daily Police Incidents than Non-Event Dates.**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_Police.png)
<br> 
### What about effects of Shelter In Place?
**The data was captured from 9/6/2019 - 3/31/2020. Major Tech companies started having employees work from home around early March, and official Shelter In Place measures were enacted 3/19/2020. How did that impact calls/incidents?**
<br>-MannWhitneyU Test Result (Fire Service): pvalue = 0.15 -> Not Significant
<br>-MannWhitneyU Test Result (Police Incidents): pvalue = 0.10 -> Not Significant
<br>-T-Test Statistic & Distribution (Fire Service) : pvalue = 0.11 -> Not Significant 
<br>-T-Test Statistic & Distribution (Police Incidents) : pvalue = 0.11 -> Not Significant 
<br>**Based on the results of all hypothesis tests after accounting for Shelter In Place, we can conclude that there is not a significant difference in the daily Fire Department Service Call volume or Police Incidents between Event & Non-event dates.**
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_FireWOSIP.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/EventsVsNonHypotheisTest_PoliceWOSIP.png)

## Bonus: Basketball, or Concerts - Which causes more trouble?
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/firecallsbballconcertdist.png)
![image](https://github.com/sherryduong93/chasestadiumimpact/blob/working/Graphs/policesbballconcertdist.png)
<br>-MannWhitneyU Test Result (Fire Service): pvalue = 0.078 -> Not Significant
<br>-MannWhitneyU Test Result (Police Incidents): pvalue = 0.16 -> Not Significant
<br>-T-Test Statistic & Distribution (Fire Service) : pvalue = 0.10 -> Not Significant 
<br>-T-Test Statistic & Distribution (Police Incidents) : pvalue = 0.26 -> Not Significant 
<br>**No significant difference between Basketball Events or Concerts in Fire Department Call volume or Police Incidents.**

## Conclusion: Sports fans are only congesting public transit, and not much else.
<br>1. No significant difference in police incidents of fire department call volume on dates with events at the Chase Center.
<br>2. Shelter in Place has had a significant impact on decreasing call volume & incidents.
<br>3. No significant difference between basketball events and concerts on police incidents or call volume.


## Assumptions Made & Caveats....
<br>-Data was normalized using annual population for SF as a whole, not drilled down to zipcode.
<br>-The Chase Center Stadium opened in September of 2019, and due to Shelter In Place, the months for observation and comparison are only 5 months.
<br><br> **Looked into the week days of events versus non events in case the distribution of events was majority on weekends, which I hypothesize would have higher incidents/fire calls in general. Events were biased towards Saturdays with 25% of Events on Saturdays.**
<br>**Day of Week Events Distribution:**
<br>Monday: 0.13
<br>Tuesday: 0.12
<br>Wednesday: 0.13
<br>Thursday: 0.16
<br>Friday: 0.12
<br>Saturday: 0.26
<br>Sunday: 0.09

## Goals for future Analysis:
<br>-Look into Saturdays in particular to compare events versus non-events.
<br>-Compare results again after more time has passed.
<br>-What about the same analysis, but for the Giants games?
<br><br>**In terms of incidents or calls overall, would be interesting to look into**
<br>-Time of day or day of week
<br>-What exactly happened on 12/21/2019 & 1/11/2020 that drove both crime & fire so high? 
<br>-Why was crime so low in 2010-2012?
<br>-General trends prior to 2018 with the 2003 - 2018 report


