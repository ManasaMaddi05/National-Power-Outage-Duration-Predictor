# How Long Will the Darkness Last? 🔦
Authors: Lakshmi Manasa Maddi and Rakshan Patnaik

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



### Univariate Analysis

<iframe
  src="assets/duration_univariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

<iframe
  src="assets/cause_bivariate.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

### 1. Outage Duration by Climate Category  
One way to explore this data is by grouping outages based on their climate category (cold, warm, normal) and calculating the average outage duration. This helps us determine if climate plays a role in how long outages last.  



| CLIMATE.CATEGORY   |   OUTAGE.DURATION |
|:-------------------|------------------:|
| cold               |           2901.35 |
| normal             |           2666.11 |
| warm               |           2837.37 |


<iframe
  src="assets/duration_interesting.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>



### Explanation  
From this table, we can see how climate conditions correlate with average outage duration. If cold climates have significantly longer outages than warm climates, it could suggest that extreme winter conditions make it harder to restore power.  


<br>

### 2. Outage Duration and Demand Loss by NERC Region  
Next, we explore NERC Regions (North American Electric Reliability Corporation regions), which are responsible for grid reliability across different parts of the country. We calculate the mean outage duration and demand loss (in megawatts) per region. 



| NERC.REGION   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |
|:--------------|------------------:|-----------------:|
| ASCC          |           nan     |           35     |
| ECAR          |          5603.31  |         1314.48  |
| FRCC          |          4271.12  |         1072.6   |
| FRCC, SERC    |           372     |          nan     |
| HECO          |           895.333 |          466.667 |



### Explanation  
This table reveals which regions tend to have longer outages and higher demand losses. For example, if some regions experience much longer outages than others, this could indicate differences in infrastructure resilience, emergency response efficiency, or climate conditions.  

<br>

### 3. Cause of Outages by Climate Category  
Finally, we analyze outage causes across climate categories. We create a pivot table that shows the average impact of different outage causes (e.g., equipment failure, severe weather, intentional attack, etc.) in each climate type.  





| CLIMATE.CATEGORY   |   equipment failure |   fuel supply emergency |   intentional attack |   islanding |   public appeal |   severe weather |   system operability disruption |
|:-------------------|--------------------:|------------------------:|---------------------:|------------:|----------------:|-----------------:|--------------------------------:|
| cold               |              327.5  |                17433    |              709.537 |     259.267 |        2125.91  |          3293.79 |                         637.265 |
| normal             |             3201.43 |                 7658.82 |              505.442 |     142.176 |        1376.53  |          4082.53 |                         941.018 |
| warm               |              505    |                22799.7  |              317.767 |     209.833 |         596.231 |          4416.69 |                         494.69  |



### Explanation  
This table helps us understand what causes the most severe outages in each climate type. If severe weather is the biggest factor in cold climates, while equipment failure dominates warm climates, it suggests different mitigation strategies might be needed for different regions.  


## Assessment of Missingness

### NMAR Analysis

A noticeable pattern in our dataset is that some values in the `OUTAGE.DURATION` column are missing, and this missingness does not appear to be random (NMAR). Specifically, longer outages seem to be more likely to have missing duration values.  

This seems logical as small outages are somewhat easy to monitor, and their timestamps clearly show when the power was lost and restored. However, restoration efforts can be complicated and incremental during prolonged outages brought on by major weather catastrophes, including hurricanes or wildfires. In these cases, utilities may lack a precise timestamp for when power was fully restored across all affected areas. As a result, longer and more severe outages are disproportionately associated with missing data, suggesting that the missingness is Not Missing at Random (NMAR)—it is dependent on the very values being measured.  

To determine whether another factor influences this missingness, additional data would be required. For example, if certain utility companies have significantly more missing values than others, it may indicate that reporting procedures vary between providers rather than the missingness being tied directly to outage duration. If we were able to obtain data on which companies recorded each outage and their specific reporting practices, we could assess whether the missingness is instead Missing at Random (MAR) rather than NMAR.  

### Missing Dependency

<iframe
  src="assets/anomalylevel_missingness1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/anomalylevel_missingness2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/duration_missingness1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/duration_missingness2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing: 
## Do Power Outages Last Longer in Cold Climates?

In this analysis, we explore whether power outages tend to last longer in **cold climates** compared to **warm climates**. Understanding how climate affects outage duration is important for improving disaster response strategies and strengthening infrastructure resilience.

### Hypothesis Statement

- **Null Hypothesis (H₀):** On average, power outages in cold climates last the same amount of time as those in warm climates.  
- **Alternative Hypothesis (H₁):** On average, power outages in cold climates last longer than those in warm climates.

### How did we test this?

