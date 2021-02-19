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

### Auto-build of images

:warning: Currently, the auto-build mechanism is broken (see [#17](https://github.com/etalab/transport-ops/issues/17)). See "Manual build" as a work-around in the mean time.

When updating that Dockerfile, push a git tag in the format `a.b.c` and the image will be built by Dockerhub : https://hub.docker.com/r/betagouv/transport/

Then update https://github.com/etalab/transport-site/blob/master/.circleci/config.yml to match the new version.

### Manual build and push of images

As a work-around for [#17](https://github.com/etalab/transport-ops/issues/17):

* Create a [release](https://github.com/etalab/transport-ops/releases) with an upgraded version number
* Build the image locally with the correct tag, and **without cache** to make sure no left-over impact the build:

```
docker build . --no-cache -t betagouv/transport:X.Y.Z
```

* Carefully verify the versions (this will be translated into a testing script later):

```
docker run -it --rm betagouv/transport:X.Y.Z /bin/bash -c 'node --version'
docker run -it --rm betagouv/transport:X.Y.Z /bin/bash -c 'elixir --version'
docker run -it --rm betagouv/transport:X.Y.Z /bin/bash -c "erl -noshell -eval 'erlang:display(erlang:system_info(system_version))' -eval 'init:stop()'"
```

* Read the [docker push documentation](https://docs.docker.com/engine/reference/commandline/push/)
* List the local images with `docker image ls`
* Filter with `docker image ls | grep "betagouv/transport" | grep "X.Y.Z"`
* Push with `docker image push betagouv/transport:X.Y.Z`
* TODO: handle `latest` (but it is currently unused)

## Useful tricks when upgrading

Before creating a tag, the following commands can be used to verify the versions:

```
cd transport-site
docker build . -t test:latest
docker run -it --rm test:latest /bin/bash -c 'node --version'
docker run -it --rm test:latest /bin/bash -c 'elixir --version'
docker run -it --rm test:latest /bin/bash -c "erl -noshell -eval 'erlang:display(erlang:system_info(system_version))' -eval 'init:stop()'"
```
