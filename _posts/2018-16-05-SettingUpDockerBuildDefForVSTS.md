---
title:  Setting up a VSTS Docker Build
author: Phil Hine
date: 2018-05-16
--- 

## Overview
This post shows you how to set up a build definition on VSTS and creating a VSTS agent to package and pass a container to an Azure container registry (staging area for containers pre deployment). 

## Prerequisites
Azure Container Registry
A git hub repository with .Net Core project that has been enabled for docker.
Set up a new project in VSTS and ensure you have admin access.

## Getting started
1)
a) You will need to create a github personal access token to allow VSTS access to your repository (for this tutorial). Open GitHub and go to settings
![Step1]({{ site.baseurl }}/img/VSTS_Build/githubPAT_1.png)<br/><br/>

b) Next go to developer settings > Personal access tokens.
![Step1B]({{ site.baseurl }}/img/VSTS_Build/githubPAT_2.png)<br/><br/>

c) Click Generate New Token. Give it a sensible description and select the repo box only.
![Step1C]({{ site.baseurl }}/img/VSTS_Build/githubPAT_3.png)<br/><br/>

d) Once generated you will get to see the new personal access token for the first and last time within github so make a note of it.
![Step1D]({{ site.baseurl }}/img/VSTS_Build/githubPAT_Generated.png)<br/><br/>

2) Open VSTS project. Build and releases > Builds. Click on New.<br/><br/>
![Step2]({{ site.baseurl }}/img/VSTS_Build/AddNewDefinition.png)<br/><br/>

3) Here you need to enter repository connection details. Select your source as GitHub. Give your connection a new name and click authorize with a github personal access token.
![Step2]({{ site.baseurl }}/img/VSTS_Build/VSTS_SelectRepository1.png)<br/><br/>