---
title:  Creating docker enabled Hello world app
author: Phil Hine
date: 2018-05-16
--- 

## Overview
This blog describes the very minimum steps of getting a hello world application to be configured for docker.

## Prerequisites
You have docker installed (see https://benchprojects.github.io/PWS-Blog/CreateDevEnvironment/)
You have a version of visual studio 2017 installed capable of developing .net Core applications with docker features switched on.

## Create project
1) File > New > Project 
Create a new solution based on the template ASP.Net Core Web Application named HelloWorld.
![Step1]({{ site.baseurl }}/img/HW.CreateSolution.png)<br/><br/>

2) Select the following options (note you enable docker in this step):
![Step2]({{ site.baseurl }}/img/HW.ApplicationSelection.png)<br/><br/>

3) Once the project scaffolding has been set up you should be left with an additional project called docker-compose. This contains two files docker-compose.yml and .dockerignore. As you would expect the latter is similar to a .gitignore file. In addition you will notice a dockerfile in your main project. 
![Step3]({{ site.baseurl }}/img/HW.ProjectFiles.png)<br/><br/>

4) Edit your dockerFile so it looks like the below:
![DockerFile]({{ site.baseurl }}/img/HW.ProjectFiles.png)<br/><br/>

In the file you can see the FROM instructions which point at base dotnet core images which contain the basic files necessary to run a .net core app. Each time you see WORKDIR this indicates we want the following commands to be run in that location within the container. If they don't exist they will be created. Sometimes it can be useful to use RUN commands for debug purposes such as RUN dir which will list the files within the current work directory for the container. You can see the bit that copies the HelloWorld project then restores all of the dependencies. The part that says COPY . . essentially says copy all files to the containers current working directory.







