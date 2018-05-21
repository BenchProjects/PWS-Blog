---
title:  Create new docker VSTS build agent
author: Phil Hine
date: 2018-05-16
--- 

## Overview 
This is a brief post on creating a docker build agent on a machine with docker installed.

## Prerequisites
A machine or virtual machine with docker installed and powershell.

## Getting started
1) Log into the machine that has docker installed.

2) Log into VSTS. Build and Release > Agent Queues > New queue... Enter a suitable pool name for the queue.<br/><br/>

![Step1]({{ site.baseurl }}/img/VSTS_CreateDockerBuildAgent/VSTS_CreateNewQueue.png)<br/><br/>

3) With the new queue selected click on download agent. Follow the instructions <a href="https://docs.microsoft.com/en-gb/vsts/build-release/actions/agents/prepare-permissions?view=vsts">here</a> to get a personal access token from VSTS that will be used by your build agent.

4) Prepare\install the agent files. Open up powershell and type in
```
c:\>mkdir agent
c:\>cd agent
c:\Add-Type -AssemblyName System.IO.Compression.FileSystem ; [System.IO.Compression.ZipFile]::ExtractToDirectory("$HOME\Downloads\vsts-agent-win-x64-2.133.3.zip", "$PWD")
```

5) In powershell in the agent directory you just created type
```
.\config.cmd
```

You will be presented with the following options:<br/><br/>
1) Enter server url
This is the main domain name of your vsts account e.g. https://myTestAccount.visualstudio.com<br/><br/>

2) Enter authentication type
Press enter<br/><br/>

3) Enter personal access token
This is the token you created in VSTS. At this point it tries to connect, any network rules might present themselves here and you may have to configure a proxy (out of scope of this tutorial)

4) Enter agent pool
This is the name of the pool you set up earlier in this tutorial

5) Enter agent name
This is the name you will see in VSTS in the list of available\compatible agents for your pool

6) Enter work folder
Press enter<br/><br/>

7) Run as a service
Say yes to this otherwise you will manually have to run the service every time your agent machine starts.

6) Enter user account to use for this service
You need to enter a user that has admin rights to docker on your machine. For tutorial purposes I used my azure vm admin user.

![Step7]({{ site.baseurl }}/img/VSTS_CreateDockerBuildAgent/configureTheAgentProperties.png)<br/><br/>

7) Once the agent has been set up and is running ***FIGURE OUT IF ADD STEPS REQUIRED*** go back to VSTS and go to settings > agent queues > 