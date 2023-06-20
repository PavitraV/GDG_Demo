# Workflow 101 GDG Code Base
## Summary
This repo contains sample code for a few simple GCP Workflows demonstrating some features. This repo is not meant to be a design recommendation nor is it suitable for a production workload. What it is meant for is to illustrate the different functions of workflows and how to use them.

## Content
Before the Workflows are created, a SA should be created with necessary permissions on Workflows, GCS Bucket, BQ, Firestore, and Logging. Each file in this repo demonstrates a certain feature of Workflow as below-

### Function Call
#### Prerequisites
Create a workflow with an eventArc which triggers the workflow when an object is added to a GCS bucket.

This Workflow is used to create a BigQuery table from a GCS Bucket. The purpose is to demo the use of API calls in GCP. The first task is to create a dataset and then create a table in that dataset.

### Exception Handling
#### Prerequisites
Create a workflow with an eventArc which triggers the workflow when an object is added to a GCS bucket.

This Workflow is used to create a BigQuery table from a GCS Bucket. The purpose is to demo exception handling mechanism in Workflows. The first task is to create a dataset, but if the dataset already exists this function throws an exception. The handling done here is to continue the Workflow and not try to create the dataset. The next task is to upload the object from the event trigger into BQ. If an incompatible file extension is uploaded or if a file is given an incorrect extension as CSV but is not a comma-separated file, BQ throws an exception. This is handled by moving the file to a quarantine bucket.

### Looping

This Workflow prints all the objects from a given GCS Bucket using a "for loop". The purpose is to demo looping in Workflows.

### Parallelism

This Workflow reads all the objects from a given GCS Bucket using a "for loop" and uploads them into BQ while also parallelly uploading the objects into a GCS Bucket to track the processed files. The purpose is to demo looping & parallelism in Workflows. 

### Callbacks

This Workflow creates a callback event and saves it in Firestore. The Workflow awaits the callback to be called before it continues execution. The purpose is to demo callback events for a human in the loop process in Workflows. The below code can be executed to quit the await_callback. This execution is done through the GCP Cloud Shell. The ``` CALLBACK_URL ``` can be found in Firestore and any message can be entered. 

```
curl -X POST -H "Content-Type: application/json" -H "Authorization: Bearer $(gcloud auth print-access-token)" -d '{"msg" : "bar"}' <CALLBACK_URL>
```
