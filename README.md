# Druid Docker
Personalised druid docker setup on the host and configured with Supervisord.

 * Each node will run in separate container independently.
 * Suitable for production.
 * You might need to change conf directory according to your hardware configurations.
 * Don't forget to replicate conf in root directory to each of individual component.


*As it is my first docker project, I put maximum effort to give the best. Please suggest if there is any import things that I missed*

<br>
### Install Docker
<hr>

Follow the [docker documentation](https://docs.docker.com/engine/installation/) to install docker-engine depending on your os confiugration. We are using docker-compose to build and start the containers., so for this you need to have docker-compose which you can install by running the following command. Make sure you have pip insalled.

```
pip install docker-compose
```
*Most of \*nix system will bundle pip with the os except for Mac. If the package doesn't exists, you need to install pip*

<br>
### Clone the Repo
<hr>

You need to download/clone the repo from [here](https://github.com/Gowtham95india/druid-docker.git) or by using the following commands.

```
git clone https://github.com/Gowtham95india/druid-docker.git druid-docker-master
cd druid-docker-master
```

*If you don't have git installed, you can directly donwload it by using the following commands*
```
curl -s https://codeload.github.com/Gowtham95india/druid-docker/zip/master > druid-docker.zip
unzip druid-docker.zip
cd druid-docker-master
```

Here the proejct directory structure.
```
./druid-docker-master/
├── broker
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── conf
│   └── druid
├── coordinator
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── historical
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── middleManager
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── mysql
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── overlord
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── zookeeper
│   ├── conf
│   ├── Dockerfile
│   └── supervisord.conf
├── Dockerfile
├── LICENSE
├── README.md
└── docker-compose.yml
```
Important things regarding the project structure:
* [./druid-docker-master/Dockerfile](https://github.com/Gowtham95india/druid-docker/blob/master/Dockerfile) is sample Dockerfile which can modified and played with.
* [./druid-docker-docker-compose.yml](https://github.com/Gowtham95india/druid-docker/blob/master/docker-compose.yml) is the main file in which defined how every container should work. Pelase take backup before modifying this file.
* [./druid-docker-master/conf](https://github.com/Gowtham95india/druid-docker/tree/master/conf) contains the common configurations with which Druid should run.
* If you are planning to modify the configurations, please modify ./druid-docker-master/conf and then copy the directory to other directories ( *[Broker](https://github.com/Gowtham95india/druid-docker/tree/master/broker), [Coordinator](https://github.com/Gowtham95india/druid-docker/tree/master/coordinator), [Historical](https://github.com/Gowtham95india/druid-docker/tree/master/historical), [middleManager](https://github.com/Gowtham95india/druid-docker/tree/master/middleManager), [Overlord](https://github.com/Gowtham95india/druid-docker/tree/master/overlord)* ).

Once you are done with configuring and copy, you are ready to go! :+1:

```
docker-compose up
```

This step will donwload Ubuntu 14.04 images and will do all the necessary steps and starts container. You can see the logs of individuals nodes by using docker logs container name here. For example, you can see the broker log in the following way. Additionally you can stream the logs by using with -f flag.

```
docker logs broker
```

<br>
### Futher recommended:
<hr>

You can use swarm to run them in different hosts. You can read about [swarm here](https://docs.docker.com/swarm).

*Don't run this in your machine as it runs 7 containers which freezes the system. I'm using 16Gb Ubuuntu 14.04 Machine to run and test this.*
