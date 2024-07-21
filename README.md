# Manhattan-Congestion-Zone-Animation-Analysis
Manhattan -- Traffic Congestion Zone -- Animated Map and Charts-- BranchAnalysis -- Python -- NYC Public Data

# Project by Gabriel del Valle, published July 21 2024
# NYC DATA SCIENCE ACADEMY
### For any questions about this project or to request full map videos or datasets, please feel free to reach out on Linkdin: 


## Full Project Summary:

### Three kinds of animated map (each with animated barcharts):

- Average congestion per street per datetime

- (Imputed Missing Data) Average congestion per street per datetime

- Congestion per data recording node per datetime (Congestion color coded marker on map indicating recording location)

In light of the plan for the Manhattan Congestion Relief Zone, which was originally scheduled to go into effect in June of 2024 (but was cancelled on the last day by the governor of New York), this study seeks to visualize New York City traffic data into a series of animated maps with corresponding bargraphs, which illustrate congestion (a normalized measure of street mobility, congestion = current street volume / max street volume) and total volume respectively. These maps summarize the data of three months of NYC traffic volume (Octobers of 2016 - 2019) in an intuitive manner so traffic and data recording patterns are easily observed.

### Traffic routes branch analysis estimation and mapping system

In order to provide actionable insights to buinesses and institutions anticipating this policy by wondering which streets will should expect to see gains in mobility (and thus strategic value), I created a branch analysis method for the dataset and a means to map it. The branch analysis starts from a selected street and a selected hour. Using the average traffic volume from that street and hour, it estimates the distribution of vehicles to each next street the vehicle could take. This process can be repeated for as many branches as desired, estimating the number of vehicles from the originally selected street and hour distributed to each subsequent branch, assuming a driver will not backtrack. 

## In this repository you will find...

Seven sequential Python Jupyter notebooks outline step by steped with detailed comments how to produce three kinds of animated traffic maps, as well as how to perform and graph a branch analysis.

There are two publicly available datasets which must be used to run these notebooks, both from OpenNYC and detailed in the next section of the README. NYC Traffic Volume Data and Geojson plottable map data.

- 01_Initial_Congestion_Zone_Data_Framing.ipynb
Refine both traffic data and map data to area of interest and align their naming schemes
  
- 02_AzureSynapse_czone_missing_dates.ipynb
A Microsoft Azure Synapse Analytics Notebook, made to work with Apache pySpark. Distributed computing is used to impute NA for the unrecorded intervals of time per street per datetime in the traffic counts dataset. While this process was useful in my investigation for data density to make the public code more integral, this information is only strictly necessary for imputing missing data, which forms the basis for only one of the three animated maps.

- 03_Post_Process_Data_Exploration.ipynb
  - Find the distribution of data and missing data
  - Create Descriptive Statistics
  - Explore Mapping Methods
  - Impute Missing Data

- 04_Congestion_Map_and_Bargraph_Animation.ipynb
This python jupyter notebook contains several functions that can be used together to produce a video of a Manhattan Congestion Zone Map, displaying average volume per street per datetime.

As well, this notebook follows a similar process to create a video of a bargraph which corresponds to the animated congestion map, producing a bar graph, labeled by street, measuring the traffic volume for each datetime frame. The colors of the bars represent congestion with the same colors used in the map.

The videos of the map and the bargraph which are the products of this notebook can be played side by side and will correspond as a single video.

This notebook will outline the steps for animating the entire dataset, which is 1 hour 39 minutes long. However, the functions can easily be used to produce a shorter slice of video given a start and an end row.

It takes hours to render the whole czone_October dataset, but on my macbook frames are produced almost once per second. 

- 05_Imputed_Congestion_Map_and_Bargraph_Animation.ipynb
This notebook is the same as the previous but using the imputed_congestion.csv dataset

- 06_Animate_WktGeom_Map.ipynb
In previous notebooks for animating maps I colored the streets on the map themselves for clarity while representing traffic data per datetime. However, this method requires aggregation, as there are multiple segements per street per datetime with their own unique rows. As a result, previous maps were a descriptive statistic rather than a form of simulation.

