
FROM ubuntu:trusty

MAINTAINER Mohamed Boussaa "mohamed.boussaa@inria.fr"
RUN echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf > /dev/null
RUN apt-get update && apt-get install -y git


RUN git --version
