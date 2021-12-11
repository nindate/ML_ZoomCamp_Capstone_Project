## MLZoomCamp Capstone Project

# **Building & Deploying a Python ML Model for Motor Vehicle Collision Prediction**

## Introduction:

Motor Vehicle collisions occur when a vehicle collides with another vehicle, pedestrian, animal or any other object on the road. Such collisions often result in injury, disability, death, damage to life as well property. This project is based on collision accident between a person and a motor vehicle that occurred in New York city during 2021. The data was sourced from [NYC Open Data](https://opendata.cityofnewyork.us/). The purpose of this project was to understand issues like - what is the frequency and type of injuries in such collisions; which are the most common factors influencing such motor vehicle collisions in New York etc. I studied a publicly available dataset on Kaggle and built a model for predicting collision incidents using Supervised Machine learning techniques in Python.

Firstly, I prepared my data by cleaning and formatting the dataset. Then, I used Exploratory Data Analysis techniques in Python to visualize relationships between different features in dataset like BODILY_INJURY, SAFETY_EQUIPMENT, EJECTION, EMOTIONAL_STATUS and their relation with the target variable, PERSON_INJURY. After cleaning, I pre-processed my dataset to train multiple Supervised Machine Learning models by splitting the data into subsets, identifying & segregating the feature matrix from the target variable and one-hot encoding of categorical variables. I used multiple Binary Classification models from scikit-learn library in Python to train my dataset and make predictions. I used both Regression-based & Tree-based models. Each model was evaluated using classification metrics AUC score and their performances compared. Thereafter the models were tuned using different parameters to find the most optimal parameters. Lastly, multiple final models with optimal parameters were run to select the Best Model for our NYC Motor Vehicle Collisions dataset. For each algorithms used, I also analyzed which were the most important features influencing the predictions made about the target variable, PERSON_INJURY.   

Once the best model for my dataset was selected its code was exported from a Python Notebook into a Python script. Thereafter, this model was put into a web service using Flask. Then, a Python virtual environment was created using Pipenv containing all the necessary dependencies and packages for running the model. The model was first deployed locally using Docker as a containerized service. Then, this locally deployed model was used to predict the incidence of collision injury for a new 'sample collided person' with unseen data as input. Finally, our deployed model gave as output, details regarding the risk of collision_injury (as True or False) and probability of collision_injury for a 'sample collided person' as inputs to the model. 

Lastly, I also tried deploying the collision injury prediction service to the cloud using AWS Elastic Beanstalk. This cloud environment created for the collision injury prediction service was then used to make predictions about a new 'sample collided person' as input to our model.




## Data Sources and Preparation: 

### Sources of Data -

For this project, I retrieved data from Kaggle Public Dataset - [NYC Motor Vehicle Collisions to Person](https://www.kaggle.com/kukuroo3/nyc-motor-vehicle-collisions-to-person) Data was shared on Kaggle as a csv file (*NYC_Motor_Vehicle_Collisions_to_Person.csv*).

### Data Processing Steps - 

Following steps were used in preparing the data for this project:-

* I processed the dataset using Python in-built functions and libraries such as **Pandas** for manipulation and structuring them as DataFrames.**Numpy** functions along with statistical analysis techniques like mean used to fill for missing values in the dataset. 
* Once the data was cleaned & formatted it was used to perform **Exploratory Data Analysis (EDA)** and create a series of visualizations for drawing insights.
* Different categorical features in the dataset like PERSON_TYPE, COMPLAINT, PERSON_SEX etc. were converted into dictionaries using one-hot encoder **DictVectorizer**.
* **Correlation** among categorical features was computed to understand their relation with target variable, PERSON_INJURY using Chi-square test.
* **Feature Importance** techniques were implemented for different ML algorithms to understand which features affected predictions about PERSON_INJURY variable the most.
* **Binary Classification (Supervised) ML algorithms** both regression-based (like *Logistic Regression*) and tree-based (like *Decision Trees*) were used from **scikit-learn** library to train and make predictions. **Ensemble methods** using bagging like *Random Forest* and boosting like *eXtreme Gradient Boosting or XGBoost* were also used. 
Models were compared and their performance evaluated using metrics like - roc_auc_score, f1_score, classification_report etc. 
* **GridSearchCV** parameter-tuning techniques were used to tune the parameters of each model, improve their performance and select the most optimal parameters.
* **Cross-Validation** technique like **K-Fold** was also used to split the dataset into ‘k’ number of subsets, then use k-1 subsets to train the model and use the last subset as a validation set to test the model. Then the score of the model on each fold was averaged to evaluate the performance of the model.
* **CatBoost for Classification** algorithm was used as an additional exercise in this project, to explore more from this dataset. CatBoost or **Categorical Boosting** is an open-source boosting library developed by Yandex. The greatest advantage of CatBoost is that it automatically handles categorical features, using Ordered target statistics. As this dataset mostly has categorical features, making predictions using CatBoost and tuning its parameters acted as a good experiment to learn.
* After evaluating multiple models the Best Model was chosen as **Random Forest for Classification**. Hereafter, this best model was used (as a Python script) in a Web service (using **Flask**) and deployed locally (using **Docker**).
* Lastly, the Collision Injury Prediction service, was also deployed in the cloud using **AWS Elastic Beanstalk**. For this, Elastic Beanstalk command-line interface (CLI) was added as a development dependency (only for the project). Then an environment for the collision injury prediction service was created in AWS which successfully launched the environment and provided a public url to reach the application. This url was finally used to make collision injury predictions about details of the new 'sample collided person' as input. 




## Exploratory Data Analysis (EDA), Correlation & Feature Importance: 


Name of the Python Notebook - ***ML_ZoomCamp_Capstone_Project_NYC_MV_Collisions.ipynb***

Below are the important components from the Python Notebook used to load data, perform EDA, train multiple models and evaluate them to select the best model:-
* **Data Loading** - Firstly, I loaded all the basic libraries used in the project. Then, I imported the data from the .csv file into a Pandas DataFrame and got a snapshot of data (like exploring the unique values in each column of dataset, getting a statistical summary of dataset).

![image](https://user-images.githubusercontent.com/50409210/145678001-3c5a240a-f94e-4a5c-92e3-d9204ccf1ced.png)

This dataset mostly had categorical features (as shown below) and the target variable was PERSON_INJURY' with primarily binary outcomes 'Injured' and 'Killed'. There were 2-3 time-series related features (like CRASH_DATE & CRASH_TIME) as well.  

![image](https://user-images.githubusercontent.com/50409210/145683746-7a173296-60b7-448d-a9b6-f5cf2d9b5a32.png)


*  **Data Cleaning and Formatting** - I cleaned the dataset by taking the following steps:-
   a) Removing Irrelevant data or Columns - like dropping columns VEHICLE_ID, PERSON_ID, UNIQUE_ID, COLLISION_ID
   b) Imputing Missing rows with mean or mode of values - like imputing missing PERSON_AGE with mean PERSON_AGE
   c) Changing Data Type for Columns - like changing CRASH_DATE to datetime format or PERSON_AGE to integers
   d) Replacing Column Values with Specific Values - like replacing 'Does Not Apply' values with 'Unknown'
   e) Feature Creation from existing feature columns - like creating CRASH_Mnth_Name column from CRASH_DATE column to extract Months from Dates


