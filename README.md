# Dockerized Laboratory for Bamboo + Bitbucket Workshop #

This repository contains a Vagrant + Docker recipe that starts the following Laboratory environment on a local machine:

| Application | Purpose | Docker Image | Ports |
| ------------| --------|--------------|-------|
| JIRA Core | Project Management and Bug Tracking | [`cptactionhank/atlassian-jira:7.4.3`](https://hub.docker.com/r/cptactionhank/atlassian-jira/) | `9090`|
| Bitbucket Server | SCM |[`atlassian/bitbucket-server`](https://hub.docker.com/r/atlassian/bitbucket-server/) | `7990` |
|Bamboo Server 6.0.3 | CI | Modified from [`cptactionhank/atlassian-bamboo:6.0.3`](https://hub.docker.com/r/cptactionhank/atlassian-bamboo/)| `8085`|
|Tomcat 7.0 (2 servers) |Dev and QA Environments for application under development |Modified from [`tomcat:7.0`](https://hub.docker.com/r/tomcat)|`8090` and `8100`|

## Prerequisites

* Windows 10 Pro (x64)
* Docker for Windows installed
* *Docker for Windows should be allocated at least 3 GB RAM.*
* Vagrant 1.9.7+

*Not tested with Docker Toolbox or other (non-Windows) Operating Systems.*

## How to run

* Clone this repository.
* Open a **Powershell** terminal as administrator and CD to this directory.
Execute the following commands
```
.\createNetwork.ps1
.\createVolume.ps1
vagrant up
```

This will
1. create a custom bridge docker network under which all containers are created.
1. create named Docker volumes for persistence
1. download and run pre-configured Docker containers as above.

* :exclamation: The Bitbucket and Bamboo applications take a bit of time to start up. Monitor the Docker logs with `docker logs -f <container name>`.

## Notes

### Docker Network

The Docker bridge network is created under subnet `172.18.0.0/24`. On Windows you might need to execute ` route add 172.18.0.0 mask 255.255.0.0 10.0.75.2` to create a route from your Hyper-V VM to the subnet.

### This is a Fresh Installation

* This creates a fresh Bitbucket server that needs a license. To generate a 90-day evaluation, visit: https://my.atlassian.com/license/evaluation
* After license creation you will need to paste it in the appropriate field and proceed with setup steps for both Bitbucket and Bamboo.

### Memory Considerations

:exclamation: Most Atlassian applications seem to require a high amount of RAM.
On Docker for Windows *at least 6 GB of RAM* needs to be allocated to the *overall Docker process*, otherwise you may encounter some strange symptoms:

* JIRA Plugins do not load within a timeout
* Bamboo fails to load

etc.

### Persistence Volumes

Data for Bitbucket and Bamboo is stored in directories `C:\Users\USER_NAME\bamboo-docker-volume` and `C:\Users\USER_NAME\bitbucket-docker-volume` on your Windows machine.

As long as these directories exist, when you execute `vagrant up` to start the docker containers, the data will persist.

### Viewing logs

Since ultimately this starts Docker containers you can view the logs as follows:

```
docker logs -f bamboo-server
docker logs -f bitbucket-server
```
