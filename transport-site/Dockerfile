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
FROM hexpm/elixir:1.12.2-erlang-24.0.3-focal-20210325

# TODO: iconv could probably be removed later 
# https://github.com/etalab/transport-site/issues/1591
RUN apk add curl bash build-base wget libtool git gnu-libiconv

# To be removed in favor of multistage build with deterministic version numbers,
# e.g. https://dev.to/quatermain/comment/njf7
RUN apk add nodejs-npm yarn

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force
