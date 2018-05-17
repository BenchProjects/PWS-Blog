---
title: 4 Setting up Azure Docker container registry
author: will_andrews
date: 2018-05-16
--- 

## Overview
This blog will go through creating a Docker container registry that will be used to store your  images that are created as part of your VSTS build of your web app.

## Prerequisites

An Azure account.

## Setting up a container Registry

1) On Azure, create a new Container Registry resource.

2) Give the registry a name, resource group and enable the admin user. Change the SKU to basic if you wish for a cheaper option. Note: The location must stay as South Central US.

![Create container registry]({{ site.baseurl }}/img/azureCreateContainerRegistry.png)
