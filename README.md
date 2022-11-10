# transport-ops

Scripts and config files for provision and deploy transport and required services

# New release process (GitHub actions + GitHub Container Registry)

To create a new release as a Docker image, just create a GitHub release (with tag name == release name), with a changelog.

Currently, the correct release/tag name can be built from a local command-line, using `rake get_image_version`.

On release creation (see https://github.com/etalab/transport-ops/blob/master/.github/workflows/docker.yml), a build will start, and should normally result into the publication of a GitHub-hosted Docker image named just like the release.

You can find the resulting image here: https://github.com/etalab/transport-ops/pkgs/container/transport-ops

 One major caveat: the workflow must exist at the moment the tag is created (https://github.community/t/workflow-set-for-on-release-not-triggering-not-showing-up/16286/7):

 > The trigger only executes when a release is created using a tag that contains the workflow.

# How to rebuild the package for an existing release (security patches etc)

See https://github.com/etalab/transport-ops/issues/45:

* For future releases (> `elixir-1.14.1-erlang-24.3.4.6-ubuntu-focal-20211006-transport-tools-1.0.5`), it will normally become possible to go to "actions / select job on left / re-run on tag" to rebuild a given image and run tests automatically
* It is also possible to move an existing release to "pre-release" (then cancel the job automatically created) then back again to "release", this will re-trigger the release.

# LEGACY DOCUMENTATION below - kept until the moment we clean it

## Image naming convention

In the past images were named "betagouv/transport:x.y.z".

Now we name them after the Elixir, Erlang and Alpine versions actually used (like the base `hexpm` image it uses).

* Base name: `hexpm/elixir:1.10.4-erlang-23.2.7.2-alpine-3.13.3`
* Replace `hexpm/elixir:` by `betagouv/transport:elixir-`
* Target name: `betagouv/transport:elixir-1.10.4-erlang-23.2.7.2-alpine-3.13.3`

You can use `rake get_image_version` to build the expected target name out of the `Dockerfile` in automated fashion.

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
IMAGE_VERSION=$(rake get_image_version)
IMAGE_NAME=betagouv/transport:$IMAGE_VERSION
docker build transport-site --no-cache -t $IMAGE_NAME --progress=plain
```

* Carefully verify the versions (this will be translated into a testing script later):

```
docker run -it --rm $IMAGE_NAME /bin/bash -c 'node --version'
docker run -it --rm $IMAGE_NAME /bin/bash -c 'elixir --version'
# only major
docker run -it --rm $IMAGE_NAME /bin/bash -c "erl -noshell -eval 'erlang:display(erlang:system_info(system_version))' -eval 'init:stop()'"
# full version (https://stackoverflow.com/a/34326368)
docker run -it --rm $IMAGE_NME /bin/bash -c "erl -eval '{ok, Version} = file:read_file(filename:join([code:root_dir(), \"releases\", erlang:system_info(otp_release), \"OTP_VERSION\"])), io:fwrite(Version), halt().' -noshell"
```

* Read the [docker push documentation](https://docs.docker.com/engine/reference/commandline/push/)
* List the local images with `docker image ls`
* Filter with `docker image ls | grep "betagouv/transport" | grep $IMAGE_VERSION`
* Push with `docker image push $IMAGE_NAME`
* Verify tag presence at https://hub.docker.com/repository/docker/betagouv/transport
* TODO: handle `latest` (but it is currently unused)

## Useful tricks when upgrading

Before creating a tag, the following commands can be used to verify the versions:

```
cd transport-site
docker build . -t test:latest --progress=plain
docker run -it --rm test:latest /bin/bash -c 'node --version'
docker run -it --rm test:latest /bin/bash -c 'elixir --version'
docker run -it --rm test:latest /bin/bash -c "erl -noshell -eval 'erlang:display(erlang:system_info(system_version))' -eval 'init:stop()'"
```
