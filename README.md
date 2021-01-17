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
Please find the below screenshot for the AutoML configuration used.

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_2.png)

### Results
The best model generated by AutoML is a VotingEnsemble method with 86% accuracy. 

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_3.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_4.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_5.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_6.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_7.png)

## Hyperparameter Tuning
`RandomForestClassifier` available in SKlearn is used for Hyperdrive Experiment. A random forest is a meta estimator that fits a number of decision tree classifiers on various sub-samples of the dataset. This model is typically used as a baseline is many projects and Kaggele competitions.
In AzureML we run HyperDrive on two parameters, which are arguably the two most important ones:
- `n-estimators`: the number of estimators, i.e. trees, in the forest. It's in range [50, 300]
- `max-depth`: the maximum depth of each tree, i.e. how many split each tree can perform. It's in range [1, 20]
    
![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_8.png)

### Results

Hyperdrive experiment produced a model with 80% accuracy.

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_9.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_10.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_11.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_12.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_13.png)

To further improve this model we could tune other parameters such as the criterion used to define the optimal split.

## Model Deployment

AutoML resulted a better model than hyperdrive experiment in terms of accuracy. The model is then registered and artifacts a downloaded. An environment with all required dependencies is created using the artifacts provided by AutoML. WebService configurations are defined, i.e. number of CPUs and memory size. The model is deployed with the previously created configurations. App insights is enabled to debug and analyze the behaviour of the endpoint.

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_14.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_15.png)

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_16.png)

The endpoint is consumed with a JSON request where the data field is the dictionary representation of the data point to predict.

![alt text](https://github.com/davijit868/Azure-ML-Engineer-Capstone/blob/master/Screenshots/Screenshot_17.png)

## Improvement:
- Different Configuration settings can be tried for both AutoML as well as Hyperdrive parameter.
- With hyperdrive different model like deep learning or XGBoost can also be tried.

## Link to screen cast
https://youtu.be/NeyK1PRhtV0

