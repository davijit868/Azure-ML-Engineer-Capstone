# Udacity Azure Machine Learning Engineer Capstone

This is the final project for Udacity Machine Learning Engineer with Microsoft Azure Nanodegree. In this project heart failure dataset is taken from Kaggle. The project pipeline is built entierly on the AzureML ecosystem and relies on AutoML and HyperDrive to find and tune the best model for classification. The best model out of the two is then deployed and an endpoint is available for real-time predictions. This project is all about giving a high-level understanding of the components of Azure and how they work together to assist in the process of building, deploying, and maintaining machine learning models.

## Dataset

### Overview
> Cardiovascular diseases (CVDs) are the number 1 cause of death globally, taking an estimated 17.9 million lives each year, which accounts for 31% of all deaths worlwide.
Heart failure is a common event caused by CVDs and this dataset contains 12 features that can be used to predict mortality by heart failure. The dataset contains 300 rows. refer https://www.kaggle.com/andrewmvd/heart-failure-clinical-data. 

### Task

In this project a classifier is trained to predict the likelihood of a person to have a death event due to cardiovascular diseases. In order to use and train our models 
on the dataset i.e. a TabularDataset, we have registered the dataset using the option from a local file (heart_failure_clinical_records_dataset). Two models i.e. AutoML and Hyperdrive ML model on Azure is built and performance of the models is compared in terms of accuracy.

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_1.png)

### Access
The dataset is registered in Azure ML workspace from locally uploaded csv file. It is then consumed in both the script i.e. automl.ipynb and hyperparameter_tuning.ipynb using code. 

## Automated ML
The settings used for the exploration of the model space with AutoML are quite limited in time and number of iterations to comply with the use of the 
workspace provided by Udacity. Nonetheless, to optimize computation speed we make use of concurrent iterations and maximum number of cores available.
Since we are tackling a classification problem, with a balanced dataset, the metric we want to optimize is the simple accuracy.
To train and evaluate the model we run a 3-fold cross validation.

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_2.png)

### Results
The best model generated by AutoML is a VotingEnsemble method that relies on a pre-trained LightGBMClassifier with a pre-processing. The best model was obtained from the last run of AutoML and a larger search over the model space might improve performance. 

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_3.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_4.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_5.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_6.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_7.png)

## Hyperparameter Tuning
The model of choice for this project is the `RandomForestClassifier` available in SKlearn.
A random forest is a meta estimator that fits a number of decision tree classifiers on various sub-samples of the dataset.
This model is typically used as a baseline is many projects and Kaggele competitions.
In AzureML we run HyperDrive on two parameters, which are arguably the two most important ones:
    * `n-estimators`: the number of estimators, i.e. trees, in the forest. It's in range [50, 300]
    * `max-depth`: the maximum depth of each tree, i.e. how many split each tree can perform. It's in range [1, 20]
![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/1.JPG)
![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/hypml-best-run.JPG)
![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/hyp-matrix.JPG)
To further improve this model we could tune other parameters such as the criterion used to define the optimal split and the [min]/[max]_samples_leaf number of samples in the 
leaf node of each tree.
![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/hyper-logs.JPG)

## Model Deployment

To deploy the best model found by AutoML we use the AzureML Endpoint. The best model is deployed following this steps:

1. Register the model and download its artifacts
2. Create an environment with all required dependencies using the artifacts provided by AutoML
3. Define the configurations of the WebService, i.e. number of CPUs and memory size
4. Deploy the model with the previously created configurations
5. Enable app insights to debug and analyze the behaviour of the endpoint

![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/automl-aci-deploy.JPG)

In order to query and consume the endpoint we have to make a request with a JSON body where the data field is the dictionary representation of the data point to predict

![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/automl-dict.JPG)

Runnung status of the model on Azure Application Insights.
![alt text](https://github.com/hammad-alt/nd00333-capstone/blob/master/Images/App-insights.JPG)

## Improvement:
1. The Hyperparameters metrics can give better results, if we can define custom min/max metric for the run.
2. The Bayesian Sampling can give better results with more than 20 concurrent runs. Hence, for better results we can resubmit the experiment with 40 concurrent runs. 


> Link to screen cast
https://youtu.be/NeyK1PRhtV0

