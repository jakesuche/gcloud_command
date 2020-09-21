# This is demo of docker and google container engine

## commands

### 1  First we build the container and tag it with * _-t_*  flag ( docker build a container with tag t as e.g web-server with a version) then the dot ( . ) will look into  the directory to check for the docker file..

_The docker file is what is responsible for  telling docker how our image will be build our image_

        ~ sudo docker build -t web-server:v1 .
        

### 2  cat Dockerfile

_This is base image we are going to use to build our app._
_if we  dont have it locally, it goes our to the trusted Docker registry and downloads it_

        ~  FROM ubuntu:latest

_Adds a new layer that will upadate apt_


        ~ RUN apt-get update -y

_Adds a new layer that will add in all the python libs_

        ~ RUN apt-get install -y python-pip python-dev build-essential

_WE want to bring our code into the image_

        ~ COPY ./app/app

_The WORKDIR intruction sets the working directory for any RUN,CMD, ENTRYPOINT,COPY and ADD instructions that follows_

        ~ WORKDIR /app

_install any python dependencies._

        ~ RUN pip install -r requirements.txt

_change the entry point from /bin/sh to python_

        ~ ENTRYPOINT ["python"]

_the python script to run_
_this really should be run behind in a WSGI conatiner of some  sort._

        ~ CMD ["app.py"]

### 3  To run a docker container 

_this will run it in the background with demonize tag (-p) which means whatever is running on the container port (8888) get  exposed to port 8888 on the container, telling it what container image we want to run specifically what version_ 

        ~ sudo docker run -d -p 8888:8888 --name py-web-server -h my-web-server py-web-server:v1

_to see to number of images with their version_

        ~ docker images

_to see the container process running_

        ~docker ps

_to kill a container

        ~docker kill [CONTAINER ID]

_to stop any container running_


        ~ docker stop $(docker ps -a -q)


### 4 To have multiple conatiner 



    