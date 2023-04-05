# FULLSTACK APP

[![CircleCI](https://circleci.com/gh/Royalboe/fullstack-note-app.svg?style=svg)](https://circleci.com/gh/Royalboe/fullstack-note-app)

## Description

This repo contains a pipeline to build a kubernetes cluster on AWS and deploy on the cluster two containerized web applications.
The Project is focused on deploying two web applications using kubernetes.
The first is a socks microservice application and the other one is a notes microservice application.

## Stack Used

* CircleCI : For the CI/CD pipeline
* Kubernetes : For orchestration
* Docker: To build the images
* Terraform: To set up the infrastructures
* AWS: The cloud platform used
* Prometheus: The monitoring tool
* Grafana: For data visualization

## Files And Directories

Files | Description
------|------------
[CircleCI](./.circleci) | This folders contains the circleci config file, terrraform file to spin off infrastructure and Dockerfile to build the image to use for the jobs  
[Terraform Cluster](./.circleci/terraform-cluster) | This folder contains the terraform files build a VPC that can work with kubernetes, then build the kubernetes cluster.
[EKS Module](./.circleci/terraform-cluster/modules/eks-cluster) | This terraform module contains the terraform files to setup the clusters with the appropriate iam-roles for the cluster and cluster node groups. The nodes were created inside of private subnets
[Network Module](./.circleci/terraform-cluster/modules/network) | This terraform module contains the terraform files to setup an ideal VPC for kubernetes. The VPC has two public and two private subnets in two AZs. It also has a NAT gateway.
[SSH-Key Module](./.circleci/terraform-cluster/modules/ssh-key-pair/) | This terraform module contains the terraform files to setup the sshkey to use with the nodes.
[CircleCi Config file](./.circleci/config.yml) | This contains the jobs to set up the EKS cluster, deploy a web application on it and take down the applications while tearing down the infrastructures.
[Note Application](./fullstack-notes-application) | This folder contains the files to build the note application and deploy it to the k8s cluster on AWS
[Socks Application](./socks-microservices/) | This folder contains the files to build the socks application and deploy it to k8s cluster on AWS
[Ingress Controller](./deploy.yaml) | This file is to set up the nginx ingress controller

## Set Up

* Created a GitHub repository for the project
* Created a CircleCi account
* Created a .circleci folder and created a config.yml file in it
* Set up CircleCi to track changes to the repository.
* In the project's environment variables on circleci, I added environment variables for AWS credentials, without this, circleci would not be able to run jobs to set up the infrastructure and deploy applications to the cluster.
* On AWS, created a user with programmatic access and gave it the admin access permission.
* Also added environment variables for docker password and username, this allows for images to be pushed to DockerHub.
* Created S3 bucket with state locking and versioning to store the terraform state.
* The state locking will prevent race conditions when multiple sources are trying to modify the terraform state at different times
* Also the S3 bucket allows for remote storage of state, this promotes remote working and collaboration.
* An alternative to AWS S3 bucket is terraform cloud
* An alternative to terraform is AWS cloud formation or eksctl
* Jobs were set up to create the cluster and deploy the application
* The terraform file creates a VPC with 4 subnets (2 private subnets and 2 public subnets) in 2 availability zones with NAT gateway to the private subnet.
* It also creates the kubernetes cluster and gave the subnets annotations to let them interact properly with kubernetes.
* The manifest files for the note application are in [this folder](./fullstack-notes-application/k8s)
* The manifest files for the socks application are in [this folder](./socks-microservices/deploy/kubernetes/manifests)
* Set Up a public TLS certificate on AWS Certificate Manager for the FQDNs (`socks.royalboe.live` and `notes.royalboe.live`).
* Copied the ARN of the certificate and pasted in the deploy.yaml file in the 'service.beta.kubernetes.io/aws-load-balancer-ssl-cert' key.
* The [deploy.yaml](./deploy.yaml) file deployed the ingress controller along with the RBAC, serviceaccount, services.
* The nginx ingress controller created a loadbalancer on AWS and the alias of the loadblancer was used as the A record of the hosted zones for the FQDNs.
* Both applications have an ingress object definition as part of the manifest files that handles the services to route traffic to.
* Prometheus was setup for collecting data for monitoring
* Grafana was setup for visualizing
* The manifests for the prometheus and grafana are in [this folder](./socks-microservices/deploy/kubernetes/manifests-monitoring/)

### Website URLS

* Socks microservices application : `https://socks.royalboe.live`
* Notes microservices application : `https://notes.royalboe.live`
* Prometheus Monitoring: `http://prometheus.royalboe.live`
* Grafana : `http://grafana.royalboe.live`

### Images

* [Socks shop]('./socks shop.png')
* [Notes Application]('./notes app.png')
* [Grafana](./grafana.png)
* [Prometheus](./prometheus.png)
* [Pipeline Image 1](./pipeline_image_1.png)
* [Pipeline Image 2](./pipeline_image_2.png)
* [Pipeline Image 3](./pipeline_image_3.png)
* [Pipeline Image 4](./pipeline_image_4.png)
* [Pipeline Image 5](./pipeline_image_5.png)
* [Pipeline Image 6](./pipeline_image_6.png)
