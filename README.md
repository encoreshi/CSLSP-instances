# CSLSP-instances
This repository contains three instance sets for the Charging Station Location and Sizing Problem.
 
# Description of instance sets

1. The instances in each instance set is grouped into one folder. 
Each instance is identified using a string `XYZ`.
- `X` indicates the type of instance 
	- if `X` is `W` it means that the instance accounts for the initial charging infrastructure in the area. 
	- if `X` is `N` it means that there is no initial infrastructure. 
- `Y` corresponds to the instance set, and can be  `A`, `B` or `C`.
- `Z` is the number of locations in the instance.
Therefore, instance `NA40` corresponds to an instance belonging to set A with 40 locations without initial infrastructure, and instance `WA40` corresponds to the same instance, with initial infrastructure. 

2. Each instance is described using three files: `instance_XYZ.txt`, `demand_data_XYZ.txt`, and `location_data_XYZ.txt`.

`instance_XYZ.txt` describes the general parameters of instance `XYZ`.
The file is a plain text file, organized as follows:
```
C(Int) - the number of locations
P(Int) - the total number of demand periods 
years(Int) - the number of years
cost_location(Float) - the cost of opening a new station
cost_install(Float) - the cost of installing a charger with two plugs
capacity(Float) - the capacity of a charger
range(Float) - the maximum coverage range

demand_points_of_year_1 - this describes which values of the demand is associated with the first year of the optimization (explained in further details when the demand file is described)
demand_points_of_year_2 - this describes which values of the demand is associated with the first year of the optimization (explained in further details when the demand file is described)
demand_points_of_year_3 - this describes which values of the demand is associated with the first year of the optimization (explained in further details when the demand file is described)
```

`location_data_XYZ.txt` describe the coordinates of each locations and the existing chargers in each location. 
There are 3 columns and C rows in this file. 
The first column in row i gives the x-coornadinate of location i. 
The second column in row i gives the y-coornadinate of location i.
The third column in row i gives the number of existing chargers in location i. 

`demand_data_XYZ.txt` file gives the demand generated in each period in each year for each location.
The file contains C rows and P columns, where C is the number of locations, and P is the total number of periods. 
The demand associated to the first year corresponds to the columns described in `demand_points_of_year_1`.
Hence, if `demand_points_of_year_1 = 0,1`, it means that for each location, the first and second values describe the demand values for the two periods of the first year.


# Complimentary information for realistic instances

Instances B and C have been generated based on data from the cities of Bologna and Genova, Italy.
Specifically, we used estimated population density data to generate both the set of demand locations and the charging demand at those locations.
The data used is available in the Geotiff file format from https://data.humdata.org/dataset/italy-high-resolution-population-density-maps-demographic-estimates. 
We now describe in further details the process used to generate the instances.

## Instance generation

The population density data used is organized in grid cells, where the size of each grid cell is about 825 square meters, and each grid-cell is associated a population estimate. 
We used the Python package Gdal (https://gdal.org) to read the longitudes and latitudes of each grid-cell, and the population of each grid-cell.
Then, we converted the longitudes and latitudes of the center of each grid-cell into coordinates in the projected coordinate system (Universal Transverse Mercator, `WGS\_1984\_UTM`). 
Therefore, the coordinates of the nodes in instances B and C are all expressed in `WGS\_1984\_UTM`
We discarded any cell with a population of 5 or less people.
We used the weighted $k$-means algorithm with the population density as weights to partition the cells into clusters, setting the number of clusters as 200.
The obtained clusters were used as the set of demand locations for instances B and C.
Finally, we generated the demand values for each zone based on the aggregate population of each cluster.

Note that the instances present in this repository do not include information on the existing charging infrastructure.
Due to privacy agreements, we could not disclose the version of instances B and C with existing infrastructure.

