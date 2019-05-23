FROM elixir:1.8-alpine

RUN apk add nodejs-npm curl yarn bash
RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force
