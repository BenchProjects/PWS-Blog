---
title:  Creating docker dev environment and hello world app
author: Phil Hine
date: 2018-05-16
--- 

## Overview
This blog describes how to set up an azure VM and create a .Net Core Web App with docker. This will provide basic details about Azure but it is not meant as a comprehensive configuration guide.

## Prerequisites
An azure account with credit.

## Create VM

1) Log into your azure account.

2) Select Virtual machines from the left hand pane or if you can't see it go to All Services and add it. Click the add button.
![Step1]({{ site.baseurl }}/img/CreateAzureVM1.png)

3) Next you need to select the base image to base your VM on. Choose the Visual Studio image and select the appropriate version you want to work with on your VM (make sure you choose Windows 10 to follow this tutorial).
![Step2]({{ site.baseurl }}/img/CreateAzureVM2.png)

4) Now you need to enter some general details for your VM. Name your VM and select your disk type (HDD is cheaper) and enter a name and password that you will use to admin the VM and press Ok.
![Step3]({{ site.baseurl }}/img/BasicSettings.png)


5)  Choose the operating system spec, I usually make the decision based on price and RAM.
![Step4]({{ site.baseurl }}/img/Size.png)

6) There are some advanced settings to fill out on the following screen. Usually I leave the default settings apart from perhaps the storage type. On the next screen understand and confirm your agreement to the licence and press create. In the background Azure is provisioning and setting up your new VM. This can take some time usually 5-10 mins after which you will get a notification to say it has completed.
![Step5]({{ site.baseurl }}/img/VMAzureSettings.png)

## Access VM
![Step6]({{ site.baseurl }}/img/VirtualMachines.png)
![Step7]({{ site.baseurl }}/img/downloadRdpFile.png)
![Step8]({{ site.baseurl }}/img/ConnectViaRdp.png)
![OptionalStep]({{ site.baseurl }}/img/resetAdminAccount.png)

