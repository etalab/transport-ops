# see https://hub.docker.com/r/hexpm/elixir
FROM hexpm/elixir:1.10.4-erlang-23.2.7.2-alpine-3.13.3

RUN apk add curl bash build-base wget libtool git

# To be removed in favor of multistage build with deterministic version numbers,
# e.g. https://dev.to/quatermain/comment/njf7
RUN apk add nodejs-npm yarn

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
