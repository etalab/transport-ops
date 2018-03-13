# transport-ops
Scripts and config files for provision and deploy transport and required services

## Usage

If you are using _Docker Machine_ (probably if you are running on macOSX or Windows), run

```
docker-machine create datatools
docker-machine start datatools
docker-compose up datatools
```

Otherwise run `docker-compose up` to start a local instance of datatools.

It will listen on http://localhost:4000
