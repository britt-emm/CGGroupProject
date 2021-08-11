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
- **Logistic Regression**
  
  Logistic regression is a type of regression when there are only two target values but the target values have order such as 0 and 1. The values do not have to actually be 0 and 1, but the values are evaluated to compare to 0 and 1 so values up to 0.5 are considered 0 and values 0.5 and above are considered 1.
    - Used Logistic Regression on two different resampling strategies with two different data scales
      - Resampling strategies
        1. Random over sampling and clustered centroid majority reduction
        2. SMOTEEN - synthetic minority samples and edited nearest neighbor majority class reduction
      - Train, test split - 75/25%
      - Best model: Grid Search to identify parameters
        - used in conjucntion with normed data and SMOTEEN resampling
        - Train data balanced accuracy:  79%
        - Test data balanced accuracy:  74%
        - Model recall score: 86%
        - Test recall score:  84%
        - Slightly overfitted
    - The notebook can be found here : <a href="https://github.com/britt-emm/CGGroupProject/blob/main/Final_Project/Logistic_Regression_Keras_model.ipynb"> Logistic_Regression_Keras_model</a>

  
  
- **Random Forest Classifier**

  Random Forest is a model that builds off the decision tree model. It introduces randomness into the model by having multiple trees that work using a sample of the full dataset that was sampled with replacement (bootstrapping). It also introduces randomness during splitting by either splitting based on all input features or by a random selection of max_features specified when building the model. The increased randomness helps to decrease variance, although it increases bias (tendency to learn the wrong things by not looking at all of the information). Random Forest does not require data to be scaled or normalized
  - Used GridSearch to identify the best hyperparameters
  - The best performing model using Random Forest Classifier:
    - Had a 60/40 train test split
    - Used Random oversampling
    - gini impurity to measure the quality of the split
    - max_depth for each tree was 6
    - log2 of n_features to determine max_features used in each tree
    - n_estimators = 300 for the total trees in the forest
    - min_samples_leaf =.05 for the minimum number of samples at each leaf node
    - This notebook can be found here: <a href="https://github.com/britt-emm/CGGroupProject/blob/main/Final_Project/raw_randomForest.ipynb">raw_RandomForest</a>

- **Neural Network- Keras**
  
  Keras is a neural network, deep learning machine learning model.  We had hoped Keras might be able to more accurately model any non-linear relationships wher we could drive our false negative rate down while keeping false positives better than other linear models.   The Keras binary classification output needed to be converted to 0 or 1 with the stroke data. Description of the better performing Keras model:
  - Used normalized, but no resampling data.  
  - Adjust class weights when initializing the model.
  - Binary crossentropy as the loss.
  - Used two layers in keras, the first with 50 nodes, the second with 25.  More nodes did not equate to better performance.
  - Withheld test data and used cross validation
  - Test data balanced accuracy:  65%
  - Test recall score:  53%
  - The notebook can be found here : <a href="https://github.com/britt-emm/CGGroupProject/blob/main/Final_Project/Logistic_Regression_Keras_model.ipynb"> Logistic_Regression_Keras_model</a>

- **Support Vector Machine (SVM)**
  
  SVM tries to find the maximum distance between target values creating a decision boundary but it is not limited to 3-dimensional space to do so. The number of features determines what plane of space the boundary lies in. Support vectors influence this decision boundary. Support vectors are a sample of the training data that are used to help influence the decision boundary.
  - Used normalized data
  - Resampled using SMOTEEN and SVMSMOTE
    - SVMSMOTE incorporates the SVM algorithm instead of using k-nearest neighbors.
    - Random state for SMOTEEN and SVMSMOTE is 101.
    - Best performing model was using SVM with SVMSMOTE.
  - SVC kernel is set to 'linear' and class_weight is set to 'balanced'.
  - Test data balanced accuracy score is 76%
  - The notebook can be found here : 
### General Findings
Our best models had higher recall for participants with stroke. Although the precision is low, we believe that it is more important to have higher recall (higher true positives) with more false positives than to have a large number of false negatives. The use case for these models would be to identify participants/patients who would be at a higher risk for stroke. It would be more concerning to miss people who could be counseled on reducing their risk of stroke than to counsel people on reducing risk who don't have as high of risk.
The best performing models were the Logistic Regression, Random Forest Classifier, and Support Vector Machines models.
### ETL 
We completed ETL of the data creating two tables with a shared ID. The tables included demographic and medical data. This file can be found here: <a href="https://github.com/britt-emm/CGGroupProject/blob/main/Final_Project/stroke_ETL.ipynb">stroke_ETL</a>
