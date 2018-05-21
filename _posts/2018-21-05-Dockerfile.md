---
title:  Understanding docker file
author: Phil Hine
date: 2018-05-16
--- 

## Overview 
The aim of this post is to give you a better understanding of crafting Dockerfiles for .Net projects.


A docker file is a text file that seems similar to a .bat file. A list of commands to be run in order against the command line to create a container image.

Each instruction in the Dockerfile adds a new “layer” to the image, with layers representing a portion of the images file system that either adds to or replaces the layer below it. Layers are key to Docker’s lightweight yet powerful structure.

So the following happens on every instruction:</br>
1) Create a temporary container from the previous image layer (or the base FROM image for the first command<br/>
2) Run the Dockerfile instruction in the temporary "intermediate" container<br/>
3) Save the temporary container as a new image layer.

## Example

```docker
ARG CACHEBUST=103
FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /app

COPY *.sln .  
COPY HelloWorld/HelloWorld.csproj ./HelloWorld/ 
COPY HelloWorldClassLibrary/HelloWorldClassLibrary.csproj ./HelloWorldClassLibrary/ 
COPY HelloWorldTests/HelloWorldTests.Tests/HelloWorldTests.Tests.csproj ./Tests/ 

RUN dotnet restore ./HelloWorldClassLibrary/HelloWorldClassLibrary.csproj
RUN dotnet restore ./HelloWorld/HelloWorld.csproj
RUN dotnet restore ./Tests/HelloWorldTests.Tests.csproj

COPY HelloWorld/. ./HelloWorld/ 
COPY HelloWorldClassLibrary/. ./HelloWorldClassLibrary/
COPY HelloWorldTests/HelloWorldTests.Tests/. ./Tests/

RUN dotnet build ./HelloWorldClassLibrary/HelloWorldClassLibrary.csproj -c Release
RUN dotnet build ./HelloWorld/HelloWorld.csproj -c Release
RUN dotnet build ./Tests/HelloWorldTests.Tests.csproj -c Release

FROM build AS testrunner
WORKDIR /app/tests
ENTRYPOINT ["dotnet", "test","--logger:trx"]

FROM build AS test
WORKDIR /app/Tests
RUN dotnet test

FROM test AS publish
WORKDIR /app/HelloWorld
RUN dotnet publish -c Release -o out

FROM microsoft/aspnetcore:2.0 AS runtime
WORKDIR /app
COPY --from=publish /app/HelloWorld/out ./
ENTRYPOINT ["dotnet", "HelloWorld.dll"]

```

Let's breakdown the example:
```docker
ARG CACHEBUST=103
FROM microsoft/aspnetcore-build:2.0 AS build
WORKDIR /app
```
Whilst making the docker file you will want it to use your latest file rather than using a cached version so you can see the effects. The easiest way I found to do this is put in a different fake ARG statement on every change and adjust the value to ensure it treats everything following as new. I chose the name CACHEBUST but you could have anything.

The FROM statement says check locally for an image called Microsoft/aspnetcore-build version 2.0 if it exists create a temporary container image with it otherwise try and grab it from hub.docker.com.  The next line says in the new temporary container set the working directory to the app folder. As it is a new folder then it will create it. 

```docker
COPY *.sln .  
COPY HelloWorld/HelloWorld.csproj ./HelloWorld/ 
COPY HelloWorldClassLibrary/HelloWorldClassLibrary.csproj ./HelloWorldClassLibrary/ 
COPY HelloWorldTests/HelloWorldTests.Tests/HelloWorldTests.Tests.csproj ./Tests/ 

RUN dotnet restore ./HelloWorldClassLibrary/HelloWorldClassLibrary.csproj
RUN dotnet restore ./HelloWorld/HelloWorld.csproj
RUN dotnet restore ./Tests/HelloWorldTests.Tests.csproj

COPY HelloWorld/. ./HelloWorld/ 
COPY HelloWorldClassLibrary/. ./HelloWorldClassLibrary/
COPY HelloWorldTests/HelloWorldTests.Tests/. ./Tests/

RUN dotnet build ./HelloWorldClassLibrary/HelloWorldClassLibrary.csproj -c Release
RUN dotnet build ./HelloWorld/HelloWorld.csproj -c Release
RUN dotnet build ./Tests/HelloWorldTests.Tests.csproj -c Release
```

In the above section you can see that we are instructing the daemon to copy the solution file to the current directory which was set previously to root/app folder. Then it copies the project files from there folders to the equivalent folders in the container. Note that ```./``` is the same as ```app/``` as ```.``` means current working directory.

Then we are running the dotnet cli to restore the dependencies from the three projects inside the temporary container. 

The next copy commands copy the rest of the files from the specified folders into the specified location within the temporary container.

Next the dotnet CLI is used to build the projects in a strict order from within the temporary container.

```docker
FROM build AS testrunner
WORKDIR /app/tests
ENTRYPOINT ["dotnet", "test","--logger:trx"]
```

