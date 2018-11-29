FROM elixir:1.6-alpine

RUN apk add nodejs-npm curl yarn

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force
