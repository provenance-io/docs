# P8e Setup

## Services

The following are the services that make up a fully functional P8e environment:

* p8e-api
* object-store
* Provenance Blockchain node
* postgres 9.6
* elasticsearch
* p8e-webservice \(optional\)
* p8e-ui \(optional\)
* obect-store-locator \(required only for external multi-party contract execution\)

## Local

A [docker compose](https://github.com/provenance-io/p8e-docker-compose) environment is provided to quickly bring up a fully configured P8e environment. This includes a four node Provenance Blockchain cluster. Having some familiarity with [docker](https://docs.docker.com/) and [docker-compose](https://docs.docker.com/compose/) will help.

The following steps will setup an environment and execute your first P8e contract.

```bash
cd $BASE_CODE_DIR
git clone git@github.com:provenance-io/p8e-docker-compose.git

cd p8e-docker-compose
./bin/update && ./bin/one-time-setup.sh
./bin/start

cd $BASE_CODE_DIR
git clone git@github.com:provenance-io/p8e-gradle-plugin.git

# requires java 11
cd p8e-gradle-plugin/example-java
./gradlew clean build
source $BASE_CODE_DIR/p8e-docker-compose/env/host/env
./gradlew p8eClean p8eBootstrap --info
./gradlew runner:run
```

From here you're all set to start creating your own contract specifications, and then publishing and executing them. The remaining documentation will walk you through these steps and help you understand P8e.

## Testnet/Mainnet



