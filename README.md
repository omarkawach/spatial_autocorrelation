# Spatial Autocorrelation

### Purpose

Review the Zhong model maybe? 

### Background

Questions to answer
1. Data manipulation
   - Process, project, etc.
2. spatial data statistical analysis
3. spatial modelling
4. visualizing 
  - Spatial autocorrelation follows toplers law
    - Spatial data = spatial autocorrelation
    - introduces redundancy in data, affects the outcome of statistical tests and correlation coefficient
      - CC = relationship between two values (or multiple locations) using Moran's I
        - Check to see if patterns dont occur just by chance, accept or reject hypothesis, show the distribution is not random (cause and effect in data), p-value, z-score
      - Moran's I to assess spatial autocorrelation (most people dont bother since data came as they were)
        - where n is the number of regions or spatial units, with a weighting matrix (w)
        - can incorporate contiguity, adjacency, and distance as well as attribute data (concepts of topology)
        - between -1 and +1
          - +ve is similar values found together
          - -ve is disisimilar values found together (rare)
          - 0 means dist is **random** and no spatial autocorrelation
      - Look at dispersed (negative I) versus clustered (positive I)
      - Measure signifigance of Moran's I
        - Look at p-values (prob. spatial autocorrelation is correlated, look for low value )and z-score (absolute value means your data is likely to spatially autocorrelated, look for high value, stdev of mean)
      - Distance decay
        - Moran's I decreases as more observations (farther away) are included in the calculations, therefore less influence
    - Use for finding hotspots (maybe like concencration of retired people, where are they in a population)
    - Most stats assume data is random, but not when it comes to spatial data
  - Map clusters (density, identify hotspots (of covid))
  - Spatial relationships (weights matrix, GWR)
  - nearest neighbour, inverse distance, and classifcations are not geostatistics
  - spatiotemporal = space and time
  - [First law of geography](https://wiki.gis.com/wiki/index.php/First_law_of_geography) 

Data issues with spatial dependce (how data relates in space)
- MAUP (Modifiable Areal Unit Problem)
  - Location of boundaries used to aggregate data can influence results of statistical tests (Moran's I)
  - How boundaries impact summary statistics (look at stdev most importantly)
  - Gerrymandering is a good example of this (larger population decides the election)
  - Difficult to get proportional representation (redrawing district lines), lopsided representation, political polarization
- Scale can cause problems 
  - Data can become homogenous, impacts autocorrelation 
- Ecological fallacy like MAUP
  - Individuals vs Popuplations 
  - Cant take aggregated results and apply them to individuals
  - statistical result change based on data aggregation
  - Provincial average income versus municipal average income (cant assume an individual makes provincial avg if they live in Toronto)
- Boundary Problem
  - Segments data
  - when data excluded due to boudnary, lose of information that influences the data that remains (we could pull things out of context)
  - can impact statistical tests
  - Best practice to keep SOME surrounding data

Limitations
- GWR https://pro.arcgis.com/en/pro-app/tool-reference/spatial-statistics/geographicallyweightedregression.htm 

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7395580/ 

https://www.youtube.com/watch?v=V_OE8Kqp1dM 

https://github.com/GeoDaCenter/covid 

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7139267/ 

DOES THE NEIGHBORING MAKE SENSE??? YOU CANT HAVE ISLANDS SINCE THERE ARE NO NEIGHBOURS. YOU HAVE TO FIX THAT SOMEHOW

LOWER THE WEIGHT OF THINGS FARTHER AWAY IF YOU DO DISTANCE (DISTANCE BASED WEIGHT) - TOPPLERS LAW

ROW STANDARDIZATION WEIGHTS BETWEEN 0 AND 1, INTERPRETATION AS AVERAGE OF NEIGHBORS
- instead of contiguity matrix (zeroes and ones if neighbors), it uses weights matrix
  - you want to do this most of the time 
    - you want the sum of all the weights to equal one 
      - ex. three neighbours to one and divide by 3
      - ex. 5 neighbours divide 1 by five
    - gives each area we're interested in, and equal weight
      - each give us an equal amount to what we're computing
        - spatial correlating or regression

LAG MODEL - spatial lag / autoregressive model SAR myth paper

https://www.mdpi.com/2225-1146/2/4/217/htm

Moran's I? What do the values mean? Maybe see how population impacts the number of neighbors you have? Check out space in geoda and click univariate morans i or do the local one to see potentially statistically signifcant areas
- lisa cluster map
- lisa signifigance map

WEIGHT MATRICES FOR SPATIAL CORRELATION (shouldnt have large impact)
- QUEEN
  - shared boudnary or shared corner
- ROOK
  - only shared boudnary 
- DISTANCE
  - neighbors within distance (by row standardization)
  - know if lat/lon or cartesian first 
  - lon/lat
    - use arc distance (over surface of sphere)
  - cartesian 
    - use euclidiean 
- INVERSE DISTANCE
  - closest in some sense by strongest neighbour 
    - could be by non spatial aspects 
  - 
- KERNEL 
- K-NEAREST neighbor (knn)
  - x nearest neighbours 
  - based on centroids and euclidian distance

PRESCISION FRESHOLD
- when two shapes meet at a point, how precise do you want the point to be

SYMMETRY
- ASYMETRIC
- NON ASYMETRIC

ORDER OF CONTIGUITY
- 1
  - ONLY IF YOU TOUCH ON A BORDER OR CORNER DIRECTLY WILL U BE A NEIGHBOR
- 2
  - NEIGHBOR OF A NEIGHBOR IS ALSO A NEIGHBOR OF MINE LOCATION -> NEIGHBOR -> NEIGHBORS NEIGHBOR
  - Here is where you should maybe select include lower orders

What is spatial lag? https://libraries.mit.edu/files/gis/regression_presentation_iap2013.pdf 
- Spatial autocorrelation http://ncgia.ucsb.edu/technical-reports/PDF/92-10.pdf

- Not that it wont always be just 6 or 12 neighbours

We have a shapefile containing the population and homes in hundreds of DAs.
- Create knn weights k=6 and create an inverse as well https://geodacenter.github.io/workbook/4b_dist_weights/lab4b.html#creating-knn-weights 
- Then do one with epanechnikov kernel weight
- create spatially lagged variable
  - spatial lag in table calculator 
  - try with k6 first
    - spatial lag with row standardized weights
      - k6_sc_lag = avg houses of 6 neighbors = (x1 + x2 ... x6) / 6 with row standardized weights
    - spatial lag as a sum of neighboring values
      - k6_sc_lag2 = sum of pop of six neighbors
    - spatial window average
      - k6_sc_lag3 = the observation itself and the average so divided by 7 instead
    - spatial window sum
      - without dividing by 7

### Spatial Statistics - COVID-19 Spread Model (weights)

Grocery stores or crimes and then use as parameter for model and how it will evolve through the years

Since the inception of TFL, researchers in the GIS community have employed such a concept to describe spatial dependence ([Leitner et al., 2018](https://www.researchgate.net/publication/323419139_Laws_of_Geography)). In the field of epidemiology, one could apply TFL to synthetically simulate the spread of infectious diseases in a geographical environment based on spatial weighting ([Zhong et al., 2009](https://www.researchgate.net/profile/Song_Dunjiang/publication/226204125_Simulation_of_the_spread_of_infectious_diseases_in_a_geographical_environment/links/00b495316b307a20ab000000/Simulation-of-the-spread-of-infectious-diseases-in-a-geographical-environment.pdf)). Such an application can play a vital role in disease prevention and control when coupled with modern spatio-temporal analysis techniques ([Watkins et al., 2007](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1805744/)). 

The recent COVID-19 outbreak has made it apparent how unprepared governments are for a global pandemic of this scale ([Timmis, and Brussow, 2020](https://sfamjournals.onlinelibrary.wiley.com/doi/10.1111/1462-2920.15029)). Matters are made worse by the fact that large, and even small-scale problems are difficult for humans to conceptualize. This is especially true when we consider global issues like global warming ([Resnik et al., 2016](https://onlinelibrary.wiley.com/doi/full/10.1111/cogs.12388)). Given the unprecedented amount of data surrounding the ongoing pandemic, local / national / global real-time, non-real-time, or simulated disease cases must be carefully analyzed to recognize high risk geographical regions which may be susceptible to outbreaks or further disease spreading. 


Spatial models involving the spread of COVID-19 between populations offers a unique perspective into how cases can spread from densely populated areas to less dense areas ([Eilersen, and Sneppen, 2020](https://www.medrxiv.org/content/10.1101/2020.09.04.20188359v1.full.pdf) **NOT YET PEER REVIEWED**). In **Table 1**, the data retrieved from the City of Ottawa reveals the number of cumulative COVID-19 cases by ward as of September 25, 2020. The COVID-19 dataset from the City of Ottawa did not provide population statistics, so it had to be added manually by spatial joining data from an Ottawa DA shapefile. 

Do building density instead? Cases by population density isnt a good approach:
- https://www.tandfonline.com/doi/full/10.1080/01944363.2020.1777891
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7439635/
- But some sources say otherwise? https://www.medrxiv.org/content/10.1101/2020.05.28.20115626v1.full.pdf


Begin by collecting COVID data from Ottawa Public Health (Oct 2020)
https://www.arcgis.com/home/item.html?id=7cf545f26fb14b3f972116241e073ada#overview



Get ward shapefile from open data 

Get building footprints for Ottawa

Create new shapefile with buildings, cases, and wards 

Use coint points in polygons to find the number of buildings (centroids) within a ward (count)

Use vector join to get cases from OPH to ward shapefile

Manually added median household income https://www.neighbourhoodstudy.ca/maps-2/#Economy%20&%20Employment/Household%20Income/Median%20household%20income%20(AT)

Case data kept: 
- Ottawa COV: Cumulative rate (per 100 000 population), excluding cases linked to outbreaks in LTCH and RH – cumulative number of residents with confirmed COVID-19 in a Ward, excluding those linked to outbreaks in LTCH and RH, divided by the total population of that Ward
- Ottawa C_1: Cumulative number of cases, excluding cases linked to outbreaks in LTCH and RH - cumulative number of residents with confirmed COVID-19 in a Ward, excluding cases linked to outbreaks in LTCH and RH
- Ottawa C_2: Cumulative number of cases linked to outbreaks in LTCH and RH - Number of residents with confirmed COVID-19 linked to an outbreak in a long-term care home or retirement home by Ward

Maybe specify that buildings arent the only factor that impact the chances of getting COVID. Population, age, income, etc. influence the vulnerability. 

Anything <5 changed to 1

**Table 1**. cumulative  COVID-19 Cases as Reported on September 25 (maybe use more up to date info later)

<img src="" alt="cases_by_ward" width="420" height="450" />

**Note**: Exlucdes retirement home and longterm care home cases

To better visualize the data from **Table 1**, the number of cases per capita (*Cumu_cases / Population*) was plotted onto a map using quantile classification (6 classes).


![]()

**Figure 1**. COVID-19 cases per capita 

Before thematically identifying which wards are at a high risk of disease case spillover (USE A BETTER WORD), a Queen matrix was applied to the Ottawa Wards shapefile to find each ward's neighbours by shared border and corners. 

![]()

**Figure 2**. Osgoode Ward and it's Neighbours  

A spatial lag calculator with row-standardized weights would give every ward an equal weight since the Queen matrix would be fractional instead of zeroes and ones (contiguity). Using the spatial lag calculator, one could sum the number of cases in each neighbouring ward and then divide by the number of neighboring wards (*Queen * (Cumu_cases / Population)*). Plotting this result shows the wards at a high risk of disease spillover (USE A BETTER WORD) from neighbouring wards. 


![](g)
**Figure 3**. Quantile Classification of Wards at Risk of Disease Spillover (USE A BETTER WORD)

NEED TO VALIDATE COVID SPREAD MODEL (spatial autocorrelation to confirm spatial dependecy) https://geodacenter.github.io/workbook/5a_global_auto/lab5a.html
https://geodacenter.github.io/workbook/5b_global_adv/lab5b.html 

- Queen weight using ward#
- Click univariate local moran's I and select cummulative covid cases

Local indicators of spatial association (LISA) for clustering

LISA CLUSTER MAP (where the concentration of covid cases are, therefore hot spots and cold spots

Maybe only consider high high?

spatial clusters
- high high (red)
  - high cases within itself and surrounding areas
- low low (blue)
  - low cases when surrounding areas have more cases

spatial outliers 
- high low (light red) 
  - high cases when surrounding areas have low cases
- low high (light blue)

![]()


LISA SIGNIFIGANCE MAP (see how confident we can be in our clusters, darkest values are most confident to show this relationship wasnt found by chance)
![]()

Moran's I scatterplot (draws the cluster map)
![]()



https://www.medrxiv.org/content/10.1101/2020.05.28.20115626v1.full.pdf

*Possible Use Case*
1. We could see which buildings are in a DA and connect them for population growth analysis.
   - Does the number of buildings in a DA impact population? Will likely need statistics data for this