The maps produced by this notebook on the other hand are a much closer representation of the dataset, and at times comes close to simulating the elastic nature of traffic movement.

It does so by mapping the WktGeom data included in the original Automated Traffic Counts Dataset from OpenNYC, which indicates with coordinates the location at which data was recorded. When Plotted these coordinates create a circular marker but not a map. By combining these markers with the geojson map, I was able to give them context and a use.

- 07_Traffic_Routes_Branch_Analysis.ipynb
Finally, this notebook seeks to establish a method for producing specific predicitions for the impact of the Congestion Relief Zone, identifying the specific streets where one would expect to see increases in mobility as a result of the program, and thus establish actionable buisness insights.

To do so, it uses branch analysis, identifying the possible next streets one could take from a given street. The analysis starts from a chosen source street, from which traffic enters the congestion releif zone. The average volume of the source street at the selected hour is the basis for the estimate. With each branch added to the analysis, an estimate is made for how much traffic volume is passed from the source street at the selected hour to each street of the branch.

Thus with this method, one can create an estimate as to how much traffic from a given source street at a given hour impacts the traffic volume of the entire congestion zone. By quantifying how much traffic volume each source street contributes to the rest of the streets in the region, this analysis method provides a baseline to compare and measure the mobility effects from the implementation of a congestion zone. Further, using this method one can estimate which streets will receive the greatest gains in mobility by identifying which streets receive the most traffic from outside the Congestion Relief Zone.


Again, if you'd like access to datasets, map images, or map videos, please feel free to reach out on Linkedin (address at top of page).


### Data sources

#### Automated Traffic Counts Dataset
#### OpenNYC
#### As of 07/11/24, last updated -- 04/02/24

https://data.cityofnewyork.us/Transportation/Automated-Traffic-Volume-Counts/7ym2-wayt/about_data

#### Dates Querried: 2016 to 2019
#### Note: Data collection ceased after 2019
#### Official description:

"New York City Department of Transportation (NYC DOT) uses Automated Traffic Recorders (ATR) to collect traffic sample volume counts at bridge crossings and roadways.These counts do not cover the entire year, and the number of days counted per location may vary from year to year."

#### Columns: 

- 'RequestID' : An unique ID that is generated for each counts request
- 'Boro' : Lists which of the five administrative divisions of New York City the location is within, written as a word 
- 'Yr' : year
- 'M' : month
- 'D' : day
- 'HH' : hour
- 'MM' : minute
- 'Vol' : traffic volume
- 'SegmentID' : "The ID that identifies each segment of a street in the LION street network version 14."
- 'WktGeom' : A text markup language for representing vector geometry objects on a map and spatial reference systems of spatial objects.
- 'street' : The 'On Street' where the count took place
- 'fromSt' : The 'From Street' where the count took place
- 'toSt' : The 'To Street' where the count took place
- 'Direction' : Cardinal direction of traffic flow



#### NYC Street Centerline (CSCL) -- Geojson shapefile
#### OpenNYC
#### As of 07/11/24, last updated -- 07/08/24

https://data.cityofnewyork.us/City-Government/NYC-Street-Centerline-CSCL-/exjm-f27b

#### Official description:


The NYC Street Centerline (CSCL) is a road-bed representation of New York City streets containing address ranges and other information such as traffic directions, road types, segment types
Previously posted versions of the data are retained to comply with Local Law 106 of 2015 and can be provided upon request made to Open Data.

#### Project relevant columns: 

- 'borocode' : A 1-digit code identifying the borough the feature is located in (Manhattan = 1)

- 'geometry' : polyline (plotable map geometry)

- 'rw_type' : Street Centerline roadway type.

- 'r_zip' : Five-digit postal zip code for the right side of the street segment, relative to the digitized direction of the segment. 

- 'l_zip' : Five-digit postal zip code for the left side of the street segment, relative to the digitized direction of the segment.

- 'st_name' : Street Name added for cartographic labeling purposes (without street suffix)

- 'st_label' : (with street suffix)


