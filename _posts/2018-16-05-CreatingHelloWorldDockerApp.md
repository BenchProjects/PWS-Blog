---
title:  3 Creating docker enabled Hello world app
author: philip_hine
date: 2018-05-16
--- 

## Overview
This blog describes the very minimum steps of getting a hello world application to be configured for docker.

## Prerequisites
You have docker installed (see https://benchprojects.github.io/PWS-Blog/CreateDevEnvironment/)
You have a version of visual studio 2017 installed capable of developing .net Core applications with docker features switched on.

## Create project
Open up docker settings and share your local drives. ** enter reason

1) File > New > Project 
Create a new solution based on the template ASP.Net Core Web Application named HelloWorld.
![Step1]({{ site.baseurl }}/img/HW.CreateSolution.png)<br/><br/>

2) Select the following options (note you enable docker in this step):
![Step2]({{ site.baseurl }}/img/HW.ApplicationSelection.png)<br/><br/>

3) Once the project scaffolding has been set up you should be left with an additional project called docker-compose. This contains two files docker-compose.yml and .dockerignore. As you would expect the latter is similar to a .gitignore file. In addition you will notice a dockerfile in your main project. 
![Step3]({{ site.baseurl }}/img/HW.ProjectFiles.png)<br/><br/>

4) Edit your dockerFile so it looks like the below:
![DockerFile]({{ site.baseurl }}/img/HW.DockerFile.png)<br/><br/>

This essentially copies the hello world files to the src/HelloWorld directory in the container. The dependencies are restored and the project is built and output to the app folder within the container.

Each time the WORKDIR command is used it set the working directory in the container for all further commands to execute on. The directories are created as needed. COPY . . is the instruction to copy all files from the project into the container. The FROM statements point to base images that contain the files need to build, run the code. 


5) Make sure the docker-container project is set as the start up project and press run to run with docker. If successful it should open your website in a new browser.
![Step5A]({{ site.baseurl }}/img/HW.RunningSite.png)<br/><br/>

You can also view the running container by using a command prompt and typing 'docker ps':
![Step5B]({{ site.baseurl }}/img/HW.CheckContainer.png)<br/><br/>










