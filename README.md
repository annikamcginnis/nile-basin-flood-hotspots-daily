# Developing a Nile Basin Flash Flood Hotspots Map

In this repository, I download and create an automatically updating interactive map from data on flood hotspots in the Nile Basin. 

Data on flash flood risk, ranked from 0 to 3, for 50 hotspots in the Nile Basin is published daily by the Nile Basin Initiative on the Nile Basin Flash Flood Early Warning System: https://flashfloodalert.nilebasin.org/workspaces/6c407e1b-5d25-4d83-b782-b6c81f8648ee/applications/9a806474-3cdd-447a-8a01-9898d9974bf8.

This is the map, which updates once daily: https://annikamcginnis.github.io/nile_basin_flood_hotspots_daily/

## Step 1: Scraping Flood Risk Geodatabase

1. In "hotspot_clusters_updater.ipynb," I download the layer "nile2:hybas_hotsport_cluster," which contains all the information for the hotspots, using requests. I identified this layer on the WFS Capabilities URL for the NBI geodatabase under <FeatureTypeList>: https://nilebasin-dss-data.azurewebsites.net/geoserver/nile2/wfs?SERVICE=WFS&REQUEST=GetCapabilities).
2. From the JSON data received as the response, I extract IDs, coordinates, flood risk and dates for each hotspot and add them to a list of dictionaries in Python.
3. I import this list to a Pandas dataframe. I clean the data using Pandas functions and regular expressions.
4. Grouping by coordinates, I determine that the dataframe includes data for 50 hotspots, repeated daily. I create a dataframe including data only for the last two weeks (700 rows).
5. I add columns in Pandas for latitude, longitude, the maximum flood risk on any day in the last two weeks, and the sum of flood risk values in the last two weeks through conducting various Pandas functions.
6. Bringing the dataframe back to a Python list, I add in a column called radius, setting hotspots with a two weeks aggregate flood risk of 0 to 1 and hotspots with a two weeks aggregate flood risk of greater than 0 to (sum of risk + 1) * 3. This is done so that Datawrapper can recognize the size of all points (even those with a 0 flood risk value) and size by the total risk in the last two weeks.
7. Using the Google Maps API in Python, I reverse Geocode the coordinates to find the names of each hotspot location. Then I add the locations to the dataframe. I change null values to 'Unknown.'
8. Using datetime in Python, I change the date format in the date column to [Day of the Week], [Month] Day, Year. 
9. I create a dataframe only for today's date by extracting the first 50 rows from the revised dataframe.
10. I save both dataframes to CSV files.

## Step 2: Auto-Updating the Data 

1. I created a Github repo for this project.
2. In Github Actions, I created a .yml file (update.yml) that imports all the libraries I used in my data download and analysis, requesting the daily dataframe ("flood_risk_today.csv") to update every day at 7pm EST through the Python file "hotspot_clusters_updater.ipynb." For this, I modified the .yml file from jsoma's bad-air-cities tutorial: https://github.com/jsoma/bad-air-cities/blob/main/.github/workflows/update.yml

## Step 3: Creating Datawrapper Map 

1. I created a Symbol map in Datawrapper.de, linking an external dataset to the flood_risk_today.csv file on my repo: https://github.com/annikamcginnis/nile_basin_flood_hotspots_daily/flood_risk_today.csv
2. I sized points by the radius values.
3. I colored points by the daily flash flood risk values, on a sliding scale.
4. I added a color and size legend.
5. I modified tooltips to include information upon hover for each hotspot's location, flood risk today, and maximum flood risk in the last two weeks.

## Step 4: Creating the Map Website 

1. I created an HTML website file ("index.html") by modifying the index.html file from jsoma's bad-air-cities tutorial: https://github.com/jsoma/bad-air-cities/blob/main/index.html
2. I inserted the responsive iFrame embed code from my Datawrapper map.
3. I modified the CSS to extend the map across the screen and tailor the page design.
4. I published the index.html file using Github Pages.

