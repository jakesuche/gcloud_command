 #     Create a Deployment Manager deployment


         gsutil cp gs://cloud-training/gcpfcoreinfra/mydeploy.yaml mydeploy.yaml


# In the Cloud Shell, use the sed command to replace the PROJECT_ID placeholder string with your Google Cloud Platform project ID using this command:

        sed -i -e "s/PROJECT_ID/$DEVSHELL_PROJECT_ID/" mydeploy.yaml

# In the Cloud Shell, use the sed command to replace the ZONE placeholder string with your Google Cloud Platform zone using this command:

        sed -i -e "s/ZONE/$MY_ZONE/" mydeploy.yaml

# Build a deployment from the template:

        gcloud deployment-manager deployments create my-first-depl --config mydeploy.yaml

        gcloud deployment-manager deployments update my-first-depl --config mydeploy.yaml