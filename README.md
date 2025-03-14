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

## Data Cleaning and Exploratory Data Analysis

### Steps of Data Cleaning

### **Data Cleaning and Preparation**

#### **1. Removing Unnecessary Columns and Rows**
We began by reading the dataset from a CSV file and identifying unnecessary columns and rows that did not contribute to the analysis. The first column was removed as it contained index-like values, and the first four rows were dropped since they contained metadata rather than actual observations. After renaming the columns using the first row of valid data, two additional rows were removed to ensure a clean structure. Finally, the index was reset to maintain a properly formatted DataFrame.

#### **2. Cleaning and Converting Numerical Columns**
Next, we addressed numerical columns that were stored as strings. Certain columns contained numeric values formatted with commas or spaces, which needed to be cleaned before analysis. To ensure consistency, we stripped unnecessary spaces, removed commas, and converted these columns to numeric values using `pd.to_numeric()`. This transformation allowed for smooth calculations, visualizations, and further statistical analysis.

#### **3. Selecting Relevant Features**
Once the data was structured correctly, we selected the most relevant columns for the study. The dataset was filtered to retain key attributes related to environmental, economic, and outage-related factors. This included general information such as `YEAR`, `MONTH`, and `U.S._STATE`, as well as climate-related variables like `CLIMATE.REGION`, `ANOMALY.LEVEL`, and `CLIMATE.CATEGORY`. Outage event details such as `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` were preserved, along with economic indicators and urbanization metrics.

#### **4. Handling Missing Values**
Handling missing values was a critical step in the cleaning process. Zero values in key columns, including `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW`, were replaced with `NaN` to indicate missing data more effectively. Numeric columns were also converted to ensure proper data types, and missing values in climate and economic data (`ANOMALY.LEVEL`, `PC.REALGSP.STATE`, and `TOTAL.PRICE`) were filled using median imputation to maintain a balanced dataset.

#### **5. Feature Engineering**
To enhance the dataset further, we created two new features that improved interpretability and provided deeper insights:

- **Outage Duration in Hours**: Since `OUTAGE.DURATION` was originally recorded in minutes, we converted it to hours for better analysis. A new column, `OUTAGE.DURATION_HOURS`, was created by dividing the original values by 60. This made it easier to compare outage durations across different locations.

- **Urbanization Score**: Instead of using separate urbanization indicators, we created a composite metric that averaged `POPPCT_URBAN`, `POPDEN_URBAN`, and `AREAPCT_URBAN`. The new `URBANIZATION` feature provided a more comprehensive representation of urban development and its potential impact on power outages.

With these steps completed, the dataset was fully cleaned and prepared for further exploratory analysis and modeling. The final structured DataFrame is now ready for use, ensuring accuracy and consistency in all subsequent analyses.

```python
df.head()
 

Here are the first five rows of our cleaned dataframe:

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   DEMAND.LOSS.MW | CAUSE.CATEGORY     |   HURRICANE.NAMES |   PC.REALGSP.STATE |   TOTAL.PRICE |   POPPCT_URBAN |   POPDEN_URBAN |   AREAPCT_URBAN |   OUTAGE.DURATION_HOURS |   URBANIZATION |
|-------:|--------:|:-------------|:--------------|:-------------------|----------------:|:-------------------|------------------:|---------------------:|-----------------:|:-------------------|------------------:|-------------------:|--------------:|---------------:|---------------:|----------------:|------------------------:|---------------:|
|   2011 |       7 | Minnesota    | MRO           | East North Central |            -0.3 | normal             |              3060 |                70000 |              nan | severe weather     |               nan |              51268 |          9.28 |          73.27 |           2279 |            2.14 |              51         |        784.803 |
|   2014 |       5 | Minnesota    | MRO           | East North Central |            -0.1 | normal             |                 1 |                  nan |              nan | intentional attack |               nan |              53499 |          9.28 |          73.27 |           2279 |            2.14 |               0.0166667 |        784.803 |
|   2010 |      10 | Minnesota    | MRO           | East North Central |            -1.5 | cold               |              3000 |                70000 |              nan | severe weather     |               nan |              50447 |          8.15 |          73.27 |           2279 |            2.14 |              50         |        784.803 |
|   2012 |       6 | Minnesota    | MRO           | East North Central |            -0.1 | normal             |              2550 |                68200 |              nan | severe weather     |               nan |              51598 |          9.19 |          73.27 |           2279 |            2.14 |              42.5       |        784.803 |
|   2015 |       7 | Minnesota    | MRO           | East North Central |             1.2 | warm               |              1740 |               250000 |              250 | severe weather     |               nan |              54431 |         10.43 |          73.27 |           2279 |            2.14 |              29         |        784.803 |


### Univariate Analysis

By implementing these steps, we refined our dataset to improve the accuracy and interpretability of our analysis.


### Bivariate Analysis


### Interesting Aggregates


