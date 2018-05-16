---
title:  Creating docker dev environment and hello world app
author: Phil Hine
date: 2018-05-16
--- 

## Overview
This blog describes how to set up an azure VM and create a .Net Core Web App with docker. This will provide basic details about Azure but it is not meant as a comprehensive configuration guide.

## Prerequisites
An azure account with credit.
Remote desktop client.
Network access to use remote desktop.

## Create VM

1) 
Log into your azure account.


2) 
Select Virtual machines from the left hand pane or if you can't see it go to All Services and add it. Click the add button.
![Step1]({{ site.baseurl }}/img/CreateAzureVM1.png)


3) 
Next you need to select the base image to base your VM on. Choose the Visual Studio image and select the appropriate version you want to work with on your VM (make sure you choose Windows 10 to follow this tutorial).
![Step2]({{ site.baseurl }}/img/CreateAzureVM2.png)


4) 
Now you need to enter some general details for your VM. Name your VM and select your disk type (HDD is cheaper) and enter a name and password that you will use to admin the VM and press Ok.
![Step3]({{ site.baseurl }}/img/BasicSettings.png)


5)  
Choose the operating system spec, I usually make the decision based on price and RAM.
![Step4]({{ site.baseurl }}/img/Size.png)


6) 
There are some advanced settings to fill out on the following screen. Usually I leave the default settings apart from perhaps the storage type. On the next screen understand and confirm your agreement to the licence and press create. In the background Azure is provisioning and setting up your new VM. This can take some time usually 5-10 mins after which you will get a notification to say it has completed.
![Step5]({{ site.baseurl }}/img/VMAzureSettings.png)

## Access VM

1) 
In your azure portal navigate to Virtual Machines in the left hand pane or go to all settings and select it. As you might expect this page lists all of your virtual machines. Click on the name of the one you have just created.
![Step6]({{ site.baseurl }}/img/VirtualMachines.png)


2) 
At the top you will see the connect icon which allows you to download an rdp file. Go ahead and click on it and download the file.
![Step7]({{ site.baseurl }}/img/downloadRdpFile.png)


3) 
Once the file has downloaded open up the file add appropriate details such as proxy settings and press connect. Here you will need to input the admin user credentials. After which you should be logged into the system.
![Step8]({{ site.baseurl }}/img/ConnectViaRdp.png)

Note if you need to reset the admin creds you can do that in the virtual machine settings on the azure portal.
![OptionalStep]({{ site.baseurl }}/img/resetAdminAccount.png)

