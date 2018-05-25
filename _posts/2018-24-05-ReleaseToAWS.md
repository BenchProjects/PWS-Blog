---
title:  Releasing a container to AWS
author: Phil Hine
date: 2018-05-24
--- 

## Overview
This post shows you how to use VSTS to release a docker image to an AWS EC2 cluster. It does not go in depth into all of the available options and is meant to teach you the simplest way to get up and running. It does not cover security or best practices.

## Prerequisites
An AWS account<br/>
A VSTS account<br/>
A windows VSTS host with docker installed and powershell<br/>
A hello world application

## Create a Elastic Container Registry (ECR)

1) Log into AWS and select region N. Virginia from the top menu on the right (just for purposes of this tutorial)<br/><br/>
2) Click on the services dropdown and enter 'container' into the filter box. Then click on Elastic Container service.<br/><br/>
![Select elastic container service]({{ site.baseurl }}/img/AWS_ECR/1_Select_ECS.PNG)<br/><br/>
3) Press the pin icon that lives alongside the top menu, this will allow you to easily access this area in future.<br/><br/>
4) In the left hand pane select Repositories then select Get Started<br/><br/>
![Select repositories]({{ site.baseurl }}/img/AWS_ECR/2_Select_Repositories.PNG)<br/><br/>
5) Enter a sensible name for your repository and take a note of your repository url although you can get this later and press Next Step.<br/><br/>
![Enter ECR name]({{ site.baseurl }}/img/AWS_ECR/3_ConfigureRepository.PNG)<br/><br/>
6) You should now see a screen that talks you through how to push an image to the registry. You can come back and view that later if you wish.<br/><br/>
![ECR Created]({{ site.baseurl }}/img/AWS_ECR/3_ConfigureRepository.PNG)<br/><br/>

## Create a Elastic Container Service (ECS) Cluster

We need to define the environments we wish to release to by creating a ECS cluster.

1) Navigate again to the ECS page, if you followed point 2 above then you should have a bookmark at the top of the page you can use to get here quickly.<br/><br/>
2) In the left pane click on clusters and in the main pane 'Create Cluster')<br/><br/>
![Create ECS Cluster]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/4_CreateCluster.png)<br/><br/>
3) Now select the cluster type, which for these set of tutorials should be set to linux and press next step.)<br/><br/>
![Select ECS Cluster Template]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/5_SelectClusterTemplate.png)<br/><br/>
4) Enter a name for your cluster. For this tutorial leave all other settings as default (it would be worth taking time to understand this once you have followed this basic example). Next press create.<br/><br/>
![Configure ECS Cluster]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/6_ConfigureCluster.png)<br/><br/>
5) On the next page you will see your environment being set up. Eventually all tasks will complete and you will have a cluster set up ready for use. Make a note of the name you gave to the cluster.<br/><br/>
![Completion ECS Cluster Screen]({{ site.baseurl }}/img/AWS_ECR_CLUSTER/6_Created.png)<br/><br/>

## Installing AWS CLI and AWS Tools

1) Log into your VSTS host machine.
2) Browse to https://aws.amazon.com/powershell/ and install the powershell tools.
3) Browse to https://aws.amazon.com/cli/ and download the appropriate msi and run it and install the aws cli.

## Setting up AWS Credentials for Powershell and AWS CLI

### AWS CLI
1) **TODO** set up AWS user
<br/><br/>

When you have created your user then you should see the following page:
![User set up completed]({{ site.baseurl }}/img/AWS_ECR/Release/user/Completed.png)<br/><br/
Make a note of your access key id and secret access key

2) Open a command prompt and type 
```
c:>aws configure
```
This should then be asked to put in your access key id from the previous step, followed by your secret access key and then your region as we selected North Virginia when setting up ECR you should use us-east-1 here. For the last option just press enter.
![AWS Configure]({{ site.baseurl }}/img/AWS_ECR/7_AWS_CLI_EnterCredentials.png)<br/><br/>

