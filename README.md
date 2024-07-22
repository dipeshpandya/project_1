# Team Picard
# Project 1


## Our Team
Team Picard is composed of Dipesh Pandya, Alexis Wukich, and Erica Yanoshak.

Team Roles:
Data Engineering: Dipesh Pandya
Presentation: Erica Yanoshak
Team Lead: Alexis Wukich


## Background
Our team initially met to discuss potential topics for our project. One of our team members is a UFO enthusiast who suggested working with the UFO Sightings database we had previously reviewed during Module 5. We felt comfortable using a dataset the curriculum team had previously vetted.


This project aimed to use exploratory data analysis to increase our understanding of documented UFO sightings in the United States and predict future sightings. Our goal was to answer the following questions:  
1. What can we learn about previously documented and recorded UFO sightings?
2. What is the trend?
3. How do we utilize the trend to predict future sightings?


## Installation


The following libraries and dependencies are required to run the project successfully:
- !pip install prophet
- !pip install jupyter_bokeh --upgrade
- !pip install hvplot --upgrade
- !pip install prophet
- !pip install cartopy
- !pip install geoviews




- import pandas as pd
- import matplotlib.pyplot as plt
- import prophet as Prophet
- import numpy as np
- import datetime as dt
- import hvplot.pandas
- import panel as pn # for panels to use with hvplot.points
- from prophet import Prophet


**Helpful hint**: After cloning the “picard” repository to your computer, add a sub-folder “images” to the folder so that the visualizations can be organized after running the code.


## Repository Files and Starter Code
ufoSightings.csv: our original dataset which is available in the “Resources” folder in the Repository


