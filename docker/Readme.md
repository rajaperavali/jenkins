### Overview

Deploy in container and demostrate Java Build and Package using containers.


### Installing Jenkins

Create a bridge network and volumes in Docker 

```
docker network create jenkins

docker volume create jenkins-docker-certs

docker volume create jenkins-data
```

Setup the docker:dind to execute docker commands within the jenkins node

```
docker run --name jenkins-docker --rm --detach \
  --privileged --network jenkins --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  --storage-driver overlay2 \
  docker:dind 
  ```
  
  
  
