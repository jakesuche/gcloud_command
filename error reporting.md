# Error Reporting and Debugging

## Overview
    In this lab, you learn how to use Cloud Error Reporting and integrate Cloud Debugger.

## Objectives
In this lab, you learn how to perform the following tasks:

1 Launch a simple Google App Engine application

2 Introduce an error into the application

3 Explore Cloud Error Reporting

4 Use Cloud Debugger to identify the error in the code

5 Fix the bug and monitor in Cloud Operations


### To create a local folder and get the App Engine Hello world application, run the following commands:

        mkdir appengine-hello
        cd appengine-hello
        gsutil cp gs://cloud-training/archinfra/gae-hello/* .

### To run the application using the local development server in Cloud Shell, run the following command:

        dev_appserver.py $(pwd)

# Deploy the application to App Engine

        gcloud app deploy app.yaml