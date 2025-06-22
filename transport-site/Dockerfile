# We are interested in the binaries compiled on that container:
FROM ghcr.io/etalab/transport-tools:v2.0.0 AS transport-tools

# We leverage the base images published by hexpm at:
#
# https://hub.docker.com/r/hexpm/elixir
#
# Which are built via:
#
# https://github.com/hexpm/bob#docker-images
#
# They provide the ability to decouple Elixir version
# and OTP version, which is a nice feature for
# incremental/decoupled upgrades.
#
# These base images package Elixir and OTP releases found at:
# - https://github.com/elixir-lang/elixir/releases
# - https://github.com/erlang/otp/releases
#
#
# So again, to upgrade this, check out : 
#
# https://hub.docker.com/r/hexpm/elixir
FROM hexpm/elixir:1.18.4-erlang-27.3.4.1-ubuntu-noble-20250529

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Paris

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    libtool \
    git \
    tzdata \
    zip \
    default-jre \
    maven

# AppSignal dependencies https://docs.appsignal.com/support/operating-systems.html#debian-ubuntu
# without them the appsignal Elixir package installation will log an error but not stop the build!
RUN apt-get update && apt-get install -y build-essential ca-certificates

# Helps bump the output of /etc/os-release from says "Ubuntu 24.04.2 LTS" to "... 24.04.3"
#
# The source image (hex) is itself based on a ubuntu image whose
# packages are not necessarily up-to-date all the time. We want to
# upgrade and ensure we are as up-to-date as possible.
# Note that the kernel itself cannot be upgraded here apparently
# (https://stackoverflow.com/a/66413248)
#
# See https://github.com/etalab/transport_deploy/issues/46 for more context
RUN apt-get upgrade -y

# debugging information
RUN uname --all
RUN cat /etc/os-release
RUN cat /etc/lsb-release

# Helps bump the output of /etc/os-release from says "Ubuntu 24.04.2 LTS" to "... 24.04.3"
#
# The source image (hex) is itself based on a ubuntu image whose
# packages are not necessarily up-to-date all the time. We want to
# upgrade and ensure we are as up-to-date as possible.
# Note that the kernel itself cannot be upgraded here apparently
# (https://stackoverflow.com/a/66413248)
# 
# See https://github.com/etalab/transport_deploy/issues/46 for more context
RUN apt-get upgrade -y

# debugging information
RUN uname --all
RUN cat /etc/os-release
RUN cat /etc/lsb-release

ENV NVM_VERSION=v0.40.1
ENV NODE_VERSION=20.17.0
ENV NVM_DIR=$HOME/.nvm

RUN mkdir $NVM_DIR

# Install NVM
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/${NVM_VERSION}/install.sh | bash

# Install NODE ("." is the sh equivalent to the bash "source" command)
RUN . $NVM_DIR/nvm.sh && nvm install $NODE_VERSION && nvm use --delete-prefix $NODE_VERSION

# add to path
ENV NODE_PATH=$NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH=$NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN npm install -g yarn

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force

RUN mkdir /transport-tools

# GTFS to GeoJSON 
COPY --from=transport-tools /usr/local/bin/gtfs-geojson ./transport-tools
RUN chmod +x /transport-tools/gtfs-geojson
# quick check to verify binary dependencies are matched
RUN /transport-tools/gtfs-geojson --help

# GTFS Realtime Validator
COPY --from=transport-tools /usr/local/bin/gtfs-realtime-validator-lib-1.0.0-SNAPSHOT.jar ./transport-tools
RUN java -jar /transport-tools/gtfs-realtime-validator-lib-1.0.0-SNAPSHOT.jar 2>&1 | grep "For batch mode you must provide a path and file name to GTFS data"

# GTFS Canonical Validator
COPY --from=transport-tools /usr/local/bin/gtfs-validator-7.1.0-cli.jar ./transport-tools
RUN java -jar /transport-tools/gtfs-validator-7.1.0-cli.jar --help | grep "Location of the input GTFS ZIP"
