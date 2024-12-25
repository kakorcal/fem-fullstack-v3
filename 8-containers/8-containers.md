# Containers

## Terminology

* Microservices - software architecture of loosely connected services
* Monolith - software architecture of tightly coupled services
* Virtualization - slicing up a server into chunks (VPS)
* Containerization - Virtualization + microservices. Enables zero down-time deploys. Operating system independant (agnostic). You are able to achieve multiple instances of your application on one VPS
* Orchestration - Manages containers
* Display running processes - `htop`

## Container services

* Docker
* Amazon EC2
* Apache Mesos
* CoreOS rkt

## Docker
* Install docker on server - `sudo apt install docker.io`
* Create docker container with DockerFile

```
FROM node:19-alpine3.16
RUN mkdir -p /home/node/app/node_modules && chown -R node:node /home/node/app
WORKDIR /home/node/app
COPY package*.json ./
USER node
RUN npm install
COPY --chown=node:node . .
EXPOSE 3000
CMD [ "node", "app.js" ]
```

* Run DockerFile - `sudo docker build -t <my-container> <my-directory>`
* Run application - `sudo docker run -d -p 3000:3000 <image-id>`

## Orchestration services

* Kubernetes (k8s)
* Docker swarm
* Amazon EKS
* Apache Mesos
* AKS

## Load balancing to each container instance with Nginx

### Scheduling algorithms

* Round robin
* IP hashing
* Random choice
* Least connection
* Least load

### Nginx config

```
upstream myloadbalancer {
  least_conn;
  server backend1.example.com;
  server backend2.example.com;
  server 192.0.0.1 backup;
}

server {
  location / {
    proxy_pass http://myloadbalancer;
  }
}
```
