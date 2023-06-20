# Predicting Bike Traffic Using Graph Neural Networks: Integrating Residential Density, Amenity Distribution, and Street Networks 

## Motivation
Cycling is considered to be more environmentally sustainable than the use of private vehicles and offers greater flexibility and accessibility compared to public transportation. Additionally, rising fuel costs and public transportation fares have further incentivized individuals to turn to bicycles as a mode of transportation. To accommodate this shift in travel patterns, authorities are keen to understand which factors drive individuals to choose bicycle transportation, how urban development can generate new demand for cycling, and how to promote bicycle usage through strategies such as the installation of dedicated bike infrastructure.

## Task
The task involves predicting the volume of bike traffic using features derived from a social and environmental dataset. These features include residential density, distribution of amenities, and characteristics of street networks (such as length, connections, and street types). 

## Dataset
The [dataset](https://www.mcloud.de/web/guest/suche/-/results/suche/relevance/stadtradeln/0/detail/ECF9DF02-37DC-4268-B017-A7C2CF302006) of cycling volumes was recorded by users using Stadtradeln app in Germany during a 3-week period in 2020 as part of the "Stadtradeln" campaign by Klima-Bündnis e.V. The data processing was conducted as part of the MOVEBIS research project at TU Dresden. The contribution is credited to "Grubitzsch P., Lißner S., Huber S., Springer T., [2021] Technische Universität Dresden, Professur für Rechnernetze und Professur für Verkehrsökologie". It is important to note that the absolute values are related to the number of participants in the cycling campaign, and the data does not represent the actual values of total bicycle users in cities as a whole.  
  
In this project, data from three cities, namely two intermediate cities, Dresden and Leipzig, and one large city, Hamburg, were selected to train the model and predict bike traffic.  
![image](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/bike_traffic_in_cities.png?raw=true)  

The networks and types of streets were retrieved from [OpenStreetMap](https://www.openstreetmap.org) (OSM). TThe street types are categorized into four classes: cycleway, main street, residential street, and path. Here is an example of a street network:  
![street network](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/road_network.png?raw=true)  

Generally, larger buildings tend to accommodate more people. Therefore, to estimate the residential density within a buffer zone (30m) along streets, I represented it with the total floor area of buildings, which is calculated from the size of building footprints and the respective levels retrieved from [OSM](https://www.openstreetmap.org). Specific keywords from the OSM classification hierarchy were utilized to identify the locations of amenities, encompassing economic and social facilities of interest. The numbers of individual amenities within a buffer zone (60m) of streets were represented as feature vectors for those streets. The following tags (keywords) were employed to retrieve the distribution of amenities from [OSM](https://www.openstreetmap.org):  
"department_store", "general", "mall", "supermarket", "wholesale", "kiosk", "convenience", "variety_store", "health_food", "greengrocer", "butcher ", "pastry", "bakery", "ice_cream", "books", "stationery", "ticket", "copyshop"  "bag", "clothes", "shoes", "cosmetics", "doityourself", "hardware", "houseware", "bed", "carpet", "furniture", "interior_decoration", "computer", "electronics", "outdoor", "sports", "florist","garden_centre", "laundry", "pet", "toys", "biergarten", "cafe", "fast_food", "food_court", "ice_cream", "restaurant", "bar", "pub", "nightclub", "theatre", "cinema", "marketplace", "place_of_worship", "bank", "pharmacy", "chemist", "post_office", "townhall", "library","kindergarten", "school", "college" , "park", "playground", "stadium", "fitness_centre", "fitness_station", "pitch", "sports_centre", "university".

Here is an example of a buffer zone along the streets used to retrieve amenities associated with those streets:  
![street network](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/amenities_buffer_zone.png?raw=true)  

### Graph data
Streets are interconnected, creating a graph-based data structure. For this project, each street segment is represented as a node, and each node is associated with a 43-dimensional feature vector. The feature vector includes segment length, street type, floor area, and the number of amenities. Intersections serve as edges, representing the connections between streets.

## Model architecture
Because the data is structured as graphs with multidimensional node features, I implemented a graph neural network (GNN) as the foundation of our model to capture the mobility patterns associated with bicycle users and predict bike traffic. Specifically, we used a Graph Attention Network (GAT). In this project, the PyTorch Geometric library, designed for training and evaluating models on graph-structured data, was utilized to construct our model.  

## Results
The model was trained to predict bike traffic based on the residential density, amenity distribution, and street networks in each individual city. During training, the feature vectors of every street served as inputs, and the model generated predictions for bike traffic on those streets. The objective was to minimize the discrepancies between the predicted and actual bike traffic data. The training procedure followed a semi-supervised learning approach, where the model was exposed to the true traffic data for 80% of the streets (training data), and then tested on the remaining 20% of streets to predict the bike traffic (testing data). The following plot illustrates the results obtained from models trained on three cities:  
![prediction](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/prediction.png?raw=true)  

Prediction errors on street networks:  
![errors on map](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/errors_on_maps.png?raw=true)  

Next, we conducted tests to determine if a model trained on data from one city could also accurately predict bike traffic in another city. The following plot demonstrates the transferability of models trained on different cities. Each row in the plot represents the same model trained using data from a specific city, while each column represents the data from the same city tested using different models.  
![model transferbility](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/transferbility.png?raw=true)

Finally, we employed GNNexplainer, an explainable machine learning technique, to identify the influential node features for predicting bike traffic, including residential density and individual amenities/facilities.
![feature importance](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/feature_importance.png?raw=true)

GNNexplainer also provided insights into the number of neighboring streets required for accurate bike traffic predictions.  
![size importance](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/size_importance.png?raw=true)

## Future work
In fact, the graph data is structured based on the traces of bike traffic rather than the network of streets. Consequently, bike traffic along certain sections of streets is recorded on multiple lanes of the same street, resulting in redundant street segments (nodes) and intersections (edges).  

The following figure provides an example where numerous traffic traces intersect at a large intersection, leading to an abundance of nodes and edges. These results increase the data size without adding additional information. Furthermore, small street segments within intersections (indicated by an arrow) often have a short length and provide limited information regarding residential density and amenity distribution within their buffer zones. However, these tiny segments labeled with high traffic volumes are usually connected to longer streets along blocks with similar traffic volumes, where more people reside and more amenities are located. Such contradictory data poses challenges for the training model. Therefore, it is necessary to preprocess the bike traffic to enhance the model's performance. For instance, an algorithm is required to identify bike traffic volumes within the same street segment across multiple traces or lanes and merge them into a single value. Please note that the blue dots in the following figures represent the connected points between street segments.  
![data preprocesssing](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/data_preprocessing.png?raw=true)


## Reference
1. P. Grubitzsch *et al.*, Technische Universität Dresden, Professur für Rechnernetze und Professur für Verkehrsökologie, 2021
2. OpenStreetMap contributors. https://www.openstreetmap.org, 2023
3. G. Boeing, “OSMnx: New methods for acquiring, constructing, analyzing, and visualizing complex street networks,” *Computers, Environment and Urban Systems*, vol. 65, pp. 126–139, Sep. 2017, doi: https://doi.org/10.1016/j.compenvurbsys.2017.05.004.
4. P. Veličković *et al.*, “Graph Attention Networks,” *arXiv*:1710.10903, Feb. 2018, https://arxiv.org/abs/1710.10903
5. R. Ying *et.al*, “GNNExplainer: Generating Explanations for Graph Neural Networks,” *arXiv*:1903.03894, Nov. 2019, https://arxiv.org/abs/1903.03894

‌

‌

‌

‌
