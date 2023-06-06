# Predict Bike Traffic

## Motivation
Cycling is considered to be more environmentally sustainable than the use of private vehicles and offers greater flexibility and accessibility compared to public transportation. Additionally, rising fuel costs and public transportation fares have further incentivized individuals to turn to bicycles as a mode of transportation. To accommodate this shift in travel patterns, authorities are keen to understand which factors drive individuals to choose bicycle transportation, how urban development can generate new demand for cycling, and how to promote bicycle usage through strategies such as the installation of dedicated bike infrastructure.

## Task
 Predicting the volumn bike traffic based on social and environmental factors including residential density, amenity distribution, and street networks (length, connections, street types). 

## Dataset
This [dataset](https://www.mcloud.de/web/guest/suche/-/results/suche/relevance/stadtradeln/0/detail/ECF9DF02-37DC-4268-B017-A7C2CF302006) contains cycling volumes recorded by users of the Stadtradeln app in Germany during a 3-week period in 2020 as part of the "Stadtradeln" campaign by Klima-Bündnis e.V. The data processing was conducted as part of the MOVEBIS research project at TU Dresden. The contribution is credited to "Grubitzsch P., Lißner S., Huber S., Springer T., [2021] Technische Universität Dresden, Professur für Rechnernetze und Professur für Verkehrsökologie". It is important to note that the absolute values are related to the number of participants in the cycling campaign, and the data does not represent the actual values of total bicycle users in cities as a whole.  
  
In this project, data from three cities, namely two intermediate cities, Dresden and Leipzig, and one large city, Hamburg, were selected to train the model and predict bike traffic.  

![image](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/bike_traffic_in_cities.png?raw=true)  

The networks and types of streets were retreived from OpenStreetMap (OSM). The types of streets are categorized into 4 classes: cycleway, main street, residential street, and path. An example of a street network:  
![street netowkr](https://github.com/Wen-ChuangChou/Predict-Bike-Traffic/blob/main/doc/fig/road_network.png?raw=true)  

## Model architecture

## Results

## Reference
1. P. Grubitzsch .et al., Technische Universität Dresden, Professur für Rechnernetze und Professur für Verkehrsökologie, 2021
2. OpenStreetMap contributors. https://www.openstreetmap.org, 2023
3. G. Boeing, “OSMnx: New methods for acquiring, constructing, analyzing, and visualizing complex street networks,” Computers, Environment and Urban Systems, vol. 65, pp. 126–139, Sep. 2017, doi: https://doi.org/10.1016/j.compenvurbsys.2017.05.004.

‌

‌
