# Client Project - A, B, C Team

# README & Executive Summary

## Team
- Antoine Mack
- Bryce Gillespie
- Christopher Williams

## Contents of Folder
- README & Executive Summary 
- Data Provided
    -  3.5 GB drone image of Dennery in TIFF format
    -  RGB data
    -  Elevation data - Digital Terrain Model (DTM)
    -  Lidar data - Digital Surface Model (DSM)
    -  GIS Layers from OpenStreetMap and Charim
    -  Image data containing .tiff, .aux, .prj files
    -  Metadata of Geo Coordinates
- Jupyter Notebooks
- Presentation - PowerPoint

## Executive Summary

### Contents of Executive Summary
- Problem
- Data
- Challenges
- Approaches & Models
    -  First Approach and Model
    -  Second Approach and Model
- Solution



### Problem

- We choose to address "Problem 12: Predicting Rooftop Quality and Integrity using Drone Image Data."

- "The Island of Saint Lucia needs to prepare its homes for the next hurricane season. This tool will use remotely sensed drone images (RGB and elevation) along with GIS layers from OpenStreetMap and Charim to assess which homes are at highest risk and which homes might need strengthening interventions (i.e new rooftops, roof straps). The tool will provide the user the ability to click on a building (in the pilot city of Dennery) and see some basic characteristics of the house that could be helpful for the government preparing for the next storm."


### Data
- The Data was provided by New Light Technologies and consisted of drone imaging of Dennery, St. Lucia. The data was made up of TIFF files of 3.52 GBs, RGB information, Elevation - Digital Terrain Model (DTM), Lidar - Digital Surface Model (DSM), GIS Layers from OpenStreetMap and Charim. The files were in the .tiff, .aux, .prj file formats containing metadata such as long/lat geo coordinates of buildings and terrain features.


### Approaches & Models
#### First Approach
- The first approach we took was to break the larger image into a smaller tiles and to manually label the building's roofs by their quality (excellent, good, fair, and poor), train a convolutional neural network and test the model on classifying the quality of the roofs. 
- Using this manual labeled approach, we entered the imagery into QGIS, mapped tiles from the QGIS processing, inserted it into TensorFlow RCNN (recurrent neural network) for object recognition, used a LabelIMG API to label the actually rooftops in multiple classes of quality and then trained the model on local GPU (Mask RCNN Inception V2). 
- The RCNN model was slow, computationally greedy, but accurate. The model used a scoring threshold to classify new data according to the trained classes. However, TensorFlow did not produce several key params such as the Intersection over Union of the different quality classes.  This model is working, but needs more training data to better classify rooftop quality. 

#### Second Approach  
- The second approach to processing the data we used was to extract ground truth labels and associate the labels with the image sliced into smaller tiles. 
- We tried a variety of automatic geospatial data labelers e.g. label-maker, ArcGIS, QGIS, OS GeoProject, OSM QA Tiles, GDAL, OpenStreetMap, and MapBox. However, none of these programs worked as intended, so we had to do a more semi-automatic label of data approach. Once the imagery data inserted into QGIS, it created a ShapeFile from the OpenStreetMap (OSM) format, then a vector mask was applied via Fiona & Rasterio, the mask was inserted back into QGIS and tiled into smaller individual images and then modeled into Keras ResNet50 using TensorFlow as the back-end.
- The second model was ResNet50. This model applies a 50 layer Residual neural network using pre-trained satelite imaging data from ImageNet datasets onto the new images. We ran it on an Amazon Web Service (AWS) GPU instance (p2.xlarge) using the Keras API. 



### Solution & Conclusion
- Our models did not within the time restrictions come up with the solution we were hoping for. But as we now know better how to work with the data in the file formats we were provided with, we can better attack the image processing, manipulation, extraction, and train/test splitting for a supervised learning model. Approaching this with a supervised learning with a renewed perspective, our work flow and model would need to build on the understanding of GIS we've acquired, image data, neural networks, and the appropriate back-end GPU processes to classify the drone data by object types and quality. 