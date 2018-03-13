FROM openjdk:10-jre-slim

RUN apt-get update \
    && apt-get --yes upgrade \
    && apt-get --yes install git maven \
    && git clone -b fix_graphql_spec https://github.com/l-vincent-l/datatools-server.git /app \
    && cd /app && mvn -Dmaven.test.skip=true package \
    && mv /app/target/dt-*.jar /app/dt.jar \
    && rm -r /app/target \
    && rm -r /root/.m2 \
    && apt-get --yes remove maven git \
    && apt-get --yes autoremove

COPY *.yml /app/configuration/

ENTRYPOINT [ "java", "-jar", "/app/dt.jar", "/app/configuration/env.yml", "/app/configuration/server.yml"]
