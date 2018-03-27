# transport-ops
Scripts and config files for provision and deploy transport and required services

## Docker for transport-site CI

The _Transport Site_ project is tested with CircleCI in a Docker container that is defined in `transport-site/Dockerfile`.

When updating that Dockerfile, push a git tag in the format `a.b.c` and the image will be built by Dockerhub : https://hub.docker.com/r/betagouv/transport/

Then update https://github.com/etalab/transport-site/blob/master/.circleci/config.yml to match the new version.


## Usage

If you are using _Docker Machine_ (probably if you are running on macOSX or Windows), run

```
docker-machine create datatools
docker-machine start datatools
docker-compose up datatools
```

Otherwise run `docker-compose up` to start a local instance of datatools.

It will listen on http://localhost:4000
