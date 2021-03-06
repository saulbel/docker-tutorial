# Docker Tutorial

## Prerequisites
Things you need before starting:
* `Debian`
* `Docker`
* `Docker-compose`

## Project structure
```
docker-tutorial
|── Dockerfile
|── docker-compose.yml
|── docker-compose_vm.yml
|── setup_vm.sh
|── README.md
|── .custom
|    └── index.html
|    └── nginx.conf
|    └── setup.sh
└── .github/workflows/
     └── docker-publish.yml
```
## Tasks to accomplish
- Create docker container that run `nginx` with a custom htlm --> index.htlm
- This index.html should be mounted from specific folder in the repo --> ./custom/index.html
- This docker container should be deployed using `docker-compose` --> docker-compose.yml
- This docker container needs to listen (internally) as port 8080 and in host needs to listen at port 80.
- This docker container should contain the following tools `netstat`, `curl` and `vim` --> ./custom/setup.sh
- Build CI pipeline that pushes the docker image into a container registry.

## How to install Docker and Docker-compose
I have included a custom script that allows you to install both `docker` and `docker-compose` on `Debian`.
I recommend to download it and change it with your `username` because I have decided to add my `user` to `docker group`. It is a good practice to run docker with a user instead of as `root`.

## How to setup this project locally
- First we should download it with `git clone` or as `.zip`
- We get into our project folder and now we are gonna build a custom `docker-image` and instead of using `docker build .` we are gonna use `docker-compose build`. 
- Now we just have to `docker-compose up -d` and we will have our container up and running.
````
$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS          PORTS                                   NAMES
dc00238beaf1   dockertutorial_nginx-test   "/docker-entrypoint.…"   28 minutes ago   Up 28 minutes   0.0.0.0:8080->80/tcp, :::8080->80/tcp   dockertutorial_nginx-test_1
````
- Now we just have to test it with `curl`.
````
$ curl localhost:80     --> host
$ curl localhost:8080   --> inside container
````
## What about docker volumes
Let´s say we already have our `debian-server` and we want to install `nginx` as docker container. Then we will use `docker-volumes` in our `docker-compose` --> docker.compose_vm.sh. We just need to point the directory where our `nginx.config` is and put it in `/etc/nginx/conf.d`

## GitHub Actions
I have built a CI pipeline that builds a custom `nginx` docker image with `Dockerfile` and pushes it into GitHub container registry  `ghcr.io`. If I want to pull new `docker image` I will just have to:
````
$ docker pull ghcr.io/saulbel/docker-tutorial:main
$ docker images
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
ghcr.io/saulbel/docker-tutorial   main      e75801090259   48 minutes ago   209MB
````

## To improve
-  As we can see, our `docker image` is quite big because I have decided to use `nginx` as image instead of `nginx:alpine`, which is much lighter. 
-  Spin up more containers with `docker-compose` and play around with `nginx-config`.
