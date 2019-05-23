FROM elixir:1.8-alpine

RUN apk add nodejs-npm curl yarn bash gnu-libiconv-dev

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force
