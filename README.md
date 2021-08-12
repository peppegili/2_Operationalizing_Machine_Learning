# Operationalizing Machine Learning

## Overview
This project is part of the Udacity Azure ML Nanodegree. In this project, will use Azure to configure a cloud-based machine learning production model, deploy it, and consume it. Moreover, we will also create, publish, and consume a pipeline.

Both ***Azure ML Studio*** and ***Python SDK*** will be used in this project. The model will be trained using *AutoML* and the best model will be, finally, deployed and consumed through *Azure Container Instance (ACI)* and *REST endpoint* respectively.

## Table of Contents
  - [Overview](#overview)
  - [Dataset](#dataset)
  - [Architectural Diagram](#architectural-diagram)
  - [Key Steps](#key-steps)
  - [Screen Recording](#screen-recording)
  - [Standout Suggestions](#standout-suggestions)

## Dataset
In this project a bank marketing [dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) is used.
It contains phone calls from a direct marketing compaign of a Portoguese banking institution.

The dataset has a series of information (age, job, marital, education, etc...) for a total of 32950 observations, 20 features, and a target variable (y)
with two possible values: yes or no.
The task is addressed as a classification task and the goal is to predict if a client will subscribe a term deposit (y variable).

## Architectural Diagram
*TODO*: Provide an architectual diagram of the project and give an introduction of each step. An architectural diagram is an image that helps visualize the flow of operations from start to finish. In this case, it has to be related to the completed project, with its various stages that are critical to the overall flow. For example, one stage for managing models could be "using Automated ML to determine the best model". 

The following diagram shows all the steps of the entire process:

![Architectural Diagram](https://github.com/peppegili/2_Operationalizing_Machine_Learning/blob/master/img/architectural_diagram.png)

  - ***Authentication***: authentication is crucial for the continuous flow of operations. infact, when it is not set properly it requires human interaction and thus, the flow is     interrupted. So whenever possible, it's good to use authentication with automation (authentication types: key-based, token-based).
    
    A *Service Principal* is a user role with controlled permissions to access specific resources. Using a service principal is a great way to allow authentication while reducing     the scope of permissions, which enhances security.
    
    This step has been skipped since we are not authorized to create a security principal, using the lab udacity provided.
  
  - ***Auto ML Model***: AutoML is the process of automating the time-consuming, iterative tasks of machine learning model development. A classification model on bank marketing       [dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) is built using AutoML.
  
  - ***Deploy the best model***
  - ***Enable logging***:
  - ***Consume model endpoints***:
  - ***Create and publish a pipeline***:
  - ***Documentation***:

## Key Steps
*TODO*: Write a short discription of the key steps. Remeber to include all the screenshots required to demonstrate key steps. 

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
