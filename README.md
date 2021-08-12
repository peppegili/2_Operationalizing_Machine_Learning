# Operationalizing Machine Learning

## Overview
This project is part of the Udacity Azure ML Nanodegree. In this project, will use Azure to configure a cloud-based machine learning production model, deploy it, and consume it. Moreover, we will also create, publish, and consume a pipeline.

Both ***Azure ML Studio*** and ***Python SDK*** will be used in this project. The model will be trained using *AutoML* and the best model will be, finally, deployed and consumed through *Azure Container Instance (ACI)* and *REST endpoint* respectively.

The [aml-pipelines-with-automated-machine-learning-step.ipynb](https://github.com/peppegili/2_Operationalizing_Machine_Learning/blob/master/aml-pipelines-with-automated-machine-learning-step.ipynb) jupyter notebook contains all the steps of the entire procedure.

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
The following diagram shows all the steps of the entire process:

![Architectural Diagram](https://github.com/peppegili/2_Operationalizing_Machine_Learning/blob/master/img/architectural_diagram.png)

  - ***Authentication***: authentication is crucial for the continuous flow of operations. infact, when it is not set properly it requires human interaction and thus, the flow is     interrupted. So whenever possible, it's good to use authentication with automation (authentication types: key-based, token-based)
    
    A *Service Principal* is a user role with controlled permissions to access specific resources. Using a service principal is a great way to allow authentication while reducing     the scope of permissions, which enhances security.
    
    This step has been skipped since we are not authorized to create a security principal, using the lab udacity provided
  
  - ***Auto ML Model***: AutoML is the process of automating the time-consuming, iterative tasks of machine learning model development. A classification model on bank marketing       [dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) is built using AutoML, and the best model is determined       based on metrics collected (*AUC_weighted*)
  
  - ***Deploy the best model***: the best performing model, the one with the highest AUC weighted value, is selected for deployment. *Azure Container Instance* or *Azure               Kubernetes Service* can be chosen in this step
  
  - ***Enable logging***: Application Insights tool allows to detect anomalies, visualize performance and keep track of the deployed model. It can be enabled before or after a         deployment
  
  - ***Consume model endpoints***: after model deployment, consuming information from it is a foundamental step. An HTTP API is exposed over the network so that interaction with       trained model can happen via HTTP requests
  
  - ***Create and publish a pipeline***: pipelines allow to automate workflows, i.e. the entire process of train and deploy a model. Published pipelines allow external services to     interact with them in a simple and efficient way
  
  - ***Documentation***: this is the last but not least step. A screencast video and a README file have been created, containing the description of the project and all the             performed steps

## Key Steps
  - ***Auto ML Model***
    Automated ML includes all the tasks of machine learning model development, from loading dataset, creating pipeline and AutoML step, to start the training procedure:
    
      - ***Load data***: dataset has been loaded as tabularDataset and registered in workspace
        ```
        found = False
        key = "BankMarketing Dataset"
        description_text = "Bank Marketing DataSet for Udacity Course 2"

        if key in ws.datasets.keys(): 
                found = True
                dataset = ws.datasets[key] 

        if not found:
                # Create AML Dataset and register it into Workspace
                example_data = 'https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv'
                dataset = Dataset.Tabular.from_delimited_files(example_data)        
                #Register Dataset in Workspace
                dataset = dataset.register(workspace=ws,
                                           name=key,
                                           description=description_text)
                                          
        df = dataset.to_pandas_dataframe()
        ```
        
      - ***AutoML***: *AutoMLConfig*, *AutoMLStep*, *Pipeline* classes, responsible of the automated machine learning process, have been instantiated with the following parameters 
        ```
        # AutoML config
        automl_settings = {
            "experiment_timeout_minutes": 20,
            "max_concurrent_iterations": 5,
            "primary_metric" : 'AUC_weighted'
        }
        automl_config = AutoMLConfig(compute_target=compute_target,
                                     task = "classification",
                                     training_data=dataset,
                                     label_column_name="y",   
                                     path = project_folder,
                                     enable_early_stopping= True,
                                     featurization= 'auto',
                                     debug_log = "automl_errors.log",
                                     **automl_settings
                                    )
        
        # AutoML step
        automl_step = AutoMLStep(
            name='automl_module',
            automl_config=automl_config,
            outputs=[metrics_data, model_data],
            allow_reuse=True)
        
        # Pipeline
        from azureml.pipeline.core import Pipeline
        pipeline = Pipeline(
            description="pipeline_with_automlstep",
            workspace=ws,    
            steps=[automl_step])
        ```
        After submitting the pipeline run to the experiment, results metrics have been collected and the best model has been retrieved.

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
