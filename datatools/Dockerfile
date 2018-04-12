FROM openjdk:10-jre-slim

RUN apt-get update \
    && apt-get --yes upgrade \
    && apt-get --yes install git maven \
    && git clone -b transport_prod https://github.com/etalab/transport-datatools-server/ /app \
    && cd /app && mvn -Dmaven.test.skip=true package \
    && mv /app/target/datatool-server.jar /app/ \
    && mkdir -p /data \
    && rm -r /app/target \
    && rm -r /root/.m2 \
    && apt-get --yes remove maven git \
    && apt-get --yes autoremove

COPY *.yml /app/configuration/

ENTRYPOINT [ "java", "-jar", "/app/datatool-server.jar", "/app/configuration/env.yml", "/app/configuration/server.yml"]
