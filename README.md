# Developing a Nile Basin Flash Flood Hotspots Map

In this repository, I download and create an automatically updating interactive map from data on flood hotspots in the Nile Basin, published by the Nile Basin Initiative on the Nile Basin Flash Flood Early Warning System: https://flashfloodalert.nilebasin.org/workspaces/6c407e1b-5d25-4d83-b782-b6c81f8648ee/applications/9a806474-3cdd-447a-8a01-9898d9974bf8.

1. In "hotspot_clusters_updater.ipynb," I download the layer "nile2:hybas_hotsport_cluster" (found on the WFS Capabilities URL for the geodatabase under <FeatureTypeList>: https://nilebasin-dss-data.azurewebsites.net/geoserver/nile2/wfs?SERVICE=WFS&REQUEST=GetCapabilities) using requests.
   
3. 
