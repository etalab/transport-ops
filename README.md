# transport-ops
Scripts and config files for provision and deploy transport and required services

## Production configuration and deployment

The production requires the following services:

* transport-site: the web frontend

  * Hosted at https://transport.data.gouv.fr
  * Elixir application hosted on CleverCloud
  * The _master_ branch of https://github.com/etalab/transport-site is deployed by CircleCI

* udata: references and/or stores the datasets and comments

  * Hosted at https://data.gouv.fr

## Docker for transport-site CI

The _Transport Site_ project is tested with CircleCI in a Docker container that is defined in `transport-site/Dockerfile`.

When updating that Dockerfile, push a git tag in the format `a.b.c` and the image will be built by Dockerhub : https://hub.docker.com/r/betagouv/transport/

Then update https://github.com/etalab/transport-site/blob/master/.circleci/config.yml to match the new version.