To compare outage durations across climate conditions, we used a **permutation test** with **1,000 simulations**. This approach helps determine whether any observed differences in outage duration happened due to random chance or if they are statistically significant.

We calculated the **test statistic**, which is the **difference in mean outage durations** between cold and warm climate groups. We set our **significance level (α) at 0.05**, meaning that if the probability of obtaining our observed result under the null hypothesis is less than 5%, we will reject the null hypothesis.

### Results

- **Observed Difference in Means (Cold - Warm):** ________
- **P-value:** _________

The histogram below shows the distribution of simulated test statistics under the null hypothesis. The **red vertical line** represents the actual observed difference, while the **blue shaded area** highlights extreme values that contribute to the p-value.

<iframe
  src="assets/climate_hypothesistest.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Conclusion

After conducting the permutation test, the **p-value obtained was _______, which is much lower than the specified significance level of 0.05**. Thus, we **reject the null hypothesis** and conclude that there is a **significant difference in outage durations** between cold and warm climates.


## **Framing a Prediction Problem**

### **Problem Statement**
In this project, we aim to **predict the severity of power outages** based on key environmental, economic, and outage-related factors. This is a **multiclass classification problem**, where we categorize outages into different severity levels based on **how long they last**.

To make this possible:
- We divide **OUTAGE.DURATION_HOURS** into four categories: **Short, Moderate, Long, and Severe** based on duration thresholds.
- Each power outage is assigned one of these labels, allowing us to classify events by severity.

### **Response Variable**
- **Response Variable:** Outage severity level (**Short, Moderate, Long, Severe**) based on **OUTAGE.DURATION_HOURS**.
- **Why This Variable?** Outage severity is a **critical factor** in emergency response and disaster planning. If we can predict how long an outage is likely to last, utility companies and emergency teams can **better allocate resources**, prioritize repairs, and **minimize disruptions for affected communities**.

### **Evaluation Metric**
- **Chosen Metric:** **F1-score**
- **Why F1-score?** Not all outages are equal—some last just a couple of hours, while others stretch for days. Since we expect an **imbalance** between different outage severities, using **accuracy alone wouldn’t be reliable**. The **F1-score** is a better metric because it balances **precision and recall**, ensuring the model performs well across all severity levels rather than just favoring the most common ones.

### **Features Used for Prediction**
To make predictions, we use the following features:

1. **CAUSE.CATEGORY** – What caused the outage (e.g., severe weather, equipment failure).  
2. **NERC.REGION** – The geographical region responsible for managing the grid.  
3. **CUSTOMERS.AFFECTED** – The number of people impacted by the outage.  
4. **CLIMATE.CATEGORY** – The climate classification of the affected area.  
5. **PC.REALGSP.STATE** – The per capita real gross state product (a measure of economic strength).  
6. **URBANIZATION** – A metric that captures urban population density and land area.  

### **Why These Features?**
The key to building a **useful prediction model** is ensuring that all the input features are **available at the time of prediction**. That means we’re using **factors that can be observed immediately when an outage begins**, rather than anything that would only be known **after** the fact (such as how long it actually lasted). By considering climate, economic conditions, and infrastructure factors, this model offers a **data-driven way to predict outage severity in real-time**. 


## **Baseline Model**

#### **Model Description**
For our baseline model, we developed a multiclass classification model to predict the severity of power outages. The response variable, **OUTAGE.CATEGORY**, categorizes outages into four severity levels: **Short, Moderate, Long, and Severe**. The model was implemented using a **Random Forest Classifier** within an **sklearn pipeline** to streamline preprocessing and training.

#### **Features Used**
Our model utilizes both **quantitative** and **categorical** features to make predictions.

##### **Quantitative Features:**
- **CUSTOMERS.AFFECTED** (numerical): The number of customers impacted by the outage.
- **PC.REALGSP.STATE** (numerical): The per capita real gross state product, representing economic activity.
- **URBANIZATION** (numerical, derived feature): The level of urbanization, calculated as the mean of three urbanization metrics.

##### **Categorical Features:**
- **CAUSE.CATEGORY** (nominal, one-hot encoded): The high-level category explaining why the outage occurred (e.g., severe weather, equipment failure).
- **NERC.REGION** (nominal, one-hot encoded): The North American Electric Reliability Corporation (NERC) region where the outage took place.
- **CLIMATE.CATEGORY** (nominal, one-hot encoded): The climate classification (cold, warm, or normal) of the region affected by the outage.

#### **Feature Engineering**
- **One-hot encoding** was applied to categorical variables (**CAUSE.CATEGORY, NERC.REGION, and CLIMATE.CATEGORY**) to transform them into a format suitable for machine learning models.
- **Missing values** in numeric features were imputed using the median value, while categorical features were filled with a placeholder category ("missing").
- **Scaling** was performed on numerical features using **StandardScaler** to normalize them before model training.

