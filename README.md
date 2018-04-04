# transport-ops
Scripts and config files for provision and deploy transport and required services

## Production configuration and deployment

The production requires the following services:

* transport-site: the web frontend

  * Hosted at https://transport.data.gouv.fr
  * Elixir application hosted on Heroku
  * The _master_ branch of https://github.com/etalab/transport-site is deployed by CircleCI

* datatools-server: the GTFS validation service

  * Hosted at https://catalogue.transport.data.gouv.fr
  * Java application hosted on Clever-cloud
  * We maintain a branch *transport_prod* with our specific deployment needs on https://github.com/etalab/transport-datatools-server/
  * That branch is automatically deployed by Clever-Cloud

* udata: references and/or stores the datasets and comments

  * Hosted at https://data.gouv.fr

## Docker for transport-site CI

The _Transport Site_ project is tested with CircleCI in a Docker container that is defined in `transport-site/Dockerfile`.

When updating that Dockerfile, push a git tag in the format `a.b.c` and the image will be built by Dockerhub : https://hub.docker.com/r/betagouv/transport/

Then update https://github.com/etalab/transport-site/blob/master/.circleci/config.yml to match the new version.

## Use datatools-server for local developpement

If you are using _Docker Machine_ (probably if you are running on macOSX or Windows), run

```
docker-machine create datatools
docker-machine start datatools
docker-compose up datatools
```

Otherwise run `docker-compose up` to start a local instance of datatools.

It will listen on http://localhost:4000