* **Exploratory Data Analysis (EDA)** - After this, I performed EDA using Python libraries like Matplotlib, Pandas and Seaborn to analyze the data and visualize its key components. Using multiple visuals and subplots I tried to answer important questions like ***When do the most traffic accidents occur in NYC during 2021?***
      
![image](https://user-images.githubusercontent.com/50409210/145678529-b005475b-e4bf-4c51-83f9-b509865b68ba.png)

As my dataset contained more categorical variables countplot was the primary plot used for most visualizations to understand the relation of different features like EJECTION, PERSON_TYPE etc. with the target variable, PERSON_INJURY. For time-series specific columns like CRASH_Mnth_Name and CRASH_TIME I used barplots to plot the count of Hourly Crashes and Monthly Collisions in 2021. PERSON_AGE column was also analyzed with other features using barplots to study, ***Which age and gender people were involved in most of the NYC collisions?*** or ***Which were most common type of BODILY_INJURY and COMPLAINT faced by those injured after the collision?***

![image](https://user-images.githubusercontent.com/50409210/145678877-e601148b-d9a7-48be-9e81-33fa352a20f9.png)

The following **interesting insights** were drawn from these plots - 
* The month of June appeared to have the most number of crashes on NYC roads during 2021, while November had the least.
* Also, most number of collisions in 2021 on NYC roads, seem to have happened around 16:00 or 4:00 pm in the evening.
* In thw NYC collisions more Females were killed than Males. Also, the proportion of Females injured were quite similar to that of injured Males.
* The count of collisions for different types of Ejection shows that most of the people were Injured and that too because they could Not Eject. Also, of those injured in collisions most people were Occupants of the motor vehicle themselves or Pedestrians.
* Most Females killed were in age group 50-60 years while, most Males killed were in age group 40-50 years.

![image](https://user-images.githubusercontent.com/50409210/145679010-49818c6e-ccb2-4dd9-b6ed-6f11bc50a730.png)

* The Pedestrian Location of most of the people injured in NYC collisions in 2021 were not Known (Unknown). Most of the injured people were in Conscious state and in most cases those injured were in the role of Driver at the time of collision.
* The injured faced mostly Back, Knee-Lower Leg Foot and Neck injuries after collision. Those injured most often complained about Pain or Nausea after meeting with accident.
 
 ![image](https://user-images.githubusercontent.com/50409210/145683308-a596322c-fddb-46b4-a908-fcd15bb49c59.png)

* Mostly the injured persons during collision were at Driver's position in the vehicle. The Lap Belt & Harness were used as Safety Equipment by most injured during the NYC Collisions in 2021. This is ironical as it shows that, people got injured even after using safety equiments. However, the second highest of those injured were those about whom the use of Safety Equipment was Unknown.


Hereafter, I used Chi-square test to compute the Correlation between each of the Categorical Features ('BODILY_INJURY','CONTRIBUTING_FACTOR_2', 'EMOTIONAL_STATUS', 'PERSON_SEX' etc.) and the target variable, PERSON_INJURY. As only one feature PERSON_AGE was numerical in our dataset we did not find its correlation with our target separately.
The following **insights** were drawn from them -
* Only 'CONTRIBUTING_FACTOR_2' feature had a p-value > 0.5 hence, accepting our Null Hypothesis we found that only this feature is not correlated to our target variable, PERSON_INJURY.
* For all other features, their respective p-value < 0.5 so, we rejected the Null Hypothesis and declared that they are correlated with our target variable.


* **One-hot Encoding of Categorical Data Using DictVectorizer** - As this dataset contained mostly all categorical feature like PERSON_TYPE, COMPLAINT, EMOTIONAL_STATUS etc. these were encoded using DictVectorizer before being used further in training ML algorithms and making predictions. DictVectorizer helped in transforming lists of feature-value mappings to vectors i.e., feature matrix into dictionaries for training and predicting on subsets. When feature values were strings, this transformer would do a binary one-hot coding. 


* **Feature Importance Using Mutual Information Score** - To understand the importance of fetaures in dataset Mutual Information metric was computed for different features with the PERSON_INJURY variable. It was found that the knowledge about EMOTIONAL_STATUS will be the most certain while knowledge about CONTRIBUTING_FACTOR_2 will be the least certain in giving information about our target variable PERSON_INJURY. I also computed the important features in the dataset for each of the models Decision tree, Random Forest and XGBoost so as to identify how they differed among models.

![image](https://user-images.githubusercontent.com/50409210/145687426-0856a1ca-1c30-44ab-b7d9-440b9eb0e5be.png)

* **Computing Difference & Risk Ratio for Features** - Risk Ratios or relative risk, is a metric that measures the risk-taking place in a particular group and comparing the results with the risk-taking place in another group. Here it helped in finding interesting facts about those injured or killed in NYC collisions - like people with Lap Belt & Harness as safety equipment, Occupants of vehicles, those in Driver seat or those who could Not Eject were more likely to get injured. Most people post collision Complained of Pain or Nausea, were Conscious. These facts would further help us find categories or variables which would make predictions about Person's Injury status after a collision using ML algorithms.



## Model Selection and Tuning & Evaluation:


* **Setting up the validation framework** - Firstly, I split the dataset into training, validation and testing subsets in the ratio of 60:20:20. Then, I defined the feature matrix containing all the factor columns and defined the 'PERSON_INJURY' column as the target variable. I also ensured that the target column was removed from each of the 3 data subsets.

* **Model Selection & Evaluation** - Once the data was split and pre-processed for machine learning algorithms I implemented different models by training them on the full_train set and made predictions on the validation set. The models were then evaluated using Classification metrics like roc_auc_score, confusion_matrix, classification_report etc. to compare their performances. 

Following were the different modelling algorithms I used in this project:

      * Logistic Regression
      * Decision Trees for Classification
      * Random Forest for Classification (using Ensemble learning, bagging)
      * XGBoost for Classification (using Gradient boosting)
      * Additional exploration - using CatBoost or Categorical Bossting  for Classification 
      
For each of the model feature importance was computed to identify which features contributed most to the predictions about collision-related injuries. AUC scores were computed both for the training set & the validation set separately to compare model's performance.

![image](https://user-images.githubusercontent.com/50409210/145686737-8c15f03d-ffbc-48c1-8321-5151036101c2.png)

* **Parameter Tuning of Models** - The parameters for each of the above models were also tuned using **Grid Search Cross Validation (GridSearchCV)** to find the most optimal parameters giving the following as outputs:
   a) .best_params_ - gives the best combination of tuned hyperparameters (Parameter setting that gave the best results on the hold out data)
   b) .best_score_ - gives the Mean cross-validated score of the best_estimator 
   c) .best_estimator_ - estimator which gave highest score (or smallest loss if specified) on the left out data
   
Following were the parameters tuned for each model.
     * Decision Trees for Classification - max_depth , min_samples_leaf and max_features 
     * Random Forest for Classification - n_estimators, max_depth, min_samples_leaf, max_features
     * XGBoost for Classification - eta, max_depth, min_child_weight
     * CatBoost for Classification - learning_rate, max_depth
 
After tuning each model with different parameters the most optimal parameters were selected for the model. This became the Final Model after Hyperparameter Tuning yielding the best AUC score:-

    * Final Decision Tree Model - max_depth = 5, min_sample_leaf = 20 and max_features = 10
    * Final Random Forest Model - max_depth = 15, min_sample_leaf = 1, and n_estimators = 70 and max_features = 8
    * Final XGBoost Model -(training for 200 iterations) - max_depth = 4, min_child_weight = 1 and eta = 0.5
    * Final CatBoost Model - max_depth = 5 and learning_rate = 0.5

* **Selecting the Best Model** - Once final models were built next step was choosing the Best Model among Final Decision tree, Random forest and XGBoost for Classification models. This was done by evaluating each of the final models on the validation set and comparing the AUC scores. By doing so, I found that, ***Random Forest for Classification*** model gave the best AUC score on validation set hence, it was selected as the **Best Model for NYC MV Collision Prediction dataset**. 

Thereafter, I used this best model to make predictions on the testing set (unseen data). Here also it performed fairly close to the validation set scores. Finally, this best model was saved as a Python script and used for further deployment as a web service.