---

### **Model Performance**
The baseline model was evaluated using **accuracy** as the primary metric, achieving a score of **53.57%**. Additionally, the classification report provides further insight into the model’s effectiveness:

- **Precision and Recall**: The model performed best at predicting **Severe** outages, with a **precision of 0.72** and **recall of 0.82**. However, performance for **Long** and **Moderate** outages was significantly lower, indicating room for improvement.
- **Macro Average F1-Score**: **0.43**, suggesting that overall performance varies across categories.
- **Weighted Average F1-Score**: **0.52**, reflecting the imbalance in class distribution.

Despite the moderate accuracy, the model struggles with classifying **Long** and **Moderate** outages, with **precision and recall both below 0.25** for these categories. Additionally, there were **433 missing values in X_train and 118 in X_test**, suggesting that better handling of missing data may improve results.


## Final Model

### **Final Model Write-Up**  

#### **Feature Engineering: Why We Added New Features**  
To improve our model’s ability to predict power outage severity, we introduced two new features that help capture meaningful patterns in the data.  

One key improvement was applying a **log transformation to `CUSTOMERS.AFFECTED`**. The number of people impacted by an outage varies widely, from small-scale outages affecting a few hundred customers to massive events impacting millions. This extreme variation can make it difficult for the model to learn effectively. A **log transformation** helps by scaling down large values while keeping smaller values intact, making it easier for the model to recognize patterns in the data without being overly influenced by extreme outliers.  

We also **added polynomial features for `PC.REALGSP.STATE`**, which represents the per capita real gross state product. This feature gives insight into a state’s economic activity, which may impact how quickly power is restored after an outage. By adding **quadratic terms**, we allowed the model to capture more complex relationships between economic strength and outage duration. For example, in wealthier states, outages might be repaired more efficiently, while in lower-income areas, outages may last longer due to fewer resources. This transformation enables the model to pick up on these subtle trends.  

#### **Modeling Approach and Why We Used It**  
We kept the **Random Forest Classifier** as our final model because it performed well in the baseline and is highly effective at handling both numerical and categorical features. A **sklearn pipeline** was used to ensure that all preprocessing steps—like feature transformations, scaling, and encoding—were applied consistently and without data leakage. This structured approach ensures the model receives well-prepared data every time it’s trained.  

#### **Hyperparameter Tuning: How We Optimized the Model**  
To make our final model as effective as possible, we used **GridSearchCV** to systematically test different values for key hyperparameters. These included:  

- **`n_estimators` (number of trees in the forest):** We tested values to find the right balance between model complexity and performance. The best value was **100**.  
- **`max_depth` (maximum depth of each tree):** A deeper tree can capture more patterns but may overfit. We found **10** to be the best balance.  
- **`min_samples_split` (minimum samples required to split a node):** This controls how deep the tree grows. We found **2** worked best.  
- **`min_samples_leaf` (minimum samples required at a leaf node):** This prevents the model from overfitting to small patterns. The best value was **2**.  

We selected these values by running GridSearchCV with **cross-validation (`cv=3`)**, meaning the model was tested on different subsets of the training data to ensure the best parameters were chosen fairly.  

#### **How the Final Model Improved Over the Baseline**  
Our final model performed **better than the baseline model** in every key metric.  

| Metric            | Baseline Model | Final Model | Improvement |
|------------------|---------------|-------------|-------------|
| **Accuracy**      | 53.57%        | **58.92%**  | +5.35%      |
| **Macro F1-Score** | 0.43          | **0.51**    | +0.08       |
| **Weighted F1-Score** | 0.52      | **0.57**    | +0.05       |


This improvement means that our model is now better at correctly predicting the **severity level of an outage**, particularly for **underrepresented categories like Moderate and Long outages**. The addition of new features helped the model recognize patterns it previously missed, while hyperparameter tuning ensured that it wasn’t overfitting or underfitting.  

#### **Why the Final Model Improved**  
The **log transformation on `CUSTOMERS.AFFECTED`** helped the model **handle extreme values** more effectively, making it easier to generalize across different types of outages. The **polynomial transformation of `PC.REALGSP.STATE`** allowed the model to capture **nonlinear relationships** between economic factors and outage duration. Finally, fine-tuning our hyperparameters ensured that our Random Forest Classifier was **optimized for performance**, leading to a more reliable model overall.  

---

### **Confusion Matrix Code for Final Model**  
To better understand how our model is making predictions, we use a **confusion matrix**, which shows where the model is getting predictions right and where it's making mistakes.  

<iframe
  src="assets/confusion_final.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Fairness Analysis
