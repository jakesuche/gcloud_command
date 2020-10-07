#  Automating the Deployment of Infrastructure Using Terraform

#### Terraform enables you to safely and predictably create, change, and improve infrastructure. It is an open-source tool that codifies APIs into declarative configuration files that can be shared among team members, treated as code, edited, reviewed, and versioned.

#### In this lab, you create a Terraform configuration with a module to automate the deployment of Google Cloud infrastructure. Specifically, you deploy one auto mode network with a firewall rule and two VM instances, as shown in this diagram:

## Objectives

In this lab, you learn how to perform the following tasks:

- [X] Create a configuration for an auto mode network

- [X] Create a configuration for a firewall rule

- [x] Create a module for VM instances

- [x] Create and deploy a configuration

- [x] Verify the deployment of a configuration


### Install Terraform

1 very the version of the terraform

        terraform --version

2  create a fold name tfinfra and a file "provider.tf"

        mkdir tfinfra
        touch tfinfra/provider.tf

3 copy "provider "google" {}" inside provider.tf

        echo provider "google" {} > provider.tf

4 navigate to the folder "tfinfra" and initialize terraform

        cd tfinfra 
        terraform init


5  after writing the code base for network, vm-instances and firewalls which are _main.tf_, _network.tf_, _mynetwork.tf_

######    Create mynetwork and its resources

_It's time to apply the mynetwork configuration._

1 To rewrite the Terraform configuration files to a canonical format and style, run the following command:


        terraform fmt

2  To initialize Terraform, run the following command:

        terraform init

3  To create an execution plan, run the following command:

        terraform plan

_The output should look like this_

        ...
        Plan: 4 to add, 0 to change, 0 to destroy.
        ...

_Terraform determined that the following 4 resources need to be added:_

| name                              |  Description                                   |
|-----------------------------------|------------------------------------------------|
| mynetwork                         | VPC network                                    |
| mynetwork-allow-http-ssh-rdp-icmp | Firewall rule to allow HTTP, SSH, RDP and ICMP |
| mynet-us-vm                       | VM instance in us-central1-a                   |
| mynet-eu-vm                       | VM instance in europe-west1-d                  |


4 To apply the desired changes, run the following command:

        terraform apply

5 To confirm the planned actions, type:

         yes

_The output should look like this_

        ...
        Apply complete! Resources: 4 added, 0 changed, 0 destroyed.





