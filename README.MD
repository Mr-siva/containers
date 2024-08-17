# 01
# ---------------------------------------------------
> **Written by MrNobody**

#### Docker play ground link

_[link to get the labs](https://labs.play-with-docker.com/)_


######  To build docker in same `folder`
```bash
   docker build .
```

###### To run docker `image` 
```bash
   docker run -p 3000:3000 d4149979e13f
```

###### To check app working or not 
```bash
   localhost:3000
```

###### docker ps to `list running containers` runnig
```bash
   docker ps
```

###### docker ps to `list all containers` runnig
```bash
   docker ps -a
```

###### To stop docker
```bash
   docker stop <CONTAINER_NAME>
   docker stop fav_app
```

###### To delete docker
```bash
   docker rm <CONTAINER_NAME>
   docker rm fav_app
```

<br />
<br />



# 02 
# --------------------------------------------------
######  Downloading form docker hub
```bash
   docker run node
```

###### Expoing docker to local machine

```bash
   docker run -it node  # Welcome to Node.js v22.2.0.
```


Run node.js code locally
: - To start server in local-host (localhost:80)
: - item 2
```bash
   npm install
   node server.js
```



---
### Attached and Detached

###### Detached mode
```bash
docker start <cont_id>
docker start -a -i <cont_name>
docker run -p 8000:80 -d <id>
```

###### Attached mode
```bash
docker run -p 8000:80 <id>
docker attach <cont_name>
```
---

<br />

---
### Logs

###### To get live logs
```bash
   docker loga -f <id>
```

---

<br />

---

## Deletions and Removed
```bash
docker ps
docker images
docker stop
docker rm <ID>-> to remove container
docker rmi <ID> -> to remove images
docker image prune -> to remove un-used iamges

docker run -p 3000:80 -d --rm <iamge_id>  -> when ever container get stoped, image will deleted automatacally
```

---

<br />

---
### inspect
```bash
docker image inspect 507b3f6e7f75
```
---
<br />

---

## Coping files into & form a `containter`
```sh
docker cp dummy/. sweet_antonelli:/test -> local to container
docker cp sweet_antonelli:/test dummy   -> containre to local
```
---
<br />

---

## Naming and Tagging `containters` & `images`
- `iamges` -t 
- `containers` --name

example
```sh
docker build -t goal:latest .
docker run -p 3000:80 -d --rm --name <CONTAINER_NAME> <IAMGE_NAME>:<IMG_TAG>
docker run -p 3000:80 -d --rm --name goalapps goal:latest 
```
---
<br />

---
> ### working on assigenment

```sh
docker build .
docker images -> get id "03bf274b202d"
docker run -p 8000:3000 -d --rm 03bf274b202d
```

---
<br />

---
> ### Sharing images and containers
- pushing images to dockerhub
- my repo name : - *sivagane/node-hello-my-app
```sh
docker push IMAGE_NAME
docker pull IMAGE_NAME
```
==need to create image as per docker repo name==
```
docker tag <old_name> <new_name>
```
- Login to Docker & build and push image
```sh
   docker build -t sivagane/node-hello-my-app .
   docker push sivagane/node-hello-my-app
```
---
<br />

---

# DOCKER VOLUMES

|Sno|Voluems Type |Useage |
|---|---          |---    |
|1  | Anonymous   |        |
|2  | Named       |        |
|3  | Bind        |        |


1, ***Volumes (managed by Docker) -v*** 

: - Anonymous Volumes
: - Named Voules 
```sh
docker run -d -p 3000:80 --rm --name feedback-app -v <voulme_name>:<path_of_name> <IMAGE_NAME>
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback <IMAGE_NAME>
```
     
2, ***Bind mounts9 (Managed by me)***
: - Bind Volues
```sh
docker run -d -p 3000:80 --name feedback-app -v <voulme_name>:<PATH_INSIDE_CONTAINER> -v <OUR_CODE_FOLDER_PATH>:/<CONTAINER_PATH> -v <CONTINER_PATH> <IMAGE_NAME>
docker run -d -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "/Users/sivaganeshreddy/learnings/docker/docker-udemy/data-volumes-03-adj-node-code:/app" -v /app/node_modules vol-img
```
- ***To list & remove docker `Volumes`***
```sh
docker volume ls
docker volume rm <VOLUME_NAME>
```
- ***To make `Volume Read Only` :ro***
```sh
docker run -d -p 3000:80 --name feedback-app -v feedback:/app/feedback -v "/Users/sivaganeshreddy/learnings/docker/docker-udemy/data-volumes-03-adj-node-code:/app:ro" -v /app/node_modules vol-img
```

## `Managing Volumes`
```sh
docker volume ls
docker volume create feedback-files
docker volume inspect <VOLUME_NAME>
docker volume rm <VOLUME_NAME>
```

---
<br />

---



## Docker Environments vars and Arguments
- Env Vars
```sh
docker run -d -p 3000:8000 --env PORT=8000 --name fbk-app img
docker run -d -p 3000:8000 --env-file ./.env --name fbk-app img
```
- Arguments
```sh
docker build -t feedback-node:dev --build-arg DEFAULT_PORT=8000
```
---
<br />

---

# ***`Multi Container`***

- app container
```sh
docker build -t fav_image .                          
docker run --name fan_app2 -d --rm -p 3000:3000 ffav_image
```
- DB container
```sh
docker run -d --name mongodb mongo
docker inspect mongodb
```
---
<br />

---


# `Network`
- ***Create a network 1st***
```sh
docker network create <NETWORK_NAME>
docker network create fav_app_network
```
-  ***DB container***
```sh
docker run -d --name mongodb --network fav_app_network mongo 
docker inspect mongodb
```
-  ***App container***
```sh
docker build -t fav_image .                          
docker run --name fav_app --network fav_app_network -d --rm -p 3000:3000 fav_image
```


---
<br />

---


# `Multi Container App`
- ***Create Mongo DB 1st with mongo images***
```sh
docker run --name mongodb --rm --network multi_network -d mongo
```

- ***Data Persist and Authentaction***
> data serversing
```sh
docker run --name mongodb --rm -v data:/data/db --network multi_network -d mongo
```

> Authenction

[Doc Link](https://hub.docker.com/_/mongo)
[mondodb link](https://www.mongodb.com/docs/manual/reference/connection-string/#self-hosted-deployment-connection-string-examples)

```
-e MONGO_INITDB_ROOT_USERNAME and MONGO_INITDB_ROOT_PASSWORD.
```
```sh
docker run --name mongodb --rm -d -v data:/data/db --network multi_network -e MONGO_INITDB_ROOT_USERNAME=max -e MONGO_INITDB_ROOT_PASSWORD=secrect mongo
docker run --name mongodb -e MONGO_INITDB_ROOT_USERNAME=max -e MONGO_INITDB_ROOT_PASSWORD=secret -v data:/data/db --rm -d --network goals-net mongo
```
```
  'mongodb://siva:password@mongodb:27017/course-goals?authSource=admin',
```

- ***To refer monfo db in backend code***
```sh
'mongodb://host.docker.internla:27017/course-goals',
```

-  ***Node App Dokerizing(Back-end)***
```sh
docker build -t goals-code .
docker run --name goals-backend --rm --network multi_network -d -p 80:80 goals-code
docker run --name goals-backend -e MONGODB_USERNAME=max -e MONGODB_PASSWORD=secret -v logs:/app/logs -v /Users/sivaganeshreddy/learnings/docker/docker-udemy/multi-01-starting-setup/backend:/app -v /app/node_modules --rm -d --network goals-net -p 80:80 goals-node
```
-  ***React App Dokerizing(Front-end)***
```sh
docker build -t goals-react .
docker run --name goals-frontend --rm --network multi_network -d -p 3000:3000 goals-react

  docker run --name goals-frontend -v /Users/sivaganeshreddy/learnings/docker/docker-udemy/multi-01-starting-setup/frontend/src:/app/src --rm -d -p 3000:3000 -it goals-react
```


---
<br />

---

# `Docker Compose`
[Doc Link](https://docker-docs.uclv.cu/compose/compose-file/)

- ***Create Mongo DB 1st with mongo images***
```sh
doczker compose up
docker compose up -d
docker compose down 
docker compose down -v
```















################# mango-db #################

opt/homebrew/Cellar/mongosh
brew services restart jenkins-lts
brew services stop mongodb-community@7.0
4CYP-QBVJ








mrnobody009















###### vulnerabulitues 
docker scout quickview <IAMGE_NAME>
docker scout quickview academind/node-hello-world





![image link](link of image uploaded in gothub)














# RAW
## Creates 2 Lines that CAN be selected as text
## -------------------------------------------------
### The non-breaking space ASCII character
&nbsp;
&nbsp;

### HTML <(br)/> tag
<br />
<br />

## Creates 2 Lines that CANNOT be selected as text
## -------------------------------------------------
### HTML Entity &NewLine;
&NewLine;
&NewLine;

### Backticks with a space inside followed by two spaces
`(space)`(space)(space)
`(space)`(space)(space)
#### sample:
` `  
` `

# End

indentation 
: - To start server in local-host (localhost:80)
: - item 2