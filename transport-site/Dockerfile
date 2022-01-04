# We are interested in the binaries compiled on that container:
FROM ghcr.io/etalab/transport-tools:master as transport-tools

# We leverage the base images published by hexpm at:
#
# https://hub.docker.com/r/hexpm/elixir
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
FROM hexpm/elixir:1.13.1-erlang-24.2-ubuntu-focal-20210325

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Paris

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    libtool \
    git \
    tzdata

ENV NVM_VERSION v0.39.1
ENV NODE_VERSION 14.16.1
ENV NVM_DIR $HOME/.nvm

# Install NVM
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/${NVM_VERSION}/install.sh | bash

# Install NODE ("." is the sh equivalent to the bash "source" command)
RUN . $NVM_DIR/nvm.sh && nvm install $NODE_VERSION && nvm use --delete-prefix $NODE_VERSION

# add to path
ENV NODE_PATH $NVM_DIR/v$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

RUN npm install -g yarn

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force

RUN mkdir /transport-tools

COPY --from=transport-tools /usr/local/bin/gtfs-geojson ./transport-tools
RUN chmod +x /transport-tools/gtfs-geojson
