---
title: 5 Setting up a VSTS Docker Build
author: philip_hine
date: 2018-05-16
order: 5
--- 

## Overview
This post shows you how to set up a build definition on VSTS and creating a VSTS agent to package and pass a container to an Azure container registry (staging area for containers pre deployment). 

## Prerequisites
A VSTS account.
Azure subscription using the same credentials you sign into to VSTS with.
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

Enter your token that you got in step one.
![Step2]({{ site.baseurl }}/img/VSTS_Build/VSTS_EnterPAT.png)<br/><br/>

Select your repository and branch:
![Step2]({{ site.baseurl }}/img/VSTS_Build/VSTS_SelectRepository2.png)<br/><br/>

4) Select the container template and press apply.
![Step2]({{ site.baseurl }}/img/VSTS_Build/VSTS_SelectTemplate.png)<br/><br/>

5) Click the process section and choose the agent you set up previously.

6) Select the Build an image task in phase 1 and select the docker file by pressing the explore button (...). That is all that is need in order to create a container containing your source code.

7) Select the next task 'Push an image'. Ensure the container registry type is 'Azure Container Registry'. Click 'Manage' on the Azure subscription field.  and then on the subscription dropdown select your subscription (this will not be available if your VSTS user does not have an azure subscription). This will open up a new tab. New Service Endpoint > Azure Resource Manager. Enter a sensible connection name and select the resource group that you assigned your container registry to.
Once that has completed you can close your tab.
![Step7]({{ site.baseurl }}/img/VSTS_Build/VSTS_SetupResourceManagerServiceEndpoint.png)<br/><br/>

8) Select the azure resource manager connection you have just set up. Then choose the container registry that you previously set up in a previous tutorial. That's all that is required to push your container to the staging area in Azure.

![Step8]({{ site.baseurl }}/img/VSTS_Build/VSTS_PushImageSettings.png)<br/><br/>
