# Additional supported SDK<a name="experiments-additional-sdk"></a>

**Important**  
As of [v2\.123\.0](https://github.com/aws/sagemaker-python-sdk/releases/tag/v2.123.0), SageMaker Experiments is now fully integrated with the [SageMaker Python SDK](https://sagemaker.readthedocs.io/en/stable/) and you no longer need to use the separate [SageMaker Experiments SDK](https://sagemaker-experiments.readthedocs.io/en/latest/)\. We recommend creating an experiment with `sagemaker.experiments.run` rather than the following `smexperiments` module\. 

The following section describes how to create a SageMaker Experiment with the SageMaker Experiments SDK\.

## Create an Amazon SageMaker Experiment with the SageMaker Experiments SDK<a name="experiments-create-smexperiments"></a>

Create an Amazon SageMaker experiment to track your SageMaker training, processing, and transform jobs\.

The following procedure shows you how to create a SageMaker experiment for a SageMaker training, processing, or transform job\. Steps labeled as \(Studio\) describe how to view the experiment in Amazon SageMaker Studio\. You don't have to run the experiment in Studio to view the experiment in Studio\.

1. Import the `sys` module to install the SDKs\.

   ```
   import sys
   ```

1. \(Optional\) The [Amazon SageMaker Python SDK](https://sagemaker.readthedocs.io), comes preinstalled in SageMaker Studio\. If you plan to run your code outside Studio, install the SageMaker Python SDK\.

   ```
   !{sys.executable} -m pip install sagemaker
   ```

1. Install the [SageMaker Experiments Python SDK](https://sagemaker-experiments.readthedocs.io/en/latest/)\.

   ```
   !{sys.executable} -m pip install sagemaker-experiments
   ```

1. Import modules\.

   ```
   import time
   from time import strftime
   
   import sagemaker
   
   from smexperiments.experiment import Experiment
   from smexperiments.trial import Trial
   from smexperiments.trial_component import TrialComponent
   from smexperiments.tracker import Tracker
   ```

1. Get the execution role and create the SageMaker session\.

   ```
   role = sagemaker.get_execution_role()
   sm_sess = sagemaker.session.Session()
   ```

1. Create a SageMaker experiment\. The experiment name must be unique in your account\.
**Note**  
The `tags` parameter is optional\. You can search for the tag using Studio, the SageMaker console, and the SDK\. Tags can also be applied to trials and trial components\.

   ```
   create_date = strftime("%Y-%m-%d-%H-%M-%S")
   demo_experiment = Experiment.create(experiment_name = "DEMO-{}".format(create_date),
                                       description = "Demo experiment",
                                       tags = [{'Key': 'demo-experiments', 'Value': 'demo1'}])
   ```

1. \(Studio\) To view the experiment in SageMaker Studio, in the left sidebar, choose the **Experiments**\.

   After the code runs, the experiment list contains the new experiment\. It might take a moment for the list to refresh and display the experiment\. The filter on the experiment tag is also displayed\. Only experiments that have a matching tag are displayed\. Your list should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-overview.png)

1. Create a trial for the experiment\. The trial name must be unique in your account\.

   ```
   demo_trial = Trial.create(trial_name = "DEMO-{}".format(create_date),
                             experiment_name = demo_experiment.experiment_name,
                             tags = [{'Key': 'demo-trials', 'Value': 'demo1'}])
   ```

1. Create a trial component as part of the trial\. The trial component is the SageMaker job\.

   Add the [ExperimentConfig](https://docs.aws.amazon.com/sagemaker/latest/APIReference/API_ExperimentConfig.html) parameter to the appropriate method\. The SageMaker jobs listed in the following table are supported\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/sagemaker/latest/dg/experiments-additional-sdk.html)

   The following examples are for a training job\. The `Tags` parameter adds a tag to the trial component\. `ExperimentName` isn't specified because the trial was associated with the experiment when the trial was created in an earlier step\.

   **Using the SageMaker Python SDK**

   ```
   sagemaker.estimator.Estimator(
       ...,
       sagemaker_session = sm_sess,
       tags = [{'Key': 'demo-jobs', 'Value': 'demo2'}])
   
   estimator.fit(
       ...,
       experiment_config = {
           # "ExperimentName"
           "TrialName" : demo_trial.trial_name,
           "TrialComponentDisplayName" : "TrainingJob",
       })
   ```

   **Using Boto3**

   ```
   create_training_job(
       ...,
       "ExperimentConfig": {
           # "ExperimentName"
           "TrialName" : demo_trial.trial_name,
           "TrialComponentDisplayName" : "TrainingJob",
       },
       "Tags": [{'Key': 'demo-jobs', 'Value': 'demo2'}])
   ```

1. \(Studio\) In the experiment list, double\-click the experiment to display a list of the trials in the experiment\. In the Studio UI, trials are referred to as* run groups* and trial components are referred to as *runs*\. Your list should look similar to the following:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sagemaker/latest/dg/images/experiments/experiments-runs-overview.png)

1. \(Studio\) To view information about the experiment, trial, and job \(trial component\), see [View, search, and compare experiment runs](experiments-view-compare.md)\.

To clean up the resources you created, see [Clean Up Amazon SageMaker Experiment Resources](experiments-cleanup.md)\.