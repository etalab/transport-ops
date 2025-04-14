# docker build debug -t test:debug --progress=plain
FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Paris

RUN apt-get clean \
    && apt-get update \
    && apt-get upgrade \
    && apt-get update --fix-missing \
    && apt-get install -y curl

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    libtool \
    git \
    tzdata \
    zip \
    default-jre \
    maven
