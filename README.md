# R in Production with Docker

> Based on the `rocker/r3.5.0` image.

This repo contains all files required to build a production-ready docker image for implemeting an R API using the `plumber` package.

## Get the rocker image to run an instance of R

```
docker pull rocker/r-ver:3.5.0
```

This will install the rocker image from the internet allowing you to use R version 3.5.0 on a linux continer.

## Run the container

This should open an R console and work as intended.

## Build the image using these files

```
docker build -t docker-demo .
```

Don't forget the trailing `.`
This should build the new image of the container (and name it docker-demo) that can run the R API built using the `plumber` package, using the version of R from `rocker/r-ver:3.5.0` docker image.

## Run an instance of this container

```
docker run --rm -p 80:80 plumber-demo
```

This will run an instance of this container and maps port **80** that `plumber` listens to, to port **80** on your computer.
This should allow the API to work just like normal. Open **http://127.0.0.1/predict_petal_length?petal_width=1** in your browser to interact with the now **production-ready** API.
