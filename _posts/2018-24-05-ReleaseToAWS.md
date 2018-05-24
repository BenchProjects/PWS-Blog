---
title:  Releasing a container to AWS
author: Phil Hine
date: 2018-05-24
--- 

## Overview
This post shows you how to use VSTS to release a docker image to an AWS EC2 cluster. It does not go in depth into all of the available options and is meant to teach you the simplest way to get up and running. It does not cover security or best practices.

## Prerequisites
An AWS account<br/>
A VSTS account<br/>
A VSTS host with docker installed<br/>
A hello world application

## Create a Elastic Container Registry (ECR)

1) Log into AWS<br/><br/>
2) Click on the services dropdown and enter 'container' into the filter box. Then click on Elastic Container service.<br/><br/>
![Select elastic container service]({{ site.baseurl }}/img/AWS_ECR/1_Select_ECS.PNG)<br/><br/>
3) Press the pin icon that lives alongside the top menu, this will allow you to easily access this area in future.<br/><br/>
4) In the left hand pane select Repositories then select Get Started<br/><br/>
![Select repositories]({{ site.baseurl }}/img/AWS_ECR/2_Select_Repositories.PNG)<br/><br/>
5) Enter a sensible name for your repository and take a note of your repository url although you can get this later and press Next Step.<br/><br/>
![Enter ECR name]({{ site.baseurl }}/img/AWS_ECR/3_ConfigureRepository.PNG)<br/><br/>
6) You should now see a screen that talks you through how to push an image to the registry. You can come back and view that later if you wish.<br/><br/>
![ECR Created]({{ site.baseurl }}/img/AWS_ECR/3_ConfigureRepository.PNG)<br/><br/>

## Create a Elastic Container Service (ECS) Cluster

We need to define the environments we wish to release to by creating a ECS cluster.

1) Navigate again to the ECS page, if you followed point 2 above then you should have a bookmark at the top of the page you can use to get here quickly.<br/><br/>
2) In the left pane click on clusters and in the main pane 'Create Cluster')<br/><br/>
![Create ECS Cluster]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/4-CreateCluster.PNG)<br/><br/>
3) Now select the cluster type, which for these set of tutorials should be set to linux and press next step.)<br/><br/>
![Select ECS Cluster Template]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/5-SelectClusterTemplate.PNG)<br/><br/>
4) Enter a name for your cluster. For this tutorial leave all other settings as default (it would be worth taking time to understand this once you have followed this basic example). Next press create.<br/><br/>
![Configure ECS Cluster]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/6_ConfigureCluster.PNG)<br/><br/>
5) On the next page you will see your environment being set up. Eventually all tasks will complete and you will have a cluster set up ready for use. Make a note of the name you gave to the cluster.<br/><br/>
![Completion ECS Cluster Screen]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/6-Created.PNG)<br/><br/>







