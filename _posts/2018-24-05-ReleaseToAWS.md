---
title:  Releasing a container to AWS
author: Phil Hine
date: 2018-05-24
--- 

## Overview
This post shows you how to use VSTS to release a docker image to an AWS EC2 cluster.

## Prerequisites
An AWS account
A VSTS account
A VSTS host with docker installed
A hello world application

## Create a Elastic Container Registry (ECR)

1) Log into AWS
2) Click on the services dropdown and enter 'container' into the filter box. Then click on Elastic Container service.<br/><br/>
![Step1]({{ site.baseurl }}/img/AWS_ECR/1.Select_ECR.png)<br/><br/>


3) 