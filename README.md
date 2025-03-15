# How Long Will the Darkness Last? 🔦

Authors: Lakshmi Manasa Maddi and Rakshan Patnaik

## **Introduction**

### Dataset and Project Introduction

In this project, we analyze a dataset documenting major power outages in the United States from January 2000 to July 2016, sourced from [Purdue University’s LASCI Research Data] (https://engineering.purdue.edu/LASCI/research-data/outages). According to the U.S. Department of Energy, a major outage is classified as one that impacts at least 50,000 customers or causes an unplanned firm load loss of at least 300 MW. The dataset contains detailed information on each outage, including geographic location, date and time, climate conditions, land-use characteristics, electricity consumption patterns, and economic factors of the affected states. By analyzing these variables, we aim to uncover relationships between key factors—such as the number of affected customers, outage cause, regional climate conditions, and economic characteristics—and the duration of a power outage.

### Research Question

How do various environmental, infrastructural, and economic characteristics impact the duration of a power outage?

### Dataset Significance

Power outages can seriously affect public safety, economic output, and disaster response initiatives as well as cause disturbance of public peace. Knowing what causes extended outages helps one to plan infrastructure and be better ready. By means of high-risk identification, people and communities may foresee power interruptions and create mitigating plans. By means of these data, utility businesses and legislators can also enhance grid resilience, distribute resources effectively, and shorten recovery times, so guaranteeing a more steady and dependable power source. Examining past outage trends helps us to investigate how climate events, urbanization, and economic considerations influence outage length, therefore offering a more data-driven method of reinforcing the power infrastructure.


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
| `'POPPCT_URBAN'` | Percentage of total state population that is urban |
| `'POPDEN_URBAN'` | Population density of the urban areas (in persons / sq. mile) |
| `'AREAPCT_URBAN'` | Percentage of state's land area that is urban |
| `'OUTAGE.DURATION_HOURS'` | Duration of an outage event (in hours) |
| `'URBANIZATION'` | Aggregated metric for POPPCT_URBAN, POPDEN_URBAN, and AREAPCT_URBAN |



## **Data Cleaning and Exploratory Data Analysis**

### **Data Cleaning and Preparation**

#### 1. Removing Unnecessary Columns and Rows
We began by reading the dataset from a CSV file and identifying unnecessary columns and rows that did not contribute to the analysis. The first column was removed as it contained index-like values, and the first four rows were dropped since they contained metadata rather than actual observations. After renaming the columns using the first row of valid data, two additional rows were removed to ensure a clean structure. Finally, the index was reset to maintain a properly formatted DataFrame.

#### 2. Cleaning and Converting Numerical Columns
Next, we addressed numerical columns that were stored as strings. Certain columns contained numeric values formatted with commas or spaces, which needed to be cleaned before analysis. To ensure consistency, we stripped unnecessary spaces, removed commas, and converted these columns to numeric values using `pd.to_numeric()`. This transformation allowed for smooth calculations, visualizations, and further statistical analysis.

#### 3. Selecting Relevant Features
Once the data was structured correctly, we selected the most relevant columns for the study. The dataset was filtered to retain key attributes related to environmental, economic, and outage-related factors. This included general information such as `YEAR`, `MONTH`, and `U.S._STATE`, as well as climate-related variables like `CLIMATE.REGION`, `ANOMALY.LEVEL`, and `CLIMATE.CATEGORY`. Outage event details such as `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW` were preserved, along with economic indicators and urbanization metrics.

#### 4. Handling Missing Values
Handling missing values was a critical step in the cleaning process. Zero values in key columns, including `OUTAGE.DURATION`, `CUSTOMERS.AFFECTED`, and `DEMAND.LOSS.MW`, were replaced with `NaN` to indicate missing data more effectively. Numeric columns were also converted to ensure proper data types, and missing values in climate and economic data (`ANOMALY.LEVEL`, `PC.REALGSP.STATE`, and `TOTAL.PRICE`) were filled using median imputation to maintain a balanced dataset.

#### 5. Feature Engineering
To enhance the dataset further, we created two new features that improved interpretability and provided deeper insights:

- **Outage Duration in Hours**: Since `OUTAGE.DURATION` was originally recorded in minutes, we converted it to hours for better analysis. A new column, `OUTAGE.DURATION_HOURS`, was created by dividing the original values by 60. This made it easier to compare outage durations across different locations.

- **Urbanization Score**: Instead of using separate urbanization indicators, we created a composite metric that averaged `POPPCT_URBAN`, `POPDEN_URBAN`, and `AREAPCT_URBAN`. The new `URBANIZATION` feature provided a more comprehensive representation of urban development and its potential impact on power outages.

With these steps completed, the dataset was fully cleaned and prepared for further exploratory analysis and modeling. The final structured DataFrame is now ready for use, ensuring accuracy and consistency in all subsequent analyses.

Here is a preview of our cleaned dataset:

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     |   ANOMALY.LEVEL | CLIMATE.CATEGORY   |   OUTAGE.DURATION |   CUSTOMERS.AFFECTED |   DEMAND.LOSS.MW | CAUSE.CATEGORY     |   HURRICANE.NAMES |   PC.REALGSP.STATE |   TOTAL.PRICE |   POPPCT_URBAN |   POPDEN_URBAN |   AREAPCT_URBAN |   OUTAGE.DURATION_HOURS |   URBANIZATION |
|-------:|--------:|:-------------|:--------------|:-------------------|----------------:|:-------------------|------------------:|---------------------:|-----------------:|:-------------------|------------------:|-------------------:|--------------:|---------------:|---------------:|----------------:|------------------------:|---------------:|
|   2011 |       7 | Minnesota    | MRO           | East North Central |            -0.3 | normal             |              3060 |                70000 |              nan | severe weather     |               nan |              51268 |          9.28 |          73.27 |           2279 |            2.14 |              51         |        784.803 |
|   2014 |       5 | Minnesota    | MRO           | East North Central |            -0.1 | normal             |                 1 |                  nan |              nan | intentional attack |               nan |              53499 |          9.28 |          73.27 |           2279 |            2.14 |               0.0166667 |        784.803 |
|   2010 |      10 | Minnesota    | MRO           | East North Central |            -1.5 | cold               |              3000 |                70000 |              nan | severe weather     |               nan |              50447 |          8.15 |          73.27 |           2279 |            2.14 |              50         |        784.803 |
|   2012 |       6 | Minnesota    | MRO           | East North Central |            -0.1 | normal             |              2550 |                68200 |              nan | severe weather     |               nan |              51598 |          9.19 |          73.27 |           2279 |            2.14 |              42.5       |        784.803 |
|   2015 |       7 | Minnesota    | MRO           | East North Central |             1.2 | warm               |              1740 |               250000 |              250 | severe weather     |               nan |              54431 |         10.43 |          73.27 |           2279 |            2.14 |              29         |        784.803 |



### Univariate Analysis

<iframe
  src="assets/duration_univariate.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>

The plot below shows the distribution of power outage durations in hours. Most outages are relatively short, with the majority lasting under 50 hours, while only a few extend beyond 100 hours. The distribution is right-skewed, meaning that while long outages do happen, they are much less common. This makes sense since utility companies generally work to restore power as quickly as possible, but some severe outages can last for days due to extreme weather or infrastructure failures. Understanding this distribution helps us see what a "typical" outage looks like and highlights cases where power restoration takes much longer than usual.



### Bivariate Analysis

<iframe
  src="assets/cause_bivariate.html"
  width="100%"
  height="800"
  frameborder="0"
></iframe>

The plot below shows the relationship between outage duration and cause category. Severe weather is the most common cause of long outages, with many lasting over 200 hours. Fuel supply emergencies and islanding also tend to result in extended outages, though they occur less frequently. On the other hand, outages caused by equipment failures or system operability disruptions are generally much shorter. This suggests that some causes, like extreme weather events or fuel shortages, may be harder to resolve, while technical failures can typically be fixed more quickly. Understanding these trends can help predict how long an outage might last based on its cause.


### Interesting Aggregates

#### 1. Outage Duration by Climate Category  
One way to explore this data is by grouping outages based on their climate category (cold, warm, normal) and calculating the average outage duration. This helps us determine if climate plays a role in how long outages last.  



| CLIMATE.CATEGORY   |   OUTAGE.DURATION |
|:-------------------|------------------:|
| cold               |           2901.35 |
| normal             |           2666.11 |
| warm               |           2837.37 |


<iframe
  src="assets/duration_interesting.html"
  width="900"
  height="600"
  frameborder="0"
></iframe>



#### Explanation  
From this table, we can see how climate conditions correlate with average outage duration. If cold climates have significantly longer outages than warm climates, it could suggest that extreme winter conditions make it harder to restore power.  


<br>

#### 2. Outage Duration and Demand Loss by NERC Region  
Next, we explore NERC Regions (North American Electric Reliability Corporation regions), which are responsible for grid reliability across different parts of the country. We calculate the mean outage duration and demand loss (in megawatts) per region. 



| NERC.REGION   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |
|:--------------|------------------:|-----------------:|
| ASCC          |           nan     |           35     |
| ECAR          |          5603.31  |         1314.48  |
| FRCC          |          4271.12  |         1072.6   |
| FRCC, SERC    |           372     |          nan     |
| HECO          |           895.333 |          466.667 |



#### Explanation  
This table reveals which regions tend to have longer outages and higher demand losses. For example, if some regions experience much longer outages than others, this could indicate differences in infrastructure resilience, emergency response efficiency, or climate conditions.  

<br>

#### 3. Cause of Outages by Climate Category  
Finally, we analyze outage causes across climate categories. We create a pivot table that shows the average impact of different outage causes (e.g., equipment failure, severe weather, intentional attack, etc.) in each climate type.  





| CLIMATE.CATEGORY   |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| cold               |              327.5  |                17433    |              709.537 |     259.267 |        2125.91  |          3293.79 |                         637.265 |
| normal             |             3201.43 |                 7658.82 |              505.442 |     142.176 |        1376.53  |          4082.53 |                         941.018 |
| warm               |              505    |                22799.7  |              317.767 |     209.833 |         596.231 |          4416.69 |                         494.69  |



#### Explanation  
This table helps us understand what causes the most severe outages in each climate type. If severe weather is the biggest factor in cold climates, while equipment failure dominates warm climates, it suggests different mitigation strategies might be needed for different regions.  


## **Assessment of Missingness**

### NMAR Analysis

In our dataset, we noticed that some values in the OUTAGE.DURATION column are missing, and it doesn’t seem random. Specifically, it looks like longer outages are more likely to have missing duration values, which suggests that the missingness is Not Missing at Random (NMAR).

This makes sense because shorter outages are easier to track—there’s a clear start and end time. But for longer outages, especially those caused by major weather events like hurricanes or wildfires, restoration happens in stages, and there might not be a single timestamp marking when power was fully restored everywhere. Since the likelihood of missing data seems to depend on the duration itself, it points to NMAR missingness.

To figure out if something else is causing this pattern, we’d need more data. For example, if some utility companies have way more missing values than others, it might mean that different companies record outage data differently rather than the missingness actually being tied to outage length. If we had access to details on which companies reported each outage and how they track restorations, we could check if the missingness is actually Missing at Random (MAR) instead of NMAR.
 

### Missingness Dependency Analysis

In this section, we analyzed the missingness dependency of the **CUSTOMERS.AFFECTED** column on the **ANOMALY.LEVEL** and **OUTAGE.DURATION_HOURS** columns.

#### No Dependency Column: ANOMALY.LEVEL

First, let’s look at the distribution of **ANOMALY.LEVEL** when **CUSTOMERS.AFFECTED** is missing versus not missing.

**Null Hypothesis:** The distribution of **ANOMALY.LEVEL** when **CUSTOMERS.AFFECTED** is missing is the same as when **CUSTOMERS.AFFECTED** is not missing.  

**Alternate Hypothesis:** The distribution of **ANOMALY.LEVEL** when **CUSTOMERS.AFFECTED** is missing differs from when **CUSTOMERS.AFFECTED** is not missing.  

**Test Statistic:** Difference of Means  

<iframe
  src="assets/anomalylevel_missingness1.html"
  width="1050"
  height="650"
  frameborder="0"
></iframe>
#### Results

- **Observed Test Statistic:** 0.0484  
- **P-value:** After performing 10,000 permutations, we got a p-value of **0.2098**.  

<iframe
  src="assets/anomalylevel_missingness2.html"
  width="1050"
  height="650"
  frameborder="0"
></iframe>

**Conclusion:** At a significance level of **0.05**, we **fail to reject the null hypothesis**. This result indicates that the missingness in the **CUSTOMERS.AFFECTED** column has **no dependency** on the **ANOMALY.LEVEL** column.

---

#### Dependency Column: OUTAGE.DURATION_HOURS

First, let’s look at the distribution of **OUTAGE.DURATION_HOURS** when **CUSTOMERS.AFFECTED** is missing versus not missing.

**Null Hypothesis:** The distribution of **OUTAGE.DURATION_HOURS** when **CUSTOMERS.AFFECTED** is missing is the same as when **CUSTOMERS.AFFECTED** is not missing.  

**Alternate Hypothesis:** The distribution of **OUTAGE.DURATION_HOURS** when **CUSTOMERS.AFFECTED** is missing differs from when **CUSTOMERS.AFFECTED** is not missing.  

**Test Statistic:** Difference of Means  

<iframe
  src="assets/duration_missingness1.html"
  width="1050"
  height="650"
  frameborder="0"
></iframe>

#### Results

- **Observed Test Statistic:** 24.12  
- **P-value:** After performing 10,000 permutations, we got a p-value of **0.0**.  

<iframe
  src="assets/duration_missingness2.html"
  width="1050"
  height="650"
  frameborder="0"
></iframe>

**Conclusion:** At a significance level of **0.05**, we **reject the null hypothesis**. This result indicates that the missingness in the **CUSTOMERS.AFFECTED** column **is dependent** on the **OUTAGE.DURATION_HOURS** column.


## **Hypothesis Testing**: 

### Do Power Outages Last Longer in Cold Climates?

In this analysis, we explore whether power outages tend to last longer in **cold climates** compared to **warm climates**. Understanding how climate affects outage duration is important for improving disaster response strategies and strengthening infrastructure resilience.

### Hypothesis Statement

- **Null Hypothesis (H₀):** On average, power outages in cold climates last the same amount of time as those in warm climates.  
- **Alternative Hypothesis (H₁):** On average, power outages in cold climates last longer than those in warm climates.

### How did we test this?

To see if outage durations are actually different across climate conditions or just random chance, we ran a **permutation test with 10,000 simulations**. This test helps us figure out whether the differences we see in outage duration between climates are meaningful or just happen by luck.

We used the difference in mean outage durations between cold and warm climates as our test statistic. We set our significance level (α) at 0.05, which means that if there's less than a 5% chance of seeing our result just by random chance, we'll reject the idea that climate has no impact on outage duration.

### Results

- **Observed Difference in Means (Cold - Warm):** -2.6727
- **P-value:** 0.6460

The histogram below shows the distribution of simulated test statistics under the null hypothesis. The **red vertical line** represents the actual observed difference, while the **blue shaded area** highlights extreme values that contribute to the p-value.

<iframe
  src="assets/climate_hypothesistest.html"
  width="1000"
  height="600"
  frameborder="0"
></iframe>

### Conclusion

After conducting the permutation test, the **p-value obtained was 0.6620, which is much higher than our chosen significance level of 0.05**. Thus, we **fail to reject the null hypothesis**. There is insufficient evidence to conclude that power outages in cold climates tend to last longer than power outages in warm climates.


## **Framing a Prediction Problem**

### Problem Statement
In this project, we’re trying to **predict how severe a power outage** will be based on different factors like environmental conditions, economic indicators, and outage-related data. Since there are multiple levels of severity, this is a multiclass classification problem where we categorize outages based on how long they last. To do this, we divide **OUTAGE.DURATION_HOURS** into four categories: Short, Moderate, Long, and Severe. Each power outage is assigned one of these labels, allowing us to classify events based on their severity. This makes it easier to analyze and predict how long an outage might last in the future.

### Response Variable
Our response variable is the **outage severity level**, which comes from the **OUTAGE.DURATION_HOURS** column. We chose this variable because predicting outage severity is a key factor in emergency response and disaster planning. If we can estimate how long an outage will last, utility companies and emergency teams can allocate resources more efficiently, prioritize repairs, and minimize disruptions for affected communities.

### Evaluation Metric
- **Chosen Metric:** **F1-score**
Not all power outages are the same—some only last a few hours, while others can go on for days. Since some outage severities are likely to happen more often than others, accuracy alone wouldn’t give us a clear picture of how well the model is performing. The F1-score is a better choice because it balances precision and recall, making sure the model doesn’t just focus on the most common outage types but actually performs well across all severity levels

### Features Used for Prediction
To make predictions, we use the following features:

1. **CAUSE.CATEGORY** – What caused the outage (e.g., severe weather, equipment failure).  
2. **NERC.REGION** – The geographical region responsible for managing the grid.  
3. **CUSTOMERS.AFFECTED** – The number of people impacted by the outage.  
4. **CLIMATE.CATEGORY** – The climate classification of the affected area.  
5. **PC.REALGSP.STATE** – The per capita real gross state product (a measure of economic strength).  
6. **URBANIZATION** – A metric that captures urban population density and land area.  

### Why These Features?
The key to building a useful prediction model is ensuring that all the input features are available at the time of prediction. That means we’re using factors that can be observed immediately when an outage begins, rather than anything that would only be known after the fact (such as how long it actually lasted). By considering climate, economic conditions, and infrastructure factors, this model offers a data-driven way to predict outage severity in real-time. 


## **Baseline Model**

#### Model Description
For our baseline model, we developed a multiclass classification model to predict the severity of power outages. The response variable, **OUTAGE.CATEGORY**, categorizes outages into four severity levels: **Short, Moderate, Long, and Severe**. The model was implemented using a **Random Forest Classifier** within an **sklearn pipeline** to streamline preprocessing and training.

#### Features Used
Our model utilizes both **quantitative** and **categorical** features to make predictions.

##### Quantitative Features:
- **CUSTOMERS.AFFECTED** (numerical): The number of customers impacted by the outage.
- **PC.REALGSP.STATE** (numerical): The per capita real gross state product, representing economic activity.
- **URBANIZATION** (numerical, derived feature): The level of urbanization, calculated as the mean of three urbanization metrics.

##### Categorical Features:
- **CAUSE.CATEGORY** (nominal, one-hot encoded): The high-level category explaining why the outage occurred (e.g., severe weather, equipment failure).
- **NERC.REGION** (nominal, one-hot encoded): The North American Electric Reliability Corporation (NERC) region where the outage took place.
- **CLIMATE.CATEGORY** (nominal, one-hot encoded): The climate classification (cold, warm, or normal) of the region affected by the outage.

#### Feature Engineering
- **One-hot encoding** was applied to categorical variables (**CAUSE.CATEGORY, NERC.REGION, and CLIMATE.CATEGORY**) to transform them into a format suitable for machine learning models.
- **Missing values** in numeric features were imputed using the median value, while categorical features were filled with a placeholder category ("missing").
- **Scaling** was performed on numerical features using **StandardScaler** to normalize them before model training.

---

### **Model Performance**
The baseline model was evaluated using **accuracy** as the primary metric, achieving a score of **53.42%**. Additionally, the classification report provides further insight into the model’s effectiveness:

- **Precision and Recall**: The model performed best at predicting **Severe** outages, with a **precision of 0.64** and **recall of 0.75**. However, performance for **Long** and **Moderate** outages was significantly lower, indicating room for improvement.
- **Macro Average F1-Score**: **0.45**, suggesting that overall performance varies across categories.
- **Weighted Average F1-Score**: **0.52**, reflecting the imbalance in class distribution.

Despite the moderate accuracy, the model struggles with classifying **Long** and **Moderate** outages, with **precision and recall both below 0.25** for these categories. Additionally, there were 6 missing values in X_train and 3 in X_test, suggesting that better handling of missing data may improve results.

### Is the Baseline Model Good?
Overall, this baseline model is not very strong. While it shows some success in predicting Severe outages, it performs poorly on other outage types and has clear imbalances in precision and recall. This indicates that improvements—such as better handling of missing data, using more advanced modeling techniques, or adjusting for class imbalance—are needed to make it a more reliable predictor.


## **Final Model**


#### Feature Engineering
To help our model better predict power outage severity, we added two new features that highlight important patterns in the data.

One of the biggest changes we made was applying a log transformation to the **CUSTOMERS.AFFECTED** column. Outages can impact anywhere from a few hundred to millions of people, and this huge difference makes it harder for the model to learn effectively. Without adjustments, really large values could dominate the model, making it less accurate overall. By using a log transformation, we scaled down extreme values while still keeping smaller values meaningful, helping the model recognize patterns without getting thrown off by really large numbers.

We also added polynomial features for **PC.REALGSP.STATE**, which represents a state’s economic activity. A state's economy might play a role in how quickly power is restored—richer states might have better infrastructure and emergency response, while lower-income areas might struggle with longer outages. By adding **quadratic terms**, we gave the model a chance to capture more complex patterns in the data, making it better at predicting outage duration based on economic conditions..  

#### Modeling Approach and Why We Used It
We decided to stick with the **Random Forest Classifier** as our final model because it performed well in our baseline tests and works great with both numerical and categorical data. To make sure all the preprocessing steps were applied correctly, we used an **sklearn pipeline**. This helped us handle things like feature transformations, scaling, and encoding in a way that kept everything consistent and avoided data leakage. By setting it up this way, we made sure the model always gets properly processed data whenever it’s trained. 

#### Hyperparameter Tuning
To make our final model as effective as possible, we used **GridSearchCV** to systematically test different values for key hyperparameters. These included:  

- **`n_estimators` (number of trees in the forest):** We tested values to find the right balance between model complexity and performance. The best value was **50**.  
- **`max_depth` (maximum depth of each tree):** A deeper tree can capture more patterns but may overfit. We found **10** to be the best balance.  
- **`min_samples_split` (minimum samples required to split a node):** This controls how deep the tree grows. We found **2** worked best.  
- **`min_samples_leaf` (minimum samples required at a leaf node):** This prevents the model from overfitting to small patterns. The best value was **4**.  

We selected these values by running GridSearchCV with **cross-validation (`cv=3`)**, meaning the model was tested on different subsets of the training data to ensure the best parameters were chosen fairly.  

#### Did the Final Model Improve Over the Baseline? 
Yes! Our final model performed **better than the baseline model** in accuracy.  

| Metric            | Baseline Model | Final Model | Improvement |
|------------------|---------------|-------------|-------------|
| **Accuracy**      | 53.42%        | **56.35%**  | +2.93%      |
| **Macro F1-Score** | 0.45          | **0.43**    | -0.02       |
| **Weighted F1-Score** | 0.52      | **0.51**    | -0.01       |


This improvement means that our model is now better at correctly predicting the **severity level of an outage**, particularly for **underrepresented categories like Moderate and Long outages**. The addition of new features helped the model recognize patterns it previously missed, while hyperparameter tuning ensured that it wasn’t overfitting or underfitting.  

#### Why the Final Model Improved
The **log transformation on `CUSTOMERS.AFFECTED`** helped the model **handle extreme values** more effectively, making it easier to generalize across different types of outages. The **polynomial transformation of `PC.REALGSP.STATE`** allowed the model to capture **nonlinear relationships** between economic factors and outage duration. Finally, fine-tuning our hyperparameters ensured that our Random Forest Classifier was **optimized for performance**, leading to a more reliable model overall.  

---

### **Confusion Matrix Code for Final Model**  
To better understand how our model is making predictions, we use a **confusion matrix**, which shows where the model is getting predictions right and where it's making mistakes.  

<iframe
  src="assets/confusion_final.html"
  width="1150"
  height="750"
  frameborder="0"
></iframe>

## **Fairness Analysis**

### Group Definitions
To evaluate the fairness of our power outage prediction model, we examined whether its performance differs based on urbanization levels. We defined two groups:

- **Group X (High Urbanization):** Outages occurring in states where more than 75% of the population lives in urban areas.
- **Group Y (Low Urbanization):** Outages occurring in states where 75% or less of the population lives in urban areas.

We chose urbanization as a fairness criterion because infrastructure in highly urbanized areas is expected to be more resilient due to better maintenance and resource allocation. If the model is biased, it might perform worse in rural areas where outages tend to be longer and more severe.

### Evaluation Metric
We used **accuracy** as the evaluation metric, as our model predicts the duration severity of power outages, making accuracy a natural choice to measure its correctness across different groups.

### Hypotheses
- **Null Hypothesis (H₀):** The model is fair, meaning its accuracy for states with high and low urbanization is roughly the same, and any observed differences are due to random chance.
- **Alternative Hypothesis (H₁):** The model is unfair, meaning its accuracy for states with low urbanization is significantly lower than for states with high urbanization.

### Test Statistic
The test statistic used in this analysis is the **difference in accuracy** between Group X (high urbanization) and Group Y (low urbanization).

### Significance Level
We set the significance level at **0.05**.

### Results
- **Observed Test Statistic:** The observed difference in mean prediction accuracy between the two groups was **-0.122**, indicating that the model performed slightly worse for low-urbanization states.
- **p-value:** After conducting a permutation test with **10,000 iterations**, we obtained a p-value of **0.3928**.

### Conclusion
- **Decision:** Since the p-value (**0.3928**) is greater than the significance level (**0.05**), we **fail to reject the null hypothesis**.
- **Interpretation:** This result suggests that any observed differences in accuracy between high-urbanization areas (Group X) and low-urbanization areas (Group Y) are likely due to random chance. Therefore, we do not have strong evidence to conclude that the model is unfair with respect to urbanization levels.

The histogram below illustrates the distribution of test statistics obtained from the permutation test. The red dashed line represents the observed test statistic of **-0.037**, showing that it falls well within the range of random variations.

<iframe
  src="assets/parabola_fairness.html"
  width="1050"
  height="650"
  frameborder="0"
></iframe>