3) Test your credentials work by entering the following in the command line:<br/><br/>
```
c:>aws ecr get-login --no-include-email --region us-east-1
```
<br/><br/>
This will generate a login command which you should copy and execute in the same command prompt. If successful you will see 'Login succeeded'.<br/><br/>
![Enter login command]({{ site.baseurl }}/img/AWS_ECR/8_AWS_CLI_GetLoginDetails.png)<br/><br/>
![Enter Command Returned]({{ site.baseurl }}/img/AWS_ECR/UseAwsCli_Login_to_AWS.png)<br/><br/>

### AWS Powershell Tools

1) Open up powershell and write the following which will store the credentials neccessary to connect to AWS - choose a sensible name for -StoreAs parameter and replace access key and secret key with your newly created user's credentials set up previously in the tutorial:
```powershell
Set-AWSCredential -AccessKey <accesskey> -SecretKey <secretKey> -StoreAs MyProfileName2
```
2) Have a look at the credential profile you just set up
```powershell
Get-AWSCredential -ListProfileDetail
```
3) Set the credential to use as the name of the profile

4) Now test the login:
```powershell
Invoke-Expression -command (aws ecr get-login --no-include-email --region us-east-1)
```
If successful you will see the last line saying 'Login Succeeded'.

## Create a Powershell script to push Container to AWS ECR

There are various means of pushing your container to AWS ECR. I initially thought it would be great to use the AWS Tools plug in for VSTS but then I thought I would prefer to make something that could be run from a powershell script so that it can be used on whichever continuous delivery platform you choose.

Here is the script you can use to push to your AWS Elastic Container Repository:
```powershell
param([Int32]$tag=0, $image="", $awsRepositoryName = "")

#Example call
# AWS_DeployToECR.ps1 38 myLocalContainerImage myRemoteAWSContainerImage
# assumes myLocalContainerImage is provided without container registry prefix

#Prerequisites:
# Machine must have Docker and AWS tools installed.
# Your container image is already built on your machine.
# Your AWS credentials have already been set on this machine.
# You have an AWS ECR repository.

function login-To-ecr(){
    $loginStatement = aws ecr get-login --no-include-email --region us-east-1 
    $loginStatementArray = $loginStatement -split ' '
    $username = $loginStatementArray[3]
    $password = $loginStatementArray[5]
    $ecr_endpoint = $loginStatementArray[6]

    $imagePrefix = $ecr_endpoint -replace "https://", ""
    $tempRepositoryImageName = $imagePrefix + "/" + $awsRepositoryName + ":latest"

    echo "$password" | docker login -u $username --password-stdin $ecr_endpoint

    if (-not $?)
    {
        throw "Login failed, please check AWS credentials are set up on this host"
    } 
    
    return $tempRepositoryImageName, $imagePrefix + "/" + $image
}

function verify-parameters(){
    if ($tag -eq 0)
    {
        throw "Parameter exception, tag was not passed"
    }

    if ($image -eq "")
    {
        throw "Parameter exception, container image name was not passed"
    }

    if ($awsRepositoryName -eq "")
    {
        throw "Parameter exception, repository name was not passed"
    }

    $image = $image.ToLower()

    Write-Output "Repository name: $awsRepositoryName"
    Write-Output "Image name: $image"
    Write-Output "Tag: $tag"
}

function push-to-ecr([string]$finalRepositoryImageName = "", $repositoryPrefix = ""){

    if ($finalRepositoryImageName -eq "")
    {
        throw "Unable to determine aws repository to push to"
    }

    $taggedImageReference = ($repositoryPrefix + "/" + $image + ":" + $tag).ToLower()

    docker tag $taggedImageReference $finalRepositoryImageName.ToLower()

    Write-Output "Source image successfully tagged"

    docker push $finalRepositoryImageName.ToLower()
}

Write-Output "Script version: 1"

#1 Verify script parameters
Write-Output "Attempting to verify script parameters"
verify-parameters

#2 login in to AWS
Write-Output "Attempting to log in to AWS"
$loginOutput = login-To-ecr
$finalRepositoryImageName =$loginOutput[1]
$registryContainerPrefix = $loginOutput[2]

#3 push container image to AWS
Write-Output "Attempting to push to AWS"
push-to-ecr $finalRepositoryImageName $registryContainerPrefix


if ($?){
    "Script completed successfully"
} else {
    "Script failed"
}
```

