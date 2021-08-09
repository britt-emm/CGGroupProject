# Stroke Dataset Project 
### Amanda Matheu, Brittany Minor, Yewande Taiwo

### The Dataset
Our dataset comes from Kaggle and includes 5110 rows.
https://www.kaggle.com/fedesoriano/stroke-prediction-dataset

### Preprocessing
- Removing outliers
  - People under age 25
  - One gender listed as “Other”
- Deal with BMI null variables
  - Because our data was so imbalanced we felt it was important to keep as many data points as possible in the minority class (had strokes). Since the age range of participants was from 25-82, we thought it might be better to use mean BMI values by age group to replace missing values rather than the mena BMI for the entire data set. Age groups were split into 10 year bins, before calculating mean BMI.
  - We did drows that were missing BMI and did not have a stroke.
- Encoding: LabelEncoder and One-hot Encoding
  - We used LabelEncoder for the following variables:
    - Gender
    - Ever_married
    - Residence_type
  - We used One_hot encoding for the following variables because they were not binary and we wanted to make sure that no option was weighted more than another option:
    - Work_type
    - Smoking_status
- Normalizing or scaling
  - We ended up with 3 datasets that we used for modeling: a raw dataset, a normalized dataset (done with MinMaxScaler) and a standardized dataset (done with StandardScaler)
- Over and under-sampling
  - SMOTEN, Cluster Centroids, Random oversampling, Random undersampling, SMOTENC 
  - We tried out a variety of different resampling techniques to address our imbalanced dataset

### Models used: 
- Logistic Regression
- Random Forest Classifier
- Neural Network- Keras
- Support Vector Machine (SVM) 

### General Findings
Our best models had higher recall for participants with stroke. Although the precision is low, we believe that it is more important to have higher recall (higher true positives) with more false positives than to have a large number of false negatives. The use case for these models would be to identify participants/patients who would be at a higher risk for stroke. It would be more concerning to miss people who could be counseled on reducing their risk of stroke than to counsel people on reducing risk who don't have as high of risk.
The best performing models were the Logistic Regression, and Random Forest Classifier models.
### ETL 
We completed ETL of the data creating two tables with a shared ID. The tables included demographic and medical data.