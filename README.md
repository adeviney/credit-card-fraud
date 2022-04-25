# How to Retain Customers and Influence People
**Author:** Alexis Deviney

## Overview
This goal of this project is to recommend strategies to minimize customer churn (or service termination) from a fictional telecommunications company. In my analysis, I answered the following questions:
1. What customer demographics have the highest proportion of customer churn?
2. Is service/product engagement predictable of churn? Which products, when opted into, confer a lower probability of a customer to churn?
3. What services are the most profitable?
4. What is the tenure of customers who decide to churn? Are new or old customers churning?
6. Are longer contracts conducive to customer retention?

## Business Problem
Attracting new customers can be up to 5x more costly than retaining existing customers. 
Predict behavior to retain customers at Telco telecommunications company. Analyze all relevant customer data to help develop focused customer retention programs. 

## Data
I analyzed a data consisting of information on 7,043 unique customers and information relating to their demographics, account history, and services.

### Data Sources
Cognos Analytics Sample Datasets
* [Customer Churn](https://community.ibm.com/accelerators/catalog/content/Customer-churn)

* [Telco Customer Churn](https://community.ibm.com/accelerators/catalog/content/Telco-customer-churn)

* [Telco Customer Churn Status and Reason for Leaving](https://community.ibm.com/accelerators/catalog/content/Telco-customer-churn-status-and-reason-for-leaving)

### Variables and Engineered Features

Variables given in datasets:

|Variable Name|Description|
|---|---|
|CustomerID|Unique ID identifying customer|
|Gender|Customer's gender, Male or Female|
|SeniorCitizen|Indicates whether the customer is 65 years or older, Yes or No|
|Partner|Indicates whether the customer has a partner, Yes or No|
|Tenure|Total number of months the customer has been with the company|
|Latitude|The latitude of the customer's residence|
|Longitude|The longitude of the customer's residence|
|Zip Code|The zip code of the customer's residence|
|PhoneService|Indicates if the customer is subscribed to phone service with the company, Yes or No|
|MultipleLines|Indicates if  the customer has multiple phone lines on their phone service, Yes or No|
|InternetService|Indicates if the customer is subscribed to internet service with the company, Yes or No|
|OnlineSecurity|Indicates if the customer subscribes to an additional online security service provided by the company, Yes or No|
|OnlineBackup|Indicates if the customer subscribes to an additional online backup service provided by the company, Yes or No|
|DeviceProtection|Indicates if the customer subscribes to additional device protection service provided by the company, Yes or No|
|TechSupport|Indicates if the customer subscribes to a premium tech support service provided by the company, Yes or No|
|StreamingTV|Indicates if the customer uses their Internet service to stream TV, Yes or No|
|StreamingMovies|Indicates if the customer uses their Internet service to stream movies, Yes or No|
|Contract|Indicates the customer's contract type with the company: Month-to-month, One-year, or Two-year|
|PaperlessBilling|Indicates if the customer has opted into paperless billing, Yes or No|
|PaymentMethod|Indicates how the customer pays their bill: Electronic check, mailed check, automatic bank transfer, or automatic credit card|
|MonthlyCharges|Customer's current monthly charges for services provided by the company|
|TotalCharges|Sum of customer's charges to-date|
|Churn|Yes = The customer left the company this quarter. No = The customer remained with the company|
|Churn Reason|A customer's reason for leaving the company, selected from a list of structured options at service termination|

**Engineered Features**:

* Packages
  - Consolidates different combinations of services to the following 8 packages: 

|Package Name|Description|
|---|---|
|Phone Only|Subscription to phone service, unsubscribed to multiple lines or Internet service.|
|Phone Plus|Subscription to phone service + multiple lines. No Internet service.|
|DSL Only|Subscription to DSL Internet service. No phone service or premium service opt-in.|
|DSL Plus|Subscription to DSL Internet service with one or more premium services. No phone service.|
|DSL Bundle|Subscription to DSL Internet service and phone service (undifferentiated by Multiple Lines). No premium services.|
|DSL Bundle Plus|Subscription to DSL Internet service, phone services, and one or more premium services.|
|Fiber Optic Bundle*|Subscription to fiber optic Internet service and phone service. No premium services.|
|Fiber Optic Bundle Plus*|Subscription to fiber optic Internet service, phone service, and one or more premium services.|
\* Please note that Telco requires bundling fiber optic Internet with phone service.

* Churn Category
  - Groups 20 different given churn reasons into common themes:

|Churn Reason|Assigned Category|
|---|---|
|Attitude of support person|Attitude|
|Competitor offered higher download speeds|Competitor|
|Competitor offered more data|Competitor|
|Don't know|Other|
|Competitor made better offer|Competitor|
|Attitude of service provider|Attitude|
|Competitor had better devices|Competitor|
|Network reliability|Dissatisfaction|
|Product dissatisfaction|Dissatisfaction|
|Price too high|Price|
|Service dissatisfaction|Dissatisfaction|
|Lack of self-service on Website|Dissatisfaction|
|Extra data charges|Price|
|Moved|Other|
|Limited range of services|Dissatisfaction|
|Lack of affordable download/upload speed|Price|
|Long distance charges|Price|
|Poor expertise of phone support|Dissatisfaction|
|Poor expertise of online support|Dissatisfaction|
|Deceased|Other|

## Methods

See [my data preparation Jupyter Notebook](./EDA.ipynb) for detailed steps I took to obtain, clean and analyze the data. The general steps were:
1. Reviewed contents of the files provided, and decided which features were unique across all files and relevant to retain in my final combined dataset.
2. Imported raw data into dataframes, did some basic formatting, and visualized distributions of univarate features. 
3. Created visualizations to explore most common reasons for churn.
4. Used hypothesis testing to create a function to indicate statistically significant churn reasons for a given demographic or service group. (Despite my hopes, this was not useful in garnering deeper insights.)
5. Created visualizations to explore churn percentages of different demographic, service, and account groups.
6. Created visualizations to explore the monthly charges of package groups, separted by contract type.
7. Created visualizations to churn rate across contract type and tenure.

See [my ML Jupyter Notebook](./ModelDevelopment.ipynb) for detailed steps I took to develop and evaluate machine learning models. The following models were tested. Due to class imblalances, I tested each model with additions of SMOTE to oversample the minority class and the True class_balanced parameter when available.
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|    | model name              |   accuracy |   roc_auc |   precision_min |   recall_min |   f1-score_min |   precision_maj |   recall_maj |   f1-score_maj |
+====+=========================+============+===========+=================+==============+================+=================+==============+================+
|  0 | KNN                     |   0.763203 |  0.787949 |        0.568282 |     0.538622 |       0.553055 |        0.83091  |     0.847114 |       0.838934 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  1 | KNN Smote               |   0.694492 |  0.771982 |        0.462131 |     0.751566 |       0.572337 |        0.878819 |     0.673167 |       0.762367 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  2 | LogReg                  |   0.812606 |  0.860225 |        0.683047 |     0.580376 |       0.62754  |        0.851551 |     0.899376 |       0.87481  |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  3 | LogReg Class Balanced   |   0.755253 |  0.859642 |        0.532    |     0.832985 |       0.649308 |        0.92087  |     0.726209 |       0.812037 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  4 | LogReg Smote            |   0.760363 |  0.858358 |        0.538462 |     0.832985 |       0.654098 |        0.921569 |     0.733229 |       0.816681 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  5 | DecTree                 |   0.743328 |  0.663804 |        0.530612 |     0.488518 |       0.508696 |        0.814394 |     0.838534 |       0.826287 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  6 | DecTree Class Balanced  |   0.740488 |  0.664136 |        0.524336 |     0.494781 |       0.50913  |        0.815126 |     0.832293 |       0.82362  |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  7 | DecTree Smote           |   0.704145 |  0.688921 |        0.468657 |     0.655532 |       0.546562 |        0.848763 |     0.722309 |       0.780447 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  8 | RF                      |   0.79046  |  0.835788 |        0.65896  |     0.475992 |       0.552727 |        0.822615 |     0.907956 |       0.863181 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
|  9 | RF Class Balanced       |   0.795003 |  0.841564 |        0.679878 |     0.465553 |       0.552664 |        0.821354 |     0.918097 |       0.867035 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 10 | Random Forest Smote     |   0.764906 |  0.844336 |        0.548148 |     0.772443 |       0.641248 |        0.899632 |     0.76209  |       0.825169 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 11 | Bagging                 |   0.768313 |  0.796102 |        0.610592 |     0.409186 |       0.49     |        0.803472 |     0.902496 |       0.85011  |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 12 | Bagging Smote           |   0.744463 |  0.80143  |        0.523349 |     0.678497 |       0.590909 |        0.864912 |     0.769111 |       0.814203 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 13 | GBC                     |   0.794435 |  0.856833 |        0.660274 |     0.503132 |       0.57109  |        0.829513 |     0.903276 |       0.864824 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 14 | Gradient Boosting Smote |   0.350369 |  0.791341 |        0.294118 |     0.991649 |       0.453677 |        0.972603 |     0.110764 |       0.19888  |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 15 | LightGBM                |   0.789892 |  0.848759 |        0.644562 |     0.507307 |       0.567757 |        0.82948  |     0.895476 |       0.861215 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 16 | LightGBM Class Balanced |   0.780806 |  0.849174 |        0.574163 |     0.751566 |       0.650995 |        0.895062 |     0.791732 |       0.840232 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 17 | LGBM Smote              |   0.466212 |  0.816038 |        0.335475 |     0.981211 |       0.5      |        0.975    |     0.273791 |       0.427527 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 18 | XGB                     |   0.79841  |  0.856802 |        0.673184 |     0.503132 |       0.575866 |        0.830364 |     0.908736 |       0.867784 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 19 | XGB Class Balanced      |   0.79841  |  0.856802 |        0.673184 |     0.503132 |       0.575866 |        0.830364 |     0.908736 |       0.867784 |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+
| 20 | LGBM Smote              |   0.373083 |  0.796523 |        0.301082 |     0.987474 |       0.461463 |        0.968421 |     0.143526 |       0.25     |
+----+-------------------------+------------+-----------+-----------------+--------------+----------------+-----------------+--------------+----------------+

## Technical Presentation about Initial EDA

[Google Drive link](https://drive.google.com/file/d/19aR9KYIWNQG5ErrFV2ksu3sAiQFCIHpA/view?usp=sharing)
