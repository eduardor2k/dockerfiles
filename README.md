# My Dockerfiles
My personal collection of docker files

## Apache + PHP
Image that runs under ubuntu 16.04 with Apache 2.4 and php 7.0.

> docker build -t apachephp -f ./Dockerfile .
> docker run -d -p 80:80 -p 443:443 apachephp

## Apache + PHP + Mongo
Coming soon
