---
title: 2 Setting up Docker on Windows 10 Virtual Machine
author: will_andrews
date: 2018-05-16
order: 2
--- 

## Overview
This blog goes through installing and setting up Docker on a virtual machine.

## Prerequisites
Virtual machine with Windows 10 installed

Installer for Docker from the docker store https://store.docker.com/editions/community/docker-ce-desktop-windows


## Installing Docker
1) Run the installer as admin

2) The installer will run and download a package. When this has finished, mae sure that the tick box to use Windows containers is IS selected and then press OK. This will then install.

![Install and use Windows Containers]({{ site.baseurl }}/img/dockerInstallUseWindows.png)

3) Once finished, it will tell you that you need to log out of Windows to complete, so select the option to do this.

4) Once logged back in, run Docker using the shortcut placed on the desktop. It will tell you that Hyper-V and containers features are not enabled. Press Ok to enable them and let it restart your virtual machine. (It may take a while for it to do this.)

![Enable Hyper-V]({{ site.baseurl }}/img/dockerEnableHyperV.png)


5) Once the virtual machine has restarted, log back on. (If you're not using static IP in Azure, the IP address may have changed. Check from the Azure portal).

6) Once logged in, run Docker using the shortcut on the desktop and wait for it to start. It may show you a toast letting you know it's starting. Or in the notification area on your virtual machine, the Docker icon will show you it's starting.

![Starting]({{ site.baseurl }}/img/dockerStarting.png)

![Starting]({{ site.baseurl }}/img/dockerStarting2.png)


## Configuring Docker

1) Once Docker has launched, you will see a screen telling you that Docker is up and running! It will also prompt you to login with a Docker Id. This needs to be done so that you can access the Docker container repository. 

![Login]({{ site.baseurl }}/img/dockerLogin.png)

2) Once logged in, right click the icon in the system tray and select the option to switch to Linux containers. This is because if you select this when installing Docker, it won't always install properly. So when installing you set it to use Windows, and then once installed you change it to use Linux. This may take some time.

![Switch to Linux containers]({{ site.baseurl }}/img/dockerSwitchToLinux.png)


That's it. You virtual machine is ready to work with Docker containers.







