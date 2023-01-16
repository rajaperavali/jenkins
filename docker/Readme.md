### Overview

Deploy in container and demostrate Java Build and Package using containers.


### Installing Jenkins

Create a bridge network and volumes in Docker 

```
docker network create jenkins

docker volume create jenkins-docker-certs

docker volume create jenkins-data
```

Create docker:dind to execute docker commands within the jenkins node

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
  
Build a new Jenkins Image with docker cli

```
docker build -t myjenkins-blueocean:2.375.2-1 .
```

Create Jenkinks container to run Jenkins CI/CD
```
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.375.2-1
```
  
### Verify and Login Jenkins

> http://localhost:8080

### Exract the admin password from jenkins

```
docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```


