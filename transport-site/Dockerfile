# see https://hub.docker.com/_/elixir
FROM elixir:1.10.4-alpine

RUN apk add nodejs-npm curl yarn bash build-base wget libtool git

# Install app dependencies
RUN mix local.hex --force
RUN mix local.rebar --force

# NOTE: development at https://git.savannah.gnu.org/gitweb/?p=libiconv.git
# and publication at https://ftp.gnu.org/pub/gnu/libiconv
RUN wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.16.tar.gz
RUN tar -xf libiconv-1.16.tar.gz
RUN cd libiconv-1.16 && ./configure --prefix= && make && make install
RUN libtool --finish /lib
RUN rm libiconv-1.16.tar.gz
RUN rm -r libiconv-1.16
