# We leverage the base images published by hexpm.
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
FROM hexpm/elixir:1.12.2-erlang-24.0.3-ubuntu-focal-20210325

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Europe/Paris
RUN apt-get install -y tzdata

RUN apt-get update && apt-get install -y \
    curl \
    wget \
    libtool \
    git

# To be removed in favor of multistage build with deterministic version numbers,
# e.g. https://dev.to/quatermain/comment/njf7
RUN apt-get install -y nodejs npm
RUN npm install -g yarn

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force
