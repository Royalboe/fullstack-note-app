# FULLSTACK APP

[![CircleCI](https://circleci.com/gh/Royalboe/project-ml-microservice-kubernetes.svg?style=svg)](https://circleci.com/gh/Royalboe/project-ml-microservice-kubernetes)

## Description

This repo contains a pipeline to build a kubernetes cluster on AWS and deploy on the cluster two containerized web applications.


## Stack Used

* CircleCI
* Kubernetes
* Docker
* Terraform
* AWS
* Prometheus
* Grafana
* Postgres
* Nginx
* Javascriot
* NodeJs
* Go

## Files And Directories

Files | Description
------|------------
[CircleCI](./.circleci) | This folders contains the circleci config file, terrraform file to spin off infrastructure and Dockerfile to build the image to use for the jobs  
[Terraform Cluster](./.circleci/terraform-cluster) | This folder contains the terraform files build a VPC that can work with kubernetes, then build the kubernetes cluster.
[CircleCi Config file](./.circleci/config.yml) | This contains the jobs to set up the EKS cluster, deploy a web application on it and take down the applications while tearing down the infrastructures.
[Note Application](./fullstack-notes-application) | This folder contains the files to build the note application and deploy it to the k8s cluster on AWS
[Socks Application](./socks-microservices/) | This folder contains the files to build the socks application and deploy it to k8s cluster on AWS
[Ingress Controller](./deploy.yaml) | This file is to set up the nginx ingress controller

## Set Up

* Create a GitHub repository for the project
* Create a CircleCi account
* Create a .circleci folder and create a config.yml file in it
* On circleci follow the project so that circleci will track the project and run jobs when ever there is a push to the repository.
* In the project's environment variables on circleci, add environment variables for AWS credentials, without this, circleci would not be able to run jobs to set up the infrastructure and deploy applications to the cluster.
* On AWS, create a user with programmatic access and give it the admin access permission.
* Also add environment variables for docker password and username, this allows for images to be pushed to DockerHub.
* Create S3 bucket with state locking and versioning to store the terraform state.
* The state locking will prevent race conditions when multiple sources are trying to modify the terraform state at different times
* Also the S3 bucket allows for remote storage of state, this promotes remote working and collaboration.
* An alternative to AWS S3 bucket is terraform cloud
* An alternative to terraform is AWS cloud formation 

### Website URLS

* Socks microservices application : `https://socks.royalboe.live`
* Notes microservices application : `https://notes.royalboe.live`
