---
title: Why use Docker for databases and how
description: Docker is an invaluable piece to add to any tech stack for a multitude of reasons, but it can often be confusing to work with. Here we'll dive into my favourite way to use Docker containers and explain every step   .
draft: false
date: "2023-02-05"
categories:
- Development
tags:
- web dev
- docker
- learning
ogimage:
- /images/Moby-Group-Docker.jpeg
- 2560
- 1724
---

![Moby the Docker whale and all his friends](/images/Moby-Group-Docker.jpeg)

Sections
## What is Docker 
Docker is a tool used to create what are known as containers, yes you can imagine it like a shipping container on a cargo ship (hence the company's logo/mascot). The containers themselves are like small virtual machines that run a specific package of software that is defined by something called an image. The image tells Docker exactly what the small virtual machine needs to be and do and Docker creates a software only version of exactly that which runs _on_ your host machine **but** still _completely_ separate from your machine. You could consider it a separate node on a network that only the host machine _can_ have access to. I say _can_ have access because if you want to be able to communicate with the container, you will need to have a network set up, but we will look at this later.
## What database can be used?
For the sake of this article, we will use MariaDB as our database example, but you could apply most of the theory we'll cover to any database of your choosing. I am a back-end-leaning-full-stack kind of developer, so my personal favourite is relational SQL over NoSQL, and although MySQL is the industry standard, I personally prefer to use MariaDB whenever possible because it's open source and I'll choose open source over corporations any day.

Whatever your choice of database, there will be an image for you to pull and work from where the set up steps will;
1. be very similar to what we will go over here
1. any differences will be apparent when you read the necessary docs, because we all RTFM, right?...
## How does a MariaDB container work?
Like we saw before, the MariaDB container will just be like a stand-alone computer on the network that runs the version of MariaDB that you specify when you provide Docker with the image that you desire. If you [take a look at the official documentation for tags](https://hub.docker.com/_/mariadb/tags), you will see pages and pages of differnet versions that are available to suit any need imaginable.

The official docs also give nice and clear instructions on setting up a basic instance, but I remember that the very first time I read them and followed them, I did not exactly know what I was doing or why it worked. I am to make all that clear below!
## How to set it up from the terminal
Firstly, we are going to skip over the installation process for docker, which [you can find help with here](https://docs.docker.com/get-docker/) if you have not already done that.

Now we will look at the basic command provided by the documentation and break down what each part is doing.
```sh
docker run --detach --name mariadb-container --env MARIADB_USER=mizouzie --env MARIADB_PASSWORD=mizouzie_loves_mariadb --env MARIADB_ROOT_PASSWORD=root mariadb:latest
```
| command        | explanation |
|----------------|-------------|
| docker         |tells docker that we are talking to it, like shouting 'Hey!'|
| run            |we want what follows to be run BY docker|
| --detach       |flag to say 'please carry this out in the background and don't occupy the terminal' (the short version is `-d`)|
| --name         |the string following this flag will be assigned as the container name|
| --env          |the string following should be added as an environment variable, handy for passwords etc|
| mariadb:latest |the name and version of the image you wish docker to pull|

### Some things to note
After running his command with all you own details, docker will first check your local images for the one you have specified. If it doesn't find it already downloaded, it will then `pull` the image from [docker hub](https://hub.docker.com/) and use that to build the container with any and all of your given arguments like the environment variables and name.

The container will now be running and will not stop until you tell it to, which can be done by:
```sh
docker stop <the-name-you-gave-the-container>
```

The container will be accessible by a default network known as a bridge. You can read details of the network and other useful information about the container by running:
```sh
docker inspect <the-name-you-gave-the-container>
```
The huge display of information after running this command can seem daunting, but just make the terminal full screen and go through the layers one by one and you will soon start to understand the way these containers work. You will spot the environment variables you passed to it earlier, as well as all imaginable configuration key:value pairs which will mostly be set to whatever their default is, but all that is customizable if you wish to delve into the documentation.

If you just want to know what port you can access the container on you can use `grep`, for example:
```sh
docker inspect <the-name-you-gave-the-container> | grep HostPort
```
### Other services that work well with it
It's all good and well having your database instance running, but while we are developing applications we often need to be able to peer inside or even feed in some raw SQL commands.

The official image docs show how to connect a MySQL command line client, so again we'll break down what means what.
```sh
docker run -it --network some-network --rm mariadb mariadb -hmariadb-container -umizouzie -p
```
| command        | explanation |
|----------------|-------------|
| docker         |tells docker that we are talking to it, like shouting 'Hey!'|
| run            |we want what follows to be run BY docker|
| -it       |tells docker to keep an interactive terminal open to allow us to actually use the connection|
| --network         |the name of the network the desired container is on, therefore we wish to join (don't worry, we will talk about these below)|
| --rm          |remove this container once it is closed|
| mariadb mariadb |the first is the image we're using, the second is a pseudonym for the running container we'll connect to|
| -h |the name of the host container **must match what you put earlier**|
| -u |the name of the user we want to connect as, equal to the MARIADB_USER environment variable **must match what you put earlier**|
| -p |upon creating the container, prompt me for the password which will be equal to the MARIADB_PASSWORD environment variable|

This will open up MySQL client right there in the terminal and you can interact with your database.

However... I find this a little cumbersome for smooth development, so I much prefer to use Adminer, which as you may have guessed by now, can be spun up in it's very own container. The setup is similar to how we set up the MariaDB container and you can [check out the official docs](https://hub.docker.com/_/adminer) for the details should you want to set it up in the terminal too. The only problem with doing this is that you must set up a named network and connect both the MariaDB container and the Adminer container to it so that they can communicate. I did it a few times just to see how it was, and it's a lot of work so I'll just briefly explain the part of creating a network, because you will see later that there is a much easier way to achieve the same results even if doing it all manually is far more educational.
### Networking containers
Keeping things separate is kind of the essence of docker containers, but they're not much use if they can't communicate with one another. This is nice and straightforward to acieve by networking.

Upon creating the MariaDB container above, it automatically made a bridge network with the host machine to expose the ports found by searching through the output of `docker inspect`, which is typically 3306. It gets a little out of that scope when we want to connect another service to both the MariaDB container and our host machine. This is where we must create a network which will connect all three.

Networking in docker treats the network itself as a container that you can add other containers to. The only difference is that it is noticably more simple to set up. Just the following command, and we're good to go:
```sh
docker network create <name-of-your-network>
```
Now that the network exists, it gives us the option to connect a container to it during the `docker run` command by using the `--network` flag and naming this newly created network, or it is possible to add a running container by using:
```sh
docker network connect <name-of-your-network> <name-of-your-container>
```
Once both the database is added to the network, you can add whatever method of interacting with it you choose.
## How to set it all up with docker compose
There is a much more simple way to do all of what we have discussed in one go. Creating a file named `docker-compose.yaml` inside your working directory. This is like an instruction script for automating all the terminal commands we just painstakingly typed in. Now that we know the ins and outs of all the commands, we should be able to read through the file and know what is doing what. Here is an example:
```yaml
version: "3"

services:

  mariadb:
    image: mariadb:10.7
    environment:
      - MARIADB_ROOT_PASSWORD=root
      - MARIADB_DATABASE=developer
      - MARIADB_USER=developer
      - MARIADB_PASSWORD=developer
      - MARIADB_AUTO_UPGRADE=true
    volumes:
      - mariadb:/var/lib/mysql
    ports:
      - "3306:3306"

  adminer:
    image: adminer:latest
    environment:
      - ADMINER_DEFAULT_SERVER=mariadb
    ports:
      - "8080:8080"

volumes:
  mysql:
```
### Stoping and starting containers

## How I keep it organised
### Boilerplate docker-compose.yaml
### Memory management

## Benefits over local installation of MariaDB

