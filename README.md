# How Long Will the Darkness Last? 🔦
Lakshmi Manasa Maddi and Rakshan Patnaik

## Introduction

### Dataset and Project Introduction

In this project, we are examining a dataset on the major power outages in the United States from January 2000 to July 2016. This dataset comes from https://engineering.purdue.edu/LASCI/research-data/outages/outagerisks. According to the Department of Energy, a major outage is classified to be an outage that has impacted atleast 50,000 customers or "caused an unplanned firm load loss atleast 300 MW". The data also contains geographic location, data and time, regional climate information, land-use characteristics, electricity consumption patterms, and the affected states' economic characteristics. We are interested in various relationships, such as the effect of number of affected customers, cause category, regional information, and climate anomalies on the duration of outage.

### Research Question

How do various environmental, infrastructural, and economic factors impact 

### Dataset Significance

### Dataset Summary
* Total Rows: 1,534
* Columns:


| **Column**                  | **Description**  |
|-----------------------------|------------------------------------------------------------|
| `'YEAR'`                    | Year an outage occurred |
| `'MONTH'`                   | Month an outage occurred |
| `'U.S._STATE'`              | State the outage occurred in |
| `'NERC.REGION'`             | North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| `'CLIMATE.REGION'`          | U.S. Climate regions as specified by National Centers for Environmental Information (9 Regions) |
| `'ANOMALY.LEVEL'`           | Oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season |
| `'CLIMATE.CATEGORY'`       | Climate category of the area of the outage |
| `'OUTAGE.DURATION'`       | Duration of an outage event (in minutes) |
| `'CUSTOMERS.AFFECTED'` | Number of customers affected by the outage |
| `'DEMAND.LOSS.MW'` | Amount of peak demand lost during the outage (in Megawatts) |
| `'CAUSE.CATEGORY'` | Type of event that caused the outage |
| `'HURRICANE.NAMES'` | Hurricane name (if outage is due to hurricane) |
| `'PC.REALGSP.STATE'` | Per capita gross state product (GSP) in the US state of the outage |
| `'TOTAL.PRICE'` | Total electricity consumption in the state of the outage |
| `'POPPCT_URBAN` | Percentage of total state population that is urban |
| `'POPDEN_URBAN'` | Population density of the urban areas (in persons / sq. mile) |
| `'AREAPCT_URBAN'` | Percentage of state's land area that is urban |
| `'OUTAGE.DURATION_HOURS'` | Duration of an outage event (in hours) |
| `'URBANIZATION'` | Aggregated metric for POPPCT_URBAN, POPDEN_URBAN, and AREAPCT_URBAN |


## Data Cleaning and EDA

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   DEMAND.LOSS.MW | CAUSE.CATEGORY     |   HURRICANE.NAMES |   PC.REALGSP.STATE |   TOTAL.PRICE |   POPPCT_URBAN |   POPDEN_URBAN |   AREAPCT_URBAN |   OUTAGE.DURATION_HOURS |   URBANIZATION |
|-------:|--------:|:-------------|:--------------|:-------------------|----------------:|:-------------------|------------------:|---------------------:|-----------------:|:-------------------|------------------:|-------------------:|--------------:|---------------:|---------------:|----------------:|------------------------:|---------------:|
|   2011 |       7 | Minnesota    | MRO           | East North Central |            -0.3 | normal             |              3060 |                70000 |              nan | severe weather     |               nan |              51268 |          9.28 |          73.27 |           2279 |            2.14 |              51         |        784.803 |
|   2014 |       5 | Minnesota    | MRO           | East North Central |            -0.1 | normal             |                 1 |                  nan |              nan | intentional attack |               nan |              53499 |          9.28 |          73.27 |           2279 |            2.14 |               0.0166667 |        784.803 |
|   2010 |      10 | Minnesota    | MRO           | East North Central |            -1.5 | cold               |              3000 |                70000 |              nan | severe weather     |               nan |              50447 |          8.15 |          73.27 |           2279 |            2.14 |              50         |        784.803 |
|   2012 |       6 | Minnesota    | MRO           | East North Central |            -0.1 | normal             |              2550 |                68200 |              nan | severe weather     |               nan |              51598 |          9.19 |          73.27 |           2279 |            2.14 |              42.5       |        784.803 |
|   2015 |       7 | Minnesota    | MRO           | East North Central |             1.2 | warm               |              1740 |               250000 |              250 | severe weather     |               nan |              54431 |         10.43 |          73.27 |           2279 |            2.14 |              29         |        784.803 |





