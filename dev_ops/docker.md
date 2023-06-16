# **Docker**

> -   [1. Instaling Docker on Linux](#installing-docker-on-linux)
> -   [2. Instaling Docker on Windows](#installing-docker-on-windows)
> -   [3. Docker Image](#docker-image)
> -   [4. Docker Container](#container)
> -   [5. Dockerfile](#dockerfile)

<br/>

## **Installing docker on Linux:** <br/>

1. Update the apt package index and install packages to allow apt to use a repository over HTTPS:

```yaml
$ sudo apt-get update
$ sudo apt-get install ca-certificates curl gnupg
```

2. Add Docker’s official GPG key:

```yaml
$ sudo install -m 0755 -d /etc/apt/keyrings
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/$ docker.gpg
$ sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

3. Use the following command to set up the repository:

```yaml
$ echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

4. Update the apt package index:

```yaml
$ sudo apt-get update
```

5. To install the latest version, run:

```yaml
$ sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

6. Verify that the Docker Engine installation is successful by running the hello-world image:

```yaml
$ sudo docker run hello-world
```

<br/>

## **Installing docker on Windows:** <br/>

## **Docker Image**

Docker Images are read-only. Once created you can not modify them, but you have to build new image.<br/>
Imge provides following resources for our app:

> -   runtime enviroment
> -   application code
> -   dependencies
> -   extra configuration
> -   commands

Images are made up from several layers. Order of layers does matter. <br/>
**Parent Images** are something like base layer that we can build our application on. We can find parent images on [Docker Hub](https://hub.docker.com/) <br/>
Layers:

> -   run commands
> -   dependencies
> -   source code
> -   parent image

_example docker image for node.js application:_

```yaml
FROM node:lts
WORKDIR /server
COPY package*.json
RUN npm install
COPY . .
EXPOSE 3000
# Define the command to run the application
CMD ["node", "index.js"]
```

<br/>

## **Container**

Isolated running instance of docker image.

<br/>

## **Dockerfile**

## **Dockerignore**

## **Working with containers**

## **Layer caching**

## **managing Images & Containers**

## **Volumes**

## **Docker Compose**

## **Dockerizing React App**

## **Sharing Images on Docker Hub**
