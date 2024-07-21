# CI/CD with Jenkins

## Jenkins

Jenkins is an automation platfrom that allows you to build, test and deploy softwares using pipeline.
It is basically just a way to automate the work that developers do not want to do. So they can save their time
and dome more productive things.

<p align="center">
    <img src="images/jenkins1.png" alt="workflow" width="%100" height="%100" style="border-radius: 20px">
</p>

But, it is not just limited to creating pipelines for code. It can be used to automate any tasks.
Jenkins provides a web UI where you can create jobs and customize all the functionality that you want
such as source control management, pre and post build actions as well as build triggers. This allows you
to run tasks on demand by clicking a button or have them triggered automatically via web hooks.

## Jenkins Features

- `Continuous Integration and Continuous Delivery`: As an extensible automation server, Jenkins can be used as a simple CI server or turned into the continuous delivery hub for any project.
- `Easy installation`: Jenkins is a self-contained Java-based program, ready to run out-of-the-box, with packages for Windows, Linux, macOS and other Unix-like operating systems.
- `Easy configuration`: Jenkins can be easily set up and configured via its web interface, which includes on-the-fly error checks and built-in help.
- `Plugins`: With hundreds of plugins in the Update Center, Jenkins integrates with practically every tool in the continuous integration and continuous delivery toolchain
- `Extensible`: Jenkins can be extended via its plugin architecture, providing nearly infinite possibilities for what Jenkins can do.
- `Distributed`: Jenkins can easily distribute work across multiple machines, helping drive builds, tests and deployments across multiple platforms faster

## Jenkins Infrastructure

First, you have the master server which controls the pipelines and schedules builds to agents.
Then, you have the agents which run the build in their workspace.

<p align="center">
    <img src="images/jenkins2.png" alt="workflow" width="%100" height="%100" style="border-radius: 20px">
</p>

An example workflow may look like this:

1. A developer commits some code to a repository,
2. The jenkins master becomes aware of this commit and triggers the appropriate pipeline,
3. And distributes the build to one of agents to run,
4. It selects the appropriate agent based on labels which issomething that you can configure through the jenkins ui,
5. The agent runs the build which is usually just a bunch of linux commands to build, test and distribute your code.

## Build Types

<p align="center">
    <img src="images/jenkins3.png" alt="workflow" width="%100" height="%100" style="border-radius: 20px">
</p>

<p align="center">
    <img src="images/jenkins4.png" alt="workflow" width="%100" height="%100" style="border-radius: 20px">
</p>

Pipelines are commonly broken into different stages.
As you can see, this pipeline is broken into five
different stages. These stages can differ depending
on the project.

- The clone stage is usually made of pulling down the code
  from a git repository and setting up the local environment
  on the agent.
- The build stage takes the code and builds it which means
  generating some type of local artifact like a jar file or
  executable.
- The test stage runs tests against newly built code.
- The package stage packages up it so it is ready for deployment.
- In the deploy stage, the artifact is sent to registry.

## Installation with Docker

### Build the Jenkins BlueOcean Docker Image (or pull and use the one I built)

```
docker build -t myjenkins-blueocean:2.414.2 .

#IF you are having problems building the image yourself, you can pull from my registry (It is version 2.332.3-1 though, the original from the video)

docker pull devopsjourney1/jenkins-blueocean:2.332.3-1 && docker tag devopsjourney1/jenkins-blueocean:2.332.3-1 myjenkins-blueocean:2.332.3-1
```

### Create the network 'jenkins'

```
docker network create jenkins
```

### Run the Container

#### MacOS / Linux

```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.414.2
```

#### Windows

```
docker run --name jenkins-blueocean --restart=on-failure --detach `
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
  --volume jenkins-data:/var/jenkins_home `
  --volume jenkins-docker-certs:/certs/client:ro `
  --publish 8080:8080 --publish 50000:50000 myjenkins-blueocean:2.414.2
```

### Get the Password

```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

### Connect to the Jenkins

```
https://localhost:8080/
```
