FROM elixir:1.8-alpine

RUN apk add nodejs-npm curl yarn bash
RUN apk add build-base

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force


RUN apk add wget
RUN wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.16.tar.gz
RUN tar -xf libiconv-1.16.tar.gz
RUN cd libiconv-1.16 && ./configure --prefix=/usr/local && make && make install
RUN rm libiconv-1.16.tar.gz