[Copy_of_Team_Picard.ipynb](https://github.com/dipeshpandya/picard/blob/a62e802fdafb7638341132f531bcbd5960ce89b3/Copy_of_Team_Picard.ipynb): completed notebook with data analysis and visualizations


## Methodology
### Phase 1: Cleaning and filtering the dataset to improve its usability and narrow the scope of our project


We began our project using the ufoSightings.csv file, which contained more than 80,000 documented UFO sightings from approximately 1910 to mid-2014. This data was collected and compiled by [The National UFO Reporting Center (“NUFORC”)](https://nuforc.org/).  We imported the ufoSightings.csv file and saved it as a DataFrame, **ufo_sightings_df**. We completed the following steps to make the DataFrame manageable for our project:


Converted datetime column to a datetime object
Sliced the “countries” column to include on the United States
Dropped blank rows using dropna
Removed columns that were not relevant to our analysis: “duration (hours/min),” “comments,” and “date posted.”
Converted “duration (seconds)” column to a float for use later with aggregations.
Extracted the year from the datetime column and added it to a separate “year” column.


After cleaning the data, we were left with a more functional DataFrame, **cleaned_us_ufo_df**.


### Phase 2: Identifying the most active period and states for UFO sightings


In phase two, we began analyzing the data. We started to see how UFO sightings had changed over time. Given the number of sightings, we grouped the data by year, as demonstrated in the![Fig. 1: UFO Sightings by Year Graph](https://drive.google.com/file/d/19hz8LAlKqbsvAtxVzBHlcRLgd4q6_zLI/view?usp=drive_link). This chart shows that UFO sightings stayed relatively steady but began to increase in the early to mid-1990s. In 1994, the number of UFO sightings went from 221 to 313, and since our dataset ended in 2014, we elected to further narrow our data to the twenty years from 1994 to 2014, contained in a new DataFrame, **cleaned_us_ufo_df_new**.


Since we had identified the most active years for sightings, we wanted to determine the most active states for sightings. Using **cleaned_us_ufo_df_new,** we identified states in the ninetieth percentile for either the number of sightings (count) and average duration of sightings and saved the results as a new DataFrame, **us_ufo_state_top_df**. The states in the 90th percentile for sightings included Arizona, California, Hawaii, Louisiana, Mississippi, New York, Texas, Virginia, and Washington, as seen in ![Fig. 2: Top 10 States for UFO Sightings](https://drive.google.com/file/d/1K46uW-yBIXBoAiNt8hmnGZz_yVtLuQTb/view?usp=drivesdk). 


We expected there would be a positive correlation between count and average duration, but this was disproven after analyzing the tables and graphs. There was, however, a moderately strong negative correlation between the count and average duration of sightings seen in ![Fig. 3: Correlation between Number of Sightings and Average Duration](https://drive.google.com/file/d/1PAsJNR5F8rEHqCpbOhlSMTHC9gc14ctb/view?usp=drive_link) 


### Phase 3: Predictions of State Data
We filtered our **cleaned_us_ufo_df_new** DataFrame by each of the top ten states, and excluded 2014 since it was a partial year with only five months of data. Using these filtered results, we created ten state-specific pivot tables for each state: Arizona, California, Hawaii, Louisiana, Mississippi, New York, Texas, Virginia, and Washington, aggregating the duration of sightings over datetime. 


Using the data from our pivot tables, we completed the following steps for Arizona, California, Hawaii, Louisiana, Mississippi, New York, Texas, Virginia, Washington, and National:
We created an instance of Prophet model and fit the model with each state-specific DataFrame
Created state-specific future DataFrames to hold predictions for ten years 
Used the predict method to calculate future sights for each state and then plotted the forecast and components


### Phase 4: Shapes


We were able to learn more about UFO sightings beyond their locations. The dataset provided a column “shape,” which identified the shape of the sighting. Before cleaning the data, there were twenty-eight unique types of shapes, which was cumbersome for analysis. To improve the data’s manageability, we broke the categories into six broader groups: “'circular,” 'light,” 'triangle,” "formation_changing,”  "geometric,” and "other_unknown.” We added a column to the DataFrame, “form,” to the DataFrame, which classified each sighting as one of the broader categories based on the shape.
- *circular* =  cigar, circle, round, sphere, oval, egg, cylinder, disk
- *light* = fireball, flare, flash or light 
- *triangle* = chevron, delta, triangle, pyramid or cone 
- *formation_changing* =  formation, changed or changing 
- *geometric* =  diamond, hexagon, crescent, cross, rectangle or teardrop
- *other_unknown* = other or unknown


### Phase 5: Visualizing the Data with a Geo Plot


Our goal was to visualize the state and shape the data we had collected. We brainstormed and collectively agreed that a geo-visualization would be an unique way to plot the data and would also serve as an opportunity for our group to stretch our skills. 


We returned to our original, cleaned DataFrame, **cleaned_us_ufo_df_new**. We knew that we would have to filter the DataFrame by our top ten states. However, we did not realize this filtering would be as involved as it was. We needed to convert our state abbreviations to state names to complete our geo-visualization. To do this, we:
1. Converted state abbreviations from two-letter lowercase abbreviations to two-letter uppercase to be able to match with publicly sourced data
2. Converted the uppercase state abbreviations to state names using a dictionary,lambda function and list comprehension


To ensure that our data was plotted correctly, we also needed to ensure that our data met specific formatting requirements, including:
1. Converting average duration from seconds to hours so the scale of the plot was more manageable
2. Ensuring no leading/trailing spaces in column names and dropping any columns containing “unnamed.”
3. Converting 'longitude' and 'latitude' columns are of numeric type


Our filtered and formatted data was saved in a new dataframe, **geo_df**, which we could plot using hvplot. ![Fig. 4: Geo-visualization Code](​​https://drive.google.com/file/d/1SaVGdClRRaXGoWZxy8Mrzj0rrOeJEn9m/view?usp=drive_link).


![Fig. 5: Geo-Visualization Screenshot](https://drive.google.com/file/d/1S6WVJBIYxYNip29ohUq7O8sJLuWEuOW8/view?usp=drive_link). 
## Results and Answers to Our Questions


### What can we learn about previously documented and recorded UFO sightings?
> As stated previously, and depicted in Figure 1, the number of UFO sightings began to increase steadily after 1993, and from 1994 to mid-2014, the most active states (or the states accounting for 90% of the data) were: Arizona, California, Hawaii, Louisiana, Mississippi, New York, Texas, Virginia, and Washington.


### Can we use this historical data to predict trends in future UFO sightings? 
We were able to analyze data from the visualizations and compile the information in Table 1. 
| State | 10-Year Trend | Best Day of Week | Worst Day of Week | Day of Year | Time of Day* |
|:---------|:---------|:---------|:---------|:---------|:---------|
| Arizona | Slight Decline | Wednesday | Friday | Early March | Afternoon |
| California | Plateau | Tuesday | Sunday | Early March | Early Morning |
| Florida | Plateau | Saturday | Thursday | Late August | Early Morning |
| Hawaii | Steady Decline | Monday | Thursday | Early July | Evening |
| Louisiana | Steady Increase | Friday | Wednesday | Early January | 
Early Morning |
| Mississippi | Steady Increase | Thursday | Tuesday | Late December | Early Morning |
| New York | Steady Increase | Friday | Sunday | Late June | Evening |
| Texas | Steady Decline | Saturday | Friday | Mid-September |Afternoon |
| Virginia | Steady Decline | Sunday | Saturday | Mid-August | Evening |
| Washington | Steady Increase | Friday | Sunday | Mid-August | Evening |
| National | Steady Increase | Friday | Thursday | Mid-August | Early Morning |


* Time of Day was broken down into the following categories, Early Morning (12:00 AM to 5:59 AM), Morning (6:00 AM to 11:59 PM), Afternoon (12:00 PM to 3:59 PM), Early Evening (4:00 PM to 8:00 PM), and Evening (8:00 PM to 11:59 PM).


The national trend plot and plot components can be seen in ![Fig. 6: National Trends](https://drive.google.com/file/d/1AZXGDvARfzatF0xyCr03luKtCUNpS2xl/view?usp=drive_link)
From a national perspective, the trends suggest that there will be a slight decline in UFO sightings over the next ten years. UFO Sightings peaked in mid-August and were most likely reported to have occurred on Fridays, and least likely to have occurred on Thursdays. The early morning hours between 1:30 AM and 2:00 AM were the most common time for sightings.


### How can we use these predictions?
Comparing the top ten most active states’ trends to the overall national trend could help a UFO enthusiast target specific locations and time frames for peak viewing or analysis. For example, based on this data we can determine that even though the national trend is moving downward, there are still four states (Louisiana, Mississippi, New York, and Washington) that are expected to see increases and two (California and Florida) that are expected to remain rather consistent.


Researchers could focus their efforts towards these states and use the data to plan when to either visit the area in hopes of witnessing a sighting, or when to increase data surveillance. For example, using the data from Mississippi in ![Fig. #](https://drive.google.com/file/d/1Pvx85I_g8z08yXGkKvxZD0VP08fjqVLD/view?usp=drive_link), UFO enthusiasts might plan a trip (or data analysts might focus on) the later half of December, and Florida, Washington and overall national events throughout the month of August.
	
## Noteworthy Pain Points and Learning Experiences
### Year and Index Mismatches
Initially, we were working under the assumption that our dataset ended in 2013. After cleaning the DataFrame and beginning our analysis, we noticed data appearing from 2014. Previous viewing of the ufo_sightings_df and cleaned_us_ufo_df had suggested the final rows were from 2013. We returned to the DataFrame and realized there were indeed sightings from 2014. However, the 2014 data was located approximately at index position 43,000 rather than the 2013 sightings found around index position 80,000 as seen in this [screenshot](https://drive.google.com/file/d/168eM_GTPJpLfTcBtGujMtVE_v1vw5avn/view?usp=drive_link).


We confirmed the true date range and ensured we sorted by datetime. We hypothesized that this 2014 data had been entered before the entry of 2013 data, so it was listed at an earlier index position. The disconnect between index position and chronological position served as a learning experience and reminder that these concepts are indeed unique and that it is worth confirming the minimum and maximum dates within a DataFrame. 
### Longitude
Early in our exploration, we wanted to  explore using longitude and latitude rather than by state. The initial exploration of this data proved frustrating as we continually received an error that said longitude was the “wrong type.” We attempted to cast it from an object to a float and even an integer, but the error remained. Ultimately, we sought assistance and could convert the pd.to_numeric function to ensure that 'longitude' was of numeric type.
> geo_df['longitude'] = pd.to_numeric(geo_df['longitude'], errors='coerce')




## Opportunities for Further Research
With a now-cleaned database and initial insights on UFO sightings from 1994 to mid-2014, there are opportunities for additional research. We were able to locate NUFORC data up to 2020 from Kaggle, There was insufficient time for us to extract data from engineer duration column as the data was co-mingled with text and there was not consistency on how duration time is captured. It would have required a lot more time and effort to clean it . However, it would be ideal to consider alternatelaternate data sourcessource that can provide more recent data that can be cleaned and merged with our DataFrame for a future project.


Also, more time could allow us to look at historical data with a more critical lens. UFO sightings are certainly not a new phenomenon, as demonstrated by our dataset, which contains more than one hundred years of occurrences. As we previously stated, there is undoubtedly a change in the number of sightings, beginning around 1994, and continues through the end of our data.


And while we focused on that approximate twenty-year time frame, there are some very interesting data points prior to that time frame. For example, in Figure 7, we see specific years with very “long” sightings, even though they might not have been as plentiful as a peak year like 2012/2013. For example, 1945 and 1991 had extremely high average durations and a spike in 1984. ![Fig. 7: Average Duration by Year](https://drive.google.com/file/d/1KDCf2PQj5hBZnivW2kRrEqM-Gnz0qlP-/view?usp=drive_link).


More recent data, coupled with a deeper analysis of historical data could also be beneficial to research how other variables, including holidays, weather events, changes in the geopolitical climate, proximity to military bases, and space launches and locations, might have impacted UFO sightings and provide tools to more precisely predict sightings in the future.


Also, with more time, we would have liked to learn how to overlay the plot component visualizations, so that we could have one visualization containing data from the top-ten states, as well as the overall national data.


With more time, we could expand this analysis to explore ways to expand the data set and use our data and analysis. For example, the data could be used as part of a feasibility study for building applications or services in a new and niche industry: UFO Tourism. 


## Resources Consulted and Credits
The following credit must be given for assistance with our project:
- Jeff Paine for his abbreviation_to_name [code](https://gist.github.com/JeffPaine/3083347)
- The curriculum team and Bootcamp Spot for providing us access to the original ufoSightings.csv dataset, which we explored in Module 5.
- Zohaib Khawaja, for helping us find a solution to convert longitude to a numeric so that we could build our geo-visualizaiton.


## Additional Resources

[NUFORC](https://nuforc.org/databank/)










