# see https://hub.docker.com/r/hexpm/elixir
FROM hexpm/elixir:1.12.1-erlang-24.0.3-alpine-3.13.3

# TODO: iconv could probably be removed later 
# https://github.com/etalab/transport-site/issues/1591
RUN apk add curl bash build-base wget libtool git gnu-libiconv

# To be removed in favor of multistage build with deterministic version numbers,
# e.g. https://dev.to/quatermain/comment/njf7
RUN apk add nodejs-npm yarn

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force
