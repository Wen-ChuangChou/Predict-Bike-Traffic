# Predicting Bike Traffic Using Graph Neural Networks: Integrating Residential Density, Amenity Distribution, and Street Networks 

## Motivation
Cycling is considered to be more environmentally sustainable than the use of private vehicles and offers greater flexibility and accessibility compared to public transportation. Additionally, rising fuel costs and public transportation fares have further incentivized individuals to turn to bicycles as a mode of transportation. To accommodate this shift in travel patterns, authorities are keen to understand which factors drive individuals to choose bicycle transportation, how urban development can generate new demand for cycling, and how to promote bicycle usage through strategies such as the installation of dedicated bike infrastructure.

## Task
The task involves predicting the volume of bike traffic using features derived from a social and environmental dataset. These features include residential density, distribution of amenities, and characteristics of street networks (such as length, connections, and street types). 

## Dataset
This [dataset](https://www.mcloud.de/web/guest/suche/-/results/suche/relevance/stadtradeln/0/detail/ECF9DF02-37DC-4268-B017-A7C2CF302006) contains cycling volumes recorded by users of the Stadtradeln app in Germany during a 3-week period in 2020 as part of the "Stadtradeln" campaign by Klima-Bündnis e.V. The data processing was conducted as part of the MOVEBIS research project at TU Dresden. The contribution is credited to "Grubitzsch P., Lißner S., Huber S., Springer T., [2021] Technische Universität Dresden, Professur für Rechnernetze und Professur für Verkehrsökologie". It is important to note that the absolute values are related to the number of participants in the cycling campaign, and the data does not represent the actual values of total bicycle users in cities as a whole.  
  
In this project, data from three cities, namely two intermediate cities, Dresden and Leipzig, and one large city, Hamburg, were selected to train the model and predict bike traffic.  
![image](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/bike_traffic_in_cities.png?raw=true)  

The networks and types of streets were retrieved from [OpenStreetMap](https://www.openstreetmap.org) (OSM). TThe street types are categorized into four classes: cycleway, main street, residential street, and path. Here is an example of a street network:  
![street network](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/road_network.png?raw=true)  

Generally, larger buildings tend to accommodate more people. Therefore, to estimate the residential density within a buffer zone (30m) along streets, I represented it with the total floor area of buildings, which is calculated from the size of building footprints and the respective levels retrieved from [OSM](https://www.openstreetmap.org). Specific keywords from the OSM classification hierarchy were utilized to identify the locations of amenities, encompassing economic and social facilities of interest. The numbers of individual amenities within a buffer zone (60m) of streets were represented as feature vectors for those streets. The following tags (keywords) were employed to retrieve the distribution of amenities from [OSM](https://www.openstreetmap.org):  
"department_store", "general", "mall", "supermarket", "wholesale", "kiosk", "convenience", "variety_store", "health_food", "greengrocer", "butcher ", "pastry", "bakery", "ice_cream", "books", "stationery", "ticket", "copyshop"  "bag", "clothes", "shoes", "cosmetics", "doityourself", "hardware", "houseware", "bed", "carpet", "furniture", "interior_decoration", "computer", "electronics", "outdoor", "sports", "florist","garden_centre", "laundry", "pet", "toys", "biergarten", "cafe", "fast_food", "food_court", "ice_cream", "restaurant", "bar", "pub", "nightclub", "theatre", "cinema", "marketplace", "place_of_worship", "bank", "pharmacy", "chemist", "post_office", "townhall", "library","kindergarten", "school", "college" , "park", "playground", "stadium", "fitness_centre", "fitness_station", "pitch", "sports_centre", "university".

Here is an example of a buffer zone (60m) along the streets:  
![street network](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/amenities_buffer_zone.png?raw=true)  

### Graph data
Streets are interconnected, creating a graph-based data structure. For this project, each street segment is represented as a node, and each node is associated with a 43-dimensional feature vector. The feature vector includes segment length, street type, floor area, and the number of amenities. Intersections serve as edges, representing the connections between streets.

## Model architecture
Because our data are structured as graphs containing multidimensional node features, a graph neural network (GNN) were implemented, specifically a Graph Attention Network, as the basis of our model to learn what are the mobility patterns of bicycle users associate and predict bike traffic. This GNN is included in the PyTorch Geometric library, which is designed for training and evaluating models on graph-structured data.
## Results

## Reference
1. P. Grubitzsch *et al.*, Technische Universität Dresden, Professur für Rechnernetze und Professur für Verkehrsökologie, 2021
2. OpenStreetMap contributors. https://www.openstreetmap.org, 2023
3. G. Boeing, “OSMnx: New methods for acquiring, constructing, analyzing, and visualizing complex street networks,” *Computers, Environment and Urban Systems*, vol. 65, pp. 126–139, Sep. 2017, doi: https://doi.org/10.1016/j.compenvurbsys.2017.05.004.
4. P. Veličković *et al.*, “Graph Attention Networks,” *arXiv*:1710.10903, Feb. 2018, https://arxiv.org/abs/1710.10903

‌

‌

‌
