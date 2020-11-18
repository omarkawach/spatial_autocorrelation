# Spatial Autocorrelation

### Purpose

Review the Zhong model maybe? 

### Background

In normal cases, statistics prefers to have sampled data be random. However, this is not the case with spatial autocorrelaiton statistics. 

Things to discuss: 
- Data manipulation
- Statistical analysis
- Modelling / Visualizing 

Spatial autocorrelation statistics follows Toblers first law of Geography where near things are more related than distant things ([TFL](https://wiki.gis.com/wiki/index.php/First_law_of_geography)). 

When conducting statistics tests, the outcome of the correlation coefficient from Moran's I explains the relationship between two values (or multiple locations).

**Maybe use bivariate instead of "relationship between two values"?**. 

The relationship between the two values can either be by chance or correlated (this is where you would reject or accept the hypothesis and show that the distribution is/isn't random, which is the same as cause and effect in data). Also look at p-value and z-score. 

#### Moran's I 
- In the formula: 
  - n is the number of regions or spatial units
  - w is the weights (will be discussed below)
- Can incorporate topological relationship types (see my other repo for more background)
- Output is between -1 and +1
  - +ve is similar values found together
  - -ve is disisimilar values found together (rare)
  - 0 means distribution is **random** and no spatial autocorrelation
- Measure signifigance 
  - p-values (prob. spatial autocorrelation is correlated, look for low value )
  - z-score (absolute value means your data is likely to spatially autocorrelated, look for high value)
  - Distance decay
    - Moran's I decreases as more observations (farther away) are included in the calculations, therefore less influence
- Great for finding hotspots (LISA)
- Spatial relationships (weights matrix, GWR)

Nearest neighbour, inverse distance, and classifcations are generally not geostatistics

#### Spatiotemporal (space and time)


#### Limitations

Spatial dependance issues (how data relates in space) that impact statistical tests

- MAUP (Modifiable Areal Unit Problem)
  - Boundaries impact statistical tests (look at stdev most importantly)
  - Location of boundaries used to aggregate data can influence results of statistical tests (Moran's I)
  - Gerrymandering is a good example of this 
- Be aware for ecological fallacy 
  - Individuals vs Popuplations 
  - Cant take aggregated results and apply them to individuals
  - Statistical test results change based on data aggregation
- Spatial Scale
- Boundary Problem
  - Might lead to loss of information

Other issues exist

- Does the neighbouring make sense? Remember that if you use queen or rook contiguity, you cant have islands or disconnected data since they will have less or no neighbours. You'd need to use a distance weight instead.
  - Lower the weight of things further away if you do distance based weight which follows Toblers first law


#### Spatial Lag Model

[Read the spatial lag / autoregressive model SAR paper on The Biggest Myth in Spatial Econometrics ](https://www.mdpi.com/2225-1146/2/4/217/htm) 

[Regression Analysis](https://libraries.mit.edu/files/gis/regression_presentation_iap2013.pdf)

Things to note 
- Row standardizaton weights between 0 and 1 and interpretation as average of neighbours
- Probably want to use weights matrix instead of contiguity matrix (0s and 1s)
    - Sum of all weights would then equal to one
        - ex. three neighbours to one and divide by 3
        - ex. 5 neighbours divide 1 by five
    - Gives each area we're interested in an equal weight
      - Each give us an equal amount to what we're computing
        - spatial correlating or regression

Weight matrices (any of these shouldn't have too large of an impact)
- Queen
  - Shared boudnary or shared corner
- Rook
  - only shared boudnary 
- Distance
  - Make sure your data is projected properly (coordinate system)
  - Neighbors within distance (by row standardization)
  - Know if lat/lon or cartesian first 
  - Lon / Lat
    - Use arc distance (over surface of sphere)
  - Cartesian 
    - Use euclidiean 
- Invserse distance
  - Closest in some sense by strongest neighbour 
    - Could be by non spatial aspects 
- Kernel
- K-nearest neighbor (knn)
  - X # of nearest neighbours 
  - Based on polygon centroids and euclidian distance

More factors to consider 
- Precision threshold 
    - When two shapes meet at a point, how precise do you want the point to be
- Symmetry
  - Asymetric
  - Non-asymetric
- Order of Contiguity
  - 1 
    - Only if you touch a border or a corner directly will you be a neighbour
  - 2
    - Neighbour of a neighbour is also a neighbour of mine 
    - Your neighbour is also your neighbours neighbour?
    - 


#### Local Indicators of Spatial Association (Space tab in GeoDa)

Cluster Map (where the concentrations are, therefore hot spots and cold spots

Spatial clusters
- high high (red)
  - high conc within itself and surrounding areas
- low low (blue)
  - low conc when surrounding areas have more conc

Spatial outliers 
- high low (light red) 
  - high conc when surrounding areas have low conc
- low high (light blue)


LISA SIGNIFIGANCE MAP (see how confident we can be in our clusters, darkest values are most confident to show this relationship wasnt found by chance)

Moran's I scatterplot (draws the cluster map)

#### The weights process in GeoDa

[Tutoria for knn](https://geodacenter.github.io/workbook/4b_dist_weights/lab4b.html#creating-knn-weights)

- Take a shapefile
- Load a weight
- Create spatially lagged variable (spatial lag section in table calculator)
  - Options (try these yourself to be sure, don't take my word for it)
    - Spatial lag with row standardized weights
      - Ex. Avg houses of 6 neighbors = (x1 + x2 ... x6) / 6 with row standardized weights
    - Spatial lag as a sum of neighboring values
      - Ex. Sum of the population of the six neighbors
    - Spatial window average
      - Ex. The observation itself and the average so divided by 7 instead
    - Spatial window sum
      - Ex. Same as above but without dividing by 7

#### Developing the right shapefile

Begin by collecting COVID data from [Ottawa Public Health (Oct 2020)](https://www.arcgis.com/home/item.html?id=7cf545f26fb14b3f972116241e073ada#overview)

Get ward shapefile from open data 

Get building footprints for Ottawa

Create new shapefile with buildings, cases, and wards 

Use coint points in polygons to find the number of buildings (centroids) within a ward (count)

Use vector join to get cases from OPH to ward shapefile

Manually added [median household income from ONS](https://www.neighbourhoodstudy.ca/maps-2/#Economy%20&%20Employment/Household%20Income/Median%20household%20income%20(AT))

Any values <5 were changed to 1

### Spatial Statistics - COVID-19 Spread Model (weights)

*Possible Use Case*
1. Grab paramaters for whatever model we're building, Zhong in this case?
   - Maybe introuce a penalty in the SIR model for being in a poor neighbourhood
2. We could see which buildings are in a DA and connect them for population growth analysis.
   - Does the number of buildings in a DA impact population? Will likely need statistics data for this
3. Grocery stores or crimes and how they evolve through the years 

Spatial models involving the spread of COVID-19 between populations offers a unique perspective into how cases can spread from densely populated areas to less dense areas ([Eilersen, and Sneppen, 2020](https://www.medrxiv.org/content/10.1101/2020.09.04.20188359v1.full.pdf) **NOT YET PEER REVIEWED**). THERE ENDED UP BEING NO SPATIAL CORRELATION WHEN I ATTEMPTED THIS.

NEED TO VALIDATE COVID SPREAD MODEL (spatial autocorrelation to confirm spatial dependecy) https://geodacenter.github.io/workbook/5a_global_auto/lab5a.html
https://geodacenter.github.io/workbook/5b_global_adv/lab5b.html 

Do building density instead? Cases by population density ALSO isnt a good approach:
- https://www.tandfonline.com/doi/full/10.1080/01944363.2020.1777891
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7439635/
- But some sources say otherwise? https://www.medrxiv.org/content/10.1101/2020.05.28.20115626v1.full.pdf


Maybe specify that buildings arent the only factor that impact the chances of getting COVID? Population, age, income, etc. influence the vulnerability. 


## Credits and Acknowledgements

[Carleton University - ARSLab](https://arslab.sce.carleton.ca/) 

[Sarah Loos - UVic GEOG 328 GIS Analysis](https://www.uvic.ca/calendar/undergrad/#/courses/BkL7SOamV?bc=true&bcCurrent=GIS%20Analysis&bcItemType=Courses)

## Resources

### Further Reading

[Want to maybe accomplish what GeoDa did for COVID analysis?](https://github.com/GeoDaCenter/covid)

[Spatial data analysis with GIS in social sciences](http://ncgia.ucsb.edu/technical-reports/PDF/92-10.pdf)

[An ICES report stated Ontarians who tested positive for COVID were more likely to come from lower income neighbourhoods.](https://www.ices.on.ca/Publications/Atlases-and-Reports/2020/COVID-19-Laboratory-Testing-in-Ontario)

[Investigating spatiotemporal patterns of the COVID-19 in São Paulo State, Brazil](https://www.medrxiv.org/content/10.1101/2020.05.28.20115626v1.full.pdf)

[Geographically Weighted Regression (GWR) (Spatial Statistics)](https://pro.arcgis.com/en/pro-app/tool-reference/spatial-statistics/geographicallyweightedregression.htm)

[The spatial econometrics of the coronavirus pandemic](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7395580/)

[Determining the spatial effects of COVID-19 using the spatial panel data model](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7139267/)

### Video Tutorials

[Making Spatial Weights - GeoDa](https://www.youtube.com/watch?v=V_OE8Kqp1dM )

### Geospatial Analysis Program(s)

[GeoDa: ](https://geodacenter.github.io/)Spatial Data Analysis Program

[QGIS Download: ](https://www.qgis.org/en/site/)Open Source Geospatial Analysis Program

[QGIS Docs: ](https://www.qgistutorials.com/en/)Tutorials and Tips

## Appendix

### Dataset Sources


**[Ottawa Neighbourhood Study (ONS) from Carleton University](https://library.carleton.ca/find/gis/geospatial-data/ottawa-neighbourhoods)**

*Description from source:*
```
The ONS Neighbourhood Boundaries were created by the Ottawa Neighbourhood Study
(ONS) to analyse population statistics and are not indicative of actual 
neighbourhood limits.  Neighbourhood refers to an inhabited area delineated by 
social and physical boundaries. ONS neighbourhood boundaries were derived based on 
census tracts, physical and demographic similarities, physical barriers (e.g. 
waterways, highways, etc.), maps used by the real estate profession (e.g. the 
Ottawa Multiple Listing Service), consultations with community stakeholders, as 
well as fieldwork by ONS researchers.

108 neighbourhood boundaries are included in the shapefile, 
from Barrhaven to Woodvale. 
```

**[Ottawa COVID cases (2020 October 20)](https://www.arcgis.com/home/item.html?id=ae347819064d45489ed732306f959a7e)**

**[Ottawa Wards](https://open.ottawa.ca/datasets/wards?geometry=-77.634%2C44.911%2C-73.968%2C45.587)**

**[ONS COVID (2020 Oct 10)](https://www.arcgis.com/home/item.html?id=7cf545f26fb14b3f972116241e073ada)**
