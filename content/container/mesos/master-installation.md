+++
date = "2015-04-09T14:34:05+08:00"
tags = ["x", "y"]
title = "Mesos Master node installation guide"

+++
install mesos-master on ubuntu 14.04 64bit <!--more-->
# master installation

## install mesos-master on ubuntu 14.04 64bit

### Test envrionment
> master1: 172.21.100.221
> 
> master2: 172.21.100.222
> 
> master3: 172.21.100.223

### installation script

```
#!/bin/bash

# Setup
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv E56151BF
DISTRO=$(lsb_release -is | tr '[:upper:]' '[:lower:]')
CODENAME=$(lsb_release -cs)

# Add the repository
echo "deb http://repos.mesosphere.io/${DISTRO} ${CODENAME} main" | \
  sudo tee /etc/apt/sources.list.d/mesosphere.list
sudo apt-get -y update

sudo apt-get -y install mesosphere

```

## configuration

### Zookeeper

`sudo vim /etc/mesos/zk`


> zk://172.21.100.221:2181,172.21.100.222:2181,172.21.100.223:2181/mesos

configure zoop unique id for each master, this example use 1,2,3 for each master

`sudo vim /etc/zookeeper/conf/myid`

> 1

`sudo vim /etc/zookeeper/conf/zoo.cfg`

> server.1=172.21.100.221:2888:3888
> 
> server.2=172.21.100.222:2888:3888
> 
> server.3=172.21.100.223:2888:3888

### Mesos Master
*Quorum*
`sudo vim /etc/mesos-master/quorum`

> 2

configure the hostname and ip address

`echo 172.21.100.221 | sudo tee /etc/mesos-master/ip`

`sudo cp /etc/mesos-master/ip /etc/mesos-master/hostname`

### Marathon
`sudo mkdir -p /etc/marathon/conf`

`sudo cp /etc/mesos-master/hostname /etc/marathon/conf`

`sudo cp /etc/mesos/zk /etc/marathon/conf/master`

`sudo cp /etc/marathon/conf/master /etc/marathon/conf/zk`

`sudo vim /etc/marathon/conf/zk`

> zk://172.21.100.221:2181,172.21.100.222:2181,172.21.100.223:2181/marathon

### configure service init rules and restart servcies
```
#stop and forbide slave process
sudo stop mesos-slave
echo manual | sudo tee /etc/init/mesos-slave.override

#start service
sudo restart zookeeper
sudo start mesos-master
sudo start marathon
```


### confirm everything is all right

> mesos service
> http://172.21.100.221:5050
> 
> marathon service
> http://172.21.100.221:8080