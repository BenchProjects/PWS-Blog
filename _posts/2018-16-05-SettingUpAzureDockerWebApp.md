---
title: 6 Setting up Azure Docker web app
author: will_andrews
date: 2018-05-16
--- 

## Overview
This blog will go through creating a Docker web app on Azure which will be used to host your web app, and it will do so by being a docker container that contains an image with your app built on. It will hook into the container registry you created earlier that you build you app image to from VSTS.

## Prerequisites

An Azure account.

A Docker container registry on Azure.

An image built and pushed to your container registry from VSTS

## Setting up an web app that will run you application from a Docker container

This is what will be used to host your web app, and it will do so by being a docker container that contains an image with your app built on.

1) On Azure, create a new Web App for Containers.

2) Give the app a name (this will be used as part of the URL https://YOURWEBAPP.azurewebsites.net so it has to be unique).

3) Add it to the resource group created as part of the container registry setup.

4) In the Configure container section, select Azure Container Registry and then configure it to look at the container registry you created earlier, select the image that was pushed from your VSTS build. Then select the latest tag. This will typically be a build number, incremental so if you have multiple to chose from, select the highest. (Don't worry, it will automatically update to the latest each time you do a release, outlined in the next blog).

![Create Docker web app]({{ site.baseurl }}/img/azureCreateDockerApp.png)