The statement above sets an entry point for testrunner so that it can be run at any time via the command line and should output a log called logger.trx. It can be called later on the command line with 
```
docker build --pull --target testrunner -t helloworld:test .
docker run --rm -v c:\TestResults:/app/Tests/TestResults helloworld:test
```

```
FROM build AS test
WORKDIR /app/Tests
RUN dotnet test
```
The above runs the tests that have been put in the ./app/Tests folder. If even one fails then the build process will abort. You can also see the results whilst the container image is being built.


```docker


FROM microsoft/aspnetcore:2.0 AS runtime
WORKDIR /app
COPY --from=publish /app/HelloWorld/out ./
ENTRYPOINT ["dotnet", "HelloWorld.dll"]
```
The last stage says 

## Comments
To add a comment use a hash symbol:
```docker
# This is a comment
```

## Format
The first command to appear in the file other than a comment should be either FROM or ARG followed on the next line by FROM or a parser directive.

## Parser directive
Parser directives instruct the docker daemon on how to read\treat the file and therefore must be at the top of the file and must not have any spaces before them. As soon as any line has anything but a parser directive all subsequent parser directives are treated as a comment.

At the time of writing there is only one supported which is escape. ***NEED TO UNDERSTAND THIS AS DOC IS CONFUSING***

```docker
# escape =`
```

## FROM command

The FROM command defines the base image to use either from a repository such as hub.docker.com or a private repository or even from an image created previously in the dockerfile. When a FROM command is encountered it looks locally first to try and find the image and if not found it will then look in the repos for it. During this command it makes a new temporary container and the image is copied to the container root (/)

```docker
# Grab the aspnet build image from hub.docker.com
FROM microsoft/aspnetcore-build:2.0 AS build-env

# Check to see what files and folders have been placed in the root.
RUN ls -1
```

```docker
# Creates a new temporary container using the docker image on hub.docker.com
FROM microsoft/aspnetcore-build:2.0 AS build
...

# Creates a new temporary container (intermediate container) using the one above.
FROM build as TestRunner
...
```

***Multi Stage Builds***
Using multiple FROM statements acts as defining multiple container build stages. The final FROM will determine what will be delivered into your container image.

If you want to build on the host environment during container build then you'll need all of the support files this can add up to a huge size. The aspnetcore-build:2.0 comes in around 1.79Gb. To optimise this and only put into the container the files neccessary for running the application then you can have multiple build stages (multiple from statements). The last FROM statement determines what will be packaged into your container image and therefore is important that the size of your base image here is as small as possible.

```docker
FROM test AS publish
WORKDIR /app/HelloWorld
RUN dotnet publish -c Release -o out

FROM microsoft/aspnetcore:2.0 AS runtime
WORKDIR /app
COPY --from=publish /app/HelloWorld/out ./
ENTRYPOINT ["dotnet", "HelloWorld.dll"]s
```

Above you can see a new stage being called publish taking a copy of the previous container that was called test. Then it uses the dotnet publish command to take all files that are needed for the site to run and puts them into the /app/HelloWorld/out folder. The -o is the output argument.

The last stage will be the final image that will make up the container. It takes an image for the dotnet core runtime files and copies the files in the app/HelloWorld/out folder from the publish container and puts them into the current container. The last line sets the default action when the container is run, in this case it will start the HelloWorld application using the dotnet cli.

## EntryPoint
This defines what application to run when the container image is run.


## Expose
This instruction makes the container available on the specified port for container to container communication on the same machine. ```EXPOSE 80```
Later on you can configure port forwarding 
```docker
docker run -p 8080:80 myApp
```

The -p argument sets the port that will be used to talk to the container. So you can access it on http:\\localhost:8080


## COPY
The copy command copies files from the dockerfile directory or sub directories to the container. On the left side of the argument is the file(s) to copy and the right is the location to copy them to in the container.

```
COPY utils/*.csproj ./utils/
```
## RUN
A run command will execute a command line instruction using the current working directory of the container.


## Working directory
You may need to change the working directory for subsequent commands. A common need is once you have specified your base image then you may want to use the RUN command to look at the different directories e.g RUN ls  (list of all files and folders in directory) among other things you may want to do such as dotnet restore etc.

You should note that you should be careful if you want a new folder that one with the same name doesn't already exist that came with the base image as it could have side effects you weren't expecting.

## Tips

The wild card syntax used in COPY statements is bash. 

If you wish to see what is happening you could just open a command window and navigate to the same directory as the docker file and run the following:
```docker
Docker build -f <name of your docker file> -t <Image output name>
```

It is sometimes useful to change directory with the WORKDIR command and look at the file contents with RUN ls.


```docker
Docker image ls
```

```docker
Docker ps -a
```


```docker 
Docker run -p 8080:80 MyNewImage
Docker run -p <port you wish to expose>:<container exposed port> <Image output name>
```
The above creates a new container based on the container image 'MyNewImage' and exposes it locally on port 8080. So you could see the output by typing into the address "http://localhost:8080"


