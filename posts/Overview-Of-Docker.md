---
title: 'Overview of Docker'
date: 'August 28, 2022'
excerpt: 'Docker is an open source platform that enables developers to build, deploy, run, update and manage containers—standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment'
cover_image: '/images/posts/Docker.png'
---

# what is Docker
Docker is an open source platform that enables developers to build, deploy, run, update and manage containers—standardized, executable components that combine application source code with the operating system (OS) libraries and dependencies required to run that code in any environment.

## docker vs virtual machine
<img src="https://user-images.githubusercontent.com/34083808/182397949-6b56cc54-0e08-4028-92f2-eef118af1ad6.png" width="400">&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://user-images.githubusercontent.com/34083808/182397971-e9ab668d-d502-48cf-9d21-d93d165c7c4c.png" width="400">

## How this all works together?. <br>
Docker client gives a call to Docker host to run a container from a particular image. If Docker host has that image, it run a container from it. If it does not have the image, it looks for it in the registry and pulls it on the host and then runs a container from it. Note that any of these three components can be on the same machine or on different machines. <br><br>

![image](https://user-images.githubusercontent.com/34083808/182399712-5475ff01-c8f9-494d-ad4f-4e55fbe6f428.png)

## Docker Setup
please refer to this link when [install docker](https://docs.docker.com/desktop/install/mac-install/)
![image](https://user-images.githubusercontent.com/34083808/181879839-1846836c-812c-4b50-886a-fb066fe8c60e.png)

* I recommend you try using [DigitalOcean](https://cloud.digitalocean.com/) to create your own enviromon on linux. 
 *I put the link for 60 days free and 100$ if your want to try [![DigitalOcean Referral Badge](https://web-platforms.sfo2.cdn.digitaloceanspaces.com/WWW/Badge%201.svg)](https://www.digitalocean.com/?refcode=97b78c07fcaf&utm_campaign=Referral_Invite&utm_medium=Referral_Program&utm_source=badge)*
### install docker on linux follow below instructor link. 
https://docs.docker.com/engine/install/ubuntu/

### install docker engine
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
# Basic Docker commands
## List all running containers
```
sudo docker ps
````

## List all running and stopped containers
```
sudo docker ps -a
```

## List all docker images
```
sudo docker images
```

## pull docker images 
```
sudo docker pull [image name]
```

## run docker 
```
sudo docker run -d -p 3000:80 --name [docker name] --rm [image name] 
```

## docker logs
```
sudo docker logs
```

## stop running docker
```
sudo docker stop <container id/name>
```
## Remove container
```
sudo docker rm <container id/name>
```

## Remove docker images
```
sudo docker rmi <imageid/image name>
```
# Networking: (Cross) Container communication.
three scenario of container comunication.<br>

![image](https://user-images.githubusercontent.com/34083808/181916942-4060e240-e0cb-4827-b4f6-49b40687dc50.png)


## Docker build
```
docker build -t myimage:1.0 .
```

## docker run 
```
docker run -d --name favorites --rm -p 3000:3000 favorites-node
```
## docker stop 
```
docker stop [container name]/[container id]
```
# Docker volume and bind mounts
![image](https://user-images.githubusercontent.com/34083808/182376823-52b42c53-8352-4f25-bff7-cf2ce63c395d.png)<br>

## Named volumes (Add -v command)(the name here is feedback which could be anything)
```
docker run -d --name runner_1 -v feedback:/app/feedback -p 8000:80 --rm testDocker:01 
```
## remove unuse volume
```
docker volume rm VOL_NAME
or 
docker volume prune
```
*If you start a container without ```--rm```, the anonymous volume would NOT be removed, even if you remove the container (with docker rm ...).*
## Bind Mounts
```
docker run -d --name runner_1 -v feedback:/app/feedback -p 8000:80 --rm -v [absolute path of host machine]:/app testDocker:01 
```
> as default, volume is read/write. if we want to protect our volume on host machine we can add another option to restrict docker from overwrite to folder on our host machine. 
```
docker run -d --name runner_1 -v feedback:/app/feedback -p 8000:80 --rm -v [absolute path of host machine]:/app:ro testDocker:01 
```
## Bind Mounts - Shortcuts
Just a quick note: If you don't always want to copy and use the full path, you can use these shortcuts:
* macOS / Linux: ```-v $(pwd):/app```
* Windows: ```-v "%cd%":/app```

## Understanding Container / Volume Interaction <br>
![image](https://user-images.githubusercontent.com/34083808/182402257-d81e0643-919e-4d76-a9fd-d821fe5c33ee.png)

>*An Anonymous Volume can protect the data on container from being overite by the bind mounts mapping*
```
docker run -d --name runner_1 -v feedback:/app/feedback -p 8000:80 --rm -v [absolute path of host machine]:/app  -v /app/node_modules testDocker:01 
```
## summary
![image](https://user-images.githubusercontent.com/34083808/182403685-bd66ef9b-cce1-43a7-82f1-5edffef64222.png)<br>
![image](https://user-images.githubusercontent.com/34083808/182500252-7fb3581c-cbdd-416a-a7cc-0ca837fe8361.png)<br>

## ARGuments & ENVironment Variables.<br>
![image](https://user-images.githubusercontent.com/34083808/182502339-04f846ac-555c-4e2e-a1aa-0784a7effc6b.png)

## working with Environment variable & ".env".
```
# docker file
ENV PORT 80

EXPOSE $PORT
```
```
# docker command
docker run -d --name runner_1 -v feedback:/app/feedback -p 8000:8000 --env PORT = 8000 --rm -v [absolute path of host machine]:/app  -v /app/node_modules testDocker:01 
```
> We can add more evn variable by continuing adding ```--evn or -e variableName = value```
> we can also add evironment variable to .env file and using below command to run docker. 
```
docker run -d --name runner_1 -v feedback:/app/feedback -p 8000:8000 --env-file ./.env --rm -v [absolute path of host machine]:/app  -v /app/node_modules testDocker:01 
```
## Using Build Arguments (ARG). 
```
#Docker file

ARG DEFAULT_PORT 80

EXPOSE $DEFAULT_PORT
```

```
docker build -t [image name]:[tag]  --build-arg DEFAULT_PORT = 8000 . 
```
# create container network
![image](https://user-images.githubusercontent.com/34083808/181917149-58d64754-3d9e-4cda-9183-94c5fa736ad5.png)<br>
> Within a Docker network, all containers can communicate with each other and IPs are automatically resolved.
## Containers & Network Requests.<br>
![image](https://user-images.githubusercontent.com/34083808/183251251-d12a3a5a-ceb0-4a9f-970c-f6b2c3aea4cb.png)

## Understanding Docker Network IP Resolving.<br>
![image](https://user-images.githubusercontent.com/34083808/183251326-baa0d104-433e-4d4e-a5ca-16f6db1aa970.png)


## create container network
```
docker network create favorites-net
docker run -d --name mongodb --network favorites-net mongo
```
## Docker Network Drivers
Docker Networks actually support different kinds of "Drivers" which influence the behavior of the Network.<br>
The default driver is the "bridge" driver - it provides the behavior shown in this module (i.e. Containers can find each other by name if they are in the same Network).
The driver can be set when a Network is created, simply by adding the ```--driver``` option.
```
docker network create --driver bridge my-net
```
Of course, if you want to use the "bridge" driver, you can simply omit the entire option since "bridge" is the default anyways.<br>
Docker also supports these alternative drivers - though you will use the "bridge" driver in most cases:
* **host**: For standalone containers, isolation between container and host system is removed (i.e. they share localhost as a network).
* **overlay**: Multiple Docker daemons (i.e. Docker running on different machines) are able to connect with each other. Only works in "Swarm" mode which is a dated / almost deprecated way of connecting multiple containers
* **macvlan**: You can set a custom MAC address to a container - this address can then be used for communication with that container
* **none**: All networking is disabled.
* **Third-party plugins**: You can install third-party plugins which then may add all kinds of behaviors and functionalities.<br>
As mentioned, the "bridge" driver makes most sense in the vast majority of scenarios.

# Build multi application with docker
Build the multi appication with front end, back end  and mongo database. 

## dockerizing mongo database service. <br>
![image](https://user-images.githubusercontent.com/34083808/184476501-a9d216dd-c713-4b5d-884e-6c75bfbf8858.png)

please refer to [this site](https://hub.docker.com/_/mongo) on docker hub website for more information on how to use mongo database with docker.

```
docker run --name mongodatabase -d --rm -p 27017:27017 mongo
```

## dockerizing backend server. <br>
create the dockerfile to build the backend image. 
```
FROM node:14

WORKDIR /app

COPY package.json /app

RUN npm install

COPY . /app

EXPOSE 80

CMD ["node","app.js"]

```
modify the localhost:27017 :arrow_right: host.docker.internal:27017 to able to connect with mongo database.
```
docker run --name backend_app -d --rm -p 80:80 backend:1.1
```

## dockerizing front end server. <br>

create the dockerfile to build the frontend image. 
```
FROM node:14

WORKDIR /app

COPY package.json .

RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "start"]
```

```
docker run --name frontend_app -d --rm -p 3000:3000 frontend:1.1
```

## make docker networks.
create docker networks.
```
docker network create app-net
```
run the container with networks.
before running the back end server, we need to change the IP address of mongo db to container name in order to comunicate with mongodb container. 
```host.docker.internal:27017``` :arrow_right: ```mongodb:27017```
the IP adress in the front end app also need to change to the backend container name. 
``` localhost/goals``` :arrow_right: ``` backend_app/goals```
```
docker run --name mongodb -d --rm --network goals-net mongo

docker run --name backend_app -d --rm -p 80:80 --network goals-net backend:1.1

docker run --name frontend_app -d --rm  -p 3000:3000 frontend:1.1
```

> there is one problem if we stop MongoDB and run it again, the data from it will be removed and we lost our data. As a result, we need to add the volume to prevent data from being lost when the container is stopped. For inspecting purposes, a bind mount can also be used to store data on a local host machine. <br>  
```
docker run --name mongodb -d --rm -v data:/data/db --network goals-net mongo
```
> mongodb image also supports ```MONGO_INITDB_ROOT_USERNAME``` and ```MONGO_INITDB_ROOT_PASSWORD``` for creating a simple user with the role root in the admin authentication database, as described in the Environment Variables section above.<br>

```
docker run --name mongodb -d --rm -v data:/data/db --network goals-net -e MONGO_INITDB_ROOT_USERNAME=Yushin -e  MONGO_INITDB_ROOT_PASSWORD=admin mongo
```
> Create volume for persit the log data from back end app and bind mount to update source code from our host machine. 
```
docker run --name backend_app -d --rm -p 80:80 -v "D:\Source\Git\Docker\multi-01-starting-setup\backend":/app -v logs:/app/logs  -v :/app/node_modules --network goals-net backend:1.1
```

> we can add environment variable to control the authentication of mongodb from outside of container. the environment variable can be declare in docker file and replace with username and password in sever.js file. <br>
```
EXPOSE 80

ENV MONGODB_USERNAME=root
ENV MONGODB_PASSWORD=admin

CMD ["npm","start"]
```

``` javascript
mongoose.connect(
  `mongodb://${process.env.MONGODB_USERNAME}:${process.env.MONGODB_PASSWORD}@mongodb:27017/course-goals?authSource=admin`,
  {
    useNewUrlParser: true,
    useUnifiedTopology: true,
  },
```
```
docker run --name backend_app -d --rm -p 80:80 -v "D:\Source\Git\Docker\multi-01-starting-setup\backend":/app -v logs:/app/logs  -v :/app/node_modules --network goals-net -e MONGODB_USERNAME=yushin backend:1.1
```
> Create volume for persit the log data from front end app and bind mount to update source code from our host machine. 
```
docker run -v "D:\Source\Git\Docker\multi-01-starting-setup\frontend\src":/app/src --name frontend_app -d --rm  -p 3000:3000 frontend:1.1
```
> the nodemon or auto reflect code change module won't be able to use in window. 

## Development vs Production/ Deployment.<br>
![image](https://user-images.githubusercontent.com/34083808/184497138-1828ce18-071d-4d46-9ea3-caa2c7811dba.png)

## Room for Improvement.<br>
![image](https://user-images.githubusercontent.com/34083808/184497160-92b34fab-34ae-43f1-b636-651898714022.png)


# What is Docker compose ?
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Compose works in all environments: production, staging, development, testing, as well as CI workflows.

# Why we need Docker compose ?
Consider this example:<br>
```
docker network create shop
docker build -t shop-node .
docker run -v logs:/app/logs --network shop --name shope-web shop-node
docker build -t shop-database
docker run -v data:/data/db --network shop --name shop-db shop-database
```
This is a very simple (made-up) example - yet you got quite a lot of commands to execute and memorize to bring up all Containers required by this application.And you have to run (most of) these commands whenever you change something in your code or you need to bring up your Containers again for some other reason. With Docker Compose, this gets much easier You can put your Container configuration into a ```docker-compose.yaml``` file and then use just one command to bring up the entire environment: ```docker-compose up``` <br>
![image](https://user-images.githubusercontent.com/34083808/184497780-dd09a92f-edf5-448e-8bde-c1c50e9e0dfa.png)

# What Docker Compose is NOT.<br>

![image](https://user-images.githubusercontent.com/34083808/184497906-375a7019-ba94-4f67-b3de-84eb7602a7e5.png)

# Writing Docker Compose Files. <br>
you can do almost anything with docker compose same as docker command in the terminal.<br>
![image](https://user-images.githubusercontent.com/34083808/184497984-6798db28-f431-4416-b942-327b0d187b97.png)

> we use YAML file for config the docker compose. YAML is a data serialization language that is often used for writing configuration files. for more information of YAML file please check out this [link](https://www.redhat.com/en/topics/automation/what-is-yaml)

> Docker compose file reference [link](https://docs.docker.com/compose/compose-file/)

## Installing Docker Compose on Linux. <br>
On macOS and Windows, you should already have Docker Compose installed - it's set up together with Docker there. On Linux machines, you need to install it separately.
```linux showLineNumbers
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

docker-compose --version

```
## Docker Compose Files.<br>
A ```docker-compose.yaml``` file looks like this:
```yaml
version: "3.8" # version of the Docker Compose spec which is being used
services: # "Services" are in the end the Containers that your app needs
  web:
  build: # Define the path to your Dockerfile for the image of this container
  context: .
  dockerfile: Dockerfile-web
  volumes: # Define any required volumes / bind mounts
    - logs:/app/logs
  db:
    build:
      context: ./db
      dockerfile: Dockerfile-web
    volumes:
      - data:/data/db
```
> below is the docker-compose config file for mutil container example of previouse section. <br>
``` yaml
version: "3.8"
services:
  mongodb:
    image: "mongo"
    volumes:
      - data:/data/db
    #container_name: mongodb
    #environment:
    #- MONGO_INITDB_ROOT_USERNAME=yushin
    #- MONGO_INITDB_ROOT_PASSWORD=admin
    # - MONGODB_USERNAME yushin
    env_file:
      - ./env/mongo.env
    # docker compose will automatically create the network for the container within the YAML file.
    # network:
    #  - goals-net
  backend:
    build: ./backend
    #build:
    #  context: ./backend
    #  dockerfile: Dockerfile
    #  args'
    #    some-arg: 1
    # the detach mode is enable by default, so no need to setup the detach mode.
    ports:
      - "80:80"
    volumes:
      - logs:/app/logs
      - ./backend:/app
      - /app/node_modules
    env_file:
      - ./env/backend.env
    depends_on:
      - mongodb
  frontend:
    build: ./frontend
    # port
    ports:
      - "3000:3000"
    volumes:
      - ./frontend/src:/app/src
    # to enable attach mode, need to add below setting to yaml file
    stdin_open: true
    tty: true
    depends_on:
      - backend
  #  image: "mongodb"
# the name volume shoud be declare here.
# name volume can be shared among container.
volumes:
  data:
  logs:
```
<br>
You can conveniently edit this file at any time and you just have a short, simple command which you can use to bring up your Containers:
you can run docker-compose by using below command.

```
docker-compose up

# to start with detach mode.
docker-compose up -d 

docker-compose down

# to remove all volume
docker-compose down -v
```
You can find the full (possibly intimidating - you'll only need a small set of the available options though) list of configurations here: [link](https://docs.docker.com/compose/compose-file/)

> **Important to keep in mind**: When using Docker Compose, you **automatically get a Network for all your Containers** - so you don't need to add your own Network unless you need multiple
Networks!
## :bell: Docker Compose Key Commands
There are two key commands:
* ``` docker-compose up :```: **Start all containers** / services mentioned in the Docker Compose file
  - ```-d```:  Start in **detached mode**
  - ```--build```: Force Docker Compose to re-evaluate / rebuild all images (otherwise, it onlydoes that if an image is missing)
* ```docker-compose down```: **Stop and remove** all containers / services
  - ```-v```: ** Remove all volumes** used for the containers - otherwise they stay around, even if the Container are removed.

Of course, there are **more commands**. You'll see more commands in other course sections (e.g. he "Utility Containers" and "Laravel Demo" sections) but you can of course also already dive into the official command [reference](https://docs.docker.com/compose/reference/).

# What are Utility Containers?
![image](https://user-images.githubusercontent.com/34083808/184524506-8926621d-780e-48ff-b68f-ec673221fa7c.png)

we can use docker as a utility, by using that we can easily using the software such as node/npm without install on our hostmachine. 

## Different ways of running command in Container.
for example, you can using node command by docker utility container. 
```
docker run -it node
```
to use docker utility we need to setup through docker file. the ``` ENTRYPOINT``` is similar with ```CMD```, but it allow to append running command after the entry points.
```
FROM node:14-alpine

WORKDIR /app

ENTRYPOINT [ "npm" ]
```
after building the image, we can run the docker container utility by running below command. 
bind mount help to move the file from the container to host machine.
```
docker build -t mynpm .
docker run -it -v "D:\Source\Git\Docker\docker-utility":/app mynpm init
```

> Docker compose can help to reduce the command and we don't need to remember long command.
docker compose file. 

```yaml
version: "3.8"
services:
  npm:
    build: ./
    stdin_open: true
    tty: true
    # volume
    volumes: #
      - ./:/app
```
this time we don't need to run ```docker-compose up/down```, instead we use ```docker-compose run --rm npm init``` 

# The target setup
this project need alot of setup step to accomplish, that the reason this project is good example of using docker to hander complicated setup. The below image show the structure of our Laravel setup, it need totaly 6 containers, 3 apps container and 3 utility containers. <br>
![image](https://user-images.githubusercontent.com/34083808/184526111-47ab8a52-face-4057-8e15-ac8206e62dda.png)

## docker compose file. 
The setup of docker compose file is simmilar with previous section. <br>

```yaml
version: '3.8'

services:
  server:
    # image: 'nginx:stable-alpine'
    build:
      context: .
      dockerfile: dockerfiles/nginx.dockerfile
    ports:
      - '8000:80'
    volumes:
      - ./src:/var/www/html
      - ./nginx/nginx.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      - php
      - mysql
  php:
    build:
      context: .
      dockerfile: dockerfiles/php.dockerfile
    volumes:
      - ./src:/var/www/html:delegated
  mysql:
    image: mysql:5.7
    env_file:
      - ./env/mysql.env
  composer:
    build:
      context: ./dockerfiles
      dockerfile: composer.dockerfile
    volumes:
      - ./src:/var/www/html
  artisan:
    build:
      context: .
      dockerfile: dockerfiles/php.dockerfile
    volumes:
      - ./src:/var/www/html
    entrypoint: ['php', '/var/www/html/artisan']
  npm:
    image: node:14
    working_dir: /var/www/html
    entrypoint: ['npm']
    volumes:
      - ./src:/var/www/html

```
## run composer utility container to generate the source code of our application.<br>
at first, we need to create our laravel source code by using composer utility container.
```
docker-compose run --rm composer create-project --prefer-dist laravel/laravel .
```

after that, we need to change the eviroment variable of the laravel source code in ```./src/.env``` to our database container.
```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

next step we will start out application by docker compose. 
```
docker-compose up --rm -d server php mysql
```
> if there is error related to permision of vendor folder, please delete this folder and run the composer update command ``` docker-compose run --rm composer update```

We can also just run only the server container by adding the dependency in docker compose file. 
```
    depends_on:
      - php
      - mysql
```
and then run the command ```docker-compose up --rm -d server```

## Run docker utility artisan to migrate the database. 
Please check the mysql password and username in .env file and also make sure to run ather 3 app container. 
```
docker-compose run --rm artisan migrate
```
> if there are any problems with migration you can you below command to refresh.
```
docker-compose run --rm artisan config:cache
docker-compose run --rm artisan config:clear
docker-compose run --rm artisan cache:clear
```

after finish all setup step, you shoud see the below page. <br>
![image](https://user-images.githubusercontent.com/34083808/184610364-b71ea362-00cb-4e6f-972b-6b97616e2b67.png)

# Why we need docker ?
![image](https://user-images.githubusercontent.com/34083808/184829729-f3909e4f-7bfb-4239-ae78-e99eb943b784.png)

![image](https://user-images.githubusercontent.com/34083808/184829571-bb47af75-b76b-413b-820b-5968d34129b1.png)

# Development to Production: Things To Watch Out For

![image](https://user-images.githubusercontent.com/34083808/184829936-42944eea-13e8-464c-bbbf-cc25d0cfb04c.png)

# Bind Mounts, Volumes & COPY
![image](https://user-images.githubusercontent.com/34083808/184832317-a0ee8754-c67f-4efa-b449-5c7338579c31.png)

# A Basic First example: Standalone Nodejs App.

![image](https://user-images.githubusercontent.com/34083808/184832399-847e0ca2-94f7-47ce-bec0-f9ef9950460a.png)

## Hosting providers.
![image](https://user-images.githubusercontent.com/34083808/184832635-5f377d2a-cfee-402d-bc02-0eff80637538.png)

## deploy to AWS EC2
![image](https://user-images.githubusercontent.com/34083808/184833854-98e977d1-2303-44fb-a620-c53efabd3673.png)

## create AWS EC2 instance. 
go to EC2 instance dashboard, click on Launch instances.<br>

![image](https://user-images.githubusercontent.com/34083808/184842955-7f71e4a3-b866-445a-b514-f3280842684c.png)

then select amazon linux version of instance. <br>
![image](https://user-images.githubusercontent.com/34083808/184843236-15d74caf-74c3-49e8-8dc8-bd10e21e922d.png)

select instance type t2.micro free tier eligible (for free). Please also select the key pair, if you don't have you can create new key pair. 
note that, this key pair only provide onece, if you lost it you have to delete your instance. we have two type of key pair one for openSSH (linux, mac os), one for putty (window). <br>

![image](https://user-images.githubusercontent.com/34083808/184843392-ca07e128-65c5-43e7-8981-17f763107f2b.png)

after completed the setting select launch instance and start connect to your virtual host. in my case I use putty. you can get the connect information in the instance connect session. <br>
![image](https://user-images.githubusercontent.com/34083808/184844387-740b2bed-58cd-464c-894e-b4f92de70bd8.png)

Putty setup for new ssh session.

![image](https://user-images.githubusercontent.com/34083808/184844801-dd9de423-07ff-44f5-abfb-b9197f4b946e.png)

![image](https://user-images.githubusercontent.com/34083808/184844950-2f48e754-9a3c-4c18-9cec-8441f064db5f.png)

after conected you'll see as below image. <br>
![image](https://user-images.githubusercontent.com/34083808/184845258-fdf7d491-beec-425a-8455-523c9c994d02.png)

please run update pakage command ``` sudo yam update -y``` for aws-linux or ```sudo apt update``` for ubuntu. 
then use can use the amazon-linux-extras to install docker, this is customize tool of amazon for easy intall program. 
``` sudo amazon-linux-extras install docker ```

to start docker service please use ``` sudo service docker start```

> incase of linux general os please folow the instructor on [docker](https://docs.docker.com/engine/install/)

## Deloy Source code vs image. <br>

![image](https://user-images.githubusercontent.com/34083808/184858580-94dbe3bb-7a5c-4f1a-a6f4-f43c309be754.png)

1. Create repository on docker hub.

![image](https://user-images.githubusercontent.com/34083808/184859482-9588c547-be04-4514-a846-1de3848de494.png)

2. Build immage on local machine and push it to Docker hub.

```
docker build -t  node-app-dep-1:1.0 .

# make a tag 
docker tag node-app-dep-1:1.0 yushinb/node-app-dep-1:1.0
# push to docker hub
# please login at first
docker login
docker push yushinb/node-app-dep-1:1.0
```

> To get access to the web page we need to set the Security groups inside EC2. 
you'll see currenly, outbound group are open for any port and any type of trafic from instance to outside, that the reason why you can download the image.

![image](https://user-images.githubusercontent.com/34083808/184916263-8bacc36d-2d01-4858-acc8-8ed7100bb7e3.png)

On the other hand, the inbound just allow some port can be access from outside the instance. 

![image](https://user-images.githubusercontent.com/34083808/184916782-029bc179-cc8b-41fd-81cb-5d2e5b07a64d.png)

# managing and updating the Container.

if you make any change in your source code then you need to build the image again. after that push image to dockerhub. 
```
docker build -t  yushinb/node-app-dep-1:1.0 

docker login
docker push yushinb/node-app-dep-1:1.0
```

update new image on virtual host machine. 
```
sudo docker pull yushinb/node-app-dep-1:1.0
sudo docker stop node-app
sudo docker run -d --rm --name node-app -p 80:80 yushinb/node-app-dep-1:1.0
```
