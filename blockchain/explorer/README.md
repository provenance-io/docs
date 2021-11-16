---
description: Overview of Explorer capabilities
---

# Explorer

The Blockchain Explorer is a UI displaying blockchain information without having to know how to query the blockchain.&#x20;

The **testnet** Explorer can be reached [here](https://explorer.test.provenance.io/dashboard).

The **mainnet** Explorer can be reached [here](https://explorer.provenance.io/dashboard).

## Prerequisites

* [Docker](https://www.docker.com/get-started)
* [Docker-compose](https://docs.docker.com/compose/)
* local Provenance Blockchain node running with port `9090` open

## Installation

### Using `docker-compose`- Recommended for Non-Developers

1. For a no-mess installation to get Explorer up and running, copy the following code into an accessible file `docker-compose.yml`:

```yaml
version: '3.9'
services:
  explorer-postgres:
    image: provenanceio/explorer-database:latest
    container_name: explorer-postgres
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password1
    ports:
      - 5432:5432

  explorer-service:
    image: provenanceio/explorer-service:latest
    container_name: explorer-service
    ports:
      - 8612:8612
    environment:
      - SPRING_PROFILES_ACTIVE=container
      - DB_USER=postgres
      - DB_PASS=password1
      - DB_HOST=postgres
      - SPRING_DATASOURCE_URL=jdbc:postgresql://explorer-postgres:5432/explorer
      - DB_PORT=5432
      - DB_NAME=explorer
      - DB_SCHEMA=explorer
      - DB_CONNECTION_POOL_SIZE=40
      - SPOTLIGHT_TTL_MS=5000
      - INITIAL_HIST_DAY_COUNT=14
      - EXPLORER_MAINNET=false
      # Hits the locally running node
      - EXPLORER_PB_URL=http://host.docker.internal:9090
      - EXPLORER_FIGMENT_APIKEY=45af964c1cc7292d06db51b5d9a523a4
      - EXPLORER_FIGMENT_URL=https://pio-testnet-1--lcd.datahub.figment.io
    depends_on:
      - explorer-postgres
    links:
      - "explorer-postgres"

  explorer-frontend:
    image: provenanceio/explorer-frontend-generic:latest
    container_name: explorer-frontend
    ports:
      - 3000:3000
    environment:
      - REACT_APP_ENV=local
    depends_on:
      - explorer-service
    links:
      - "explorer-service"
```

1. a. For the most up-to-date compose file, head [here](https://github.com/provenance-io/explorer-service/blob/main/docker/docker-compose.yml).
2. From the location of the above saved file, run \``docker-compose pull`. This pulls the latest versions of the dockers defined in the file.
3. Once the pull is complete, run `docker-compose up`. This starts up the dockers as a single unit.

Once it's up and running, you should be able to access the UI from [http://localhost:3000/dashboard](http://localhost:3000/dashboard).

### Using Github/IDE

Additional prerequisites:

* [Gradle](https://gradle.org)
* [Java](https://www.java.com/en/)
* [Kotlin](https://kotlinlang.org)

1. Clone the repo: [https://github.com/provenance-io/explorer-service](https://github.com/provenance-io/explorer-service)
2. From within your favorite IDE (or CLI, you brute), you'll need to get everything to build. This project uses Gradle as the build tool. Run the following series of commands from the root directory to get everything built:
   1. `sh ./gradlew`
   2. `./gradlew clean proto:generateProto`
   3. `./gradlew build`
3. Start up the database
   1. Run `docker-compose -f docker/docker-compose-db.yml up -d` . This starts the database in a docker environment.
   2. By running the database separately, you can keep it running in the background while starting and stopping the frontend separately.
4. Run `./gradlew bootRun -Dspring.profiles.active=development` to run in a development environment.
5. Start up the frontend
   1. Open `docker/docker-compose.yml`
   2. Make the following changes

```yaml
version: '3.9'
services:
#  explorer-postgres:
#    image: provenanceio/explorer-database:latest
#    container_name: explorer-postgres
#    environment:
#      - POSTGRES_USER=postgres
#      - POSTGRES_PASSWORD=password1
#    ports:
#      - 5432:5432

#  explorer-service:
#    image: provenanceio/explorer-service:latest
#    container_name: explorer-service
#    ports:
#      - 8612:8612
#    environment:
#      - SPRING_PROFILES_ACTIVE=container
#      - DB_USER=postgres
#      - DB_PASS=password1
#      - DB_HOST=postgres
#      - SPRING_DATASOURCE_URL=jdbc:postgresql://explorer-postgres:5432/explorer
#      - DB_PORT=5432
#      - DB_NAME=explorer
#      - DB_SCHEMA=explorer
#      - DB_CONNECTION_POOL_SIZE=40
#      - SPOTLIGHT_TTL_MS=5000
#      - INITIAL_HIST_DAY_COUNT=14
#      - EXPLORER_MAINNET=false
#      # Hits the locally running node
#      - EXPLORER_PB_URL=http://host.docker.internal:9090
#    depends_on:
#      - explorer-postgres
#    links:
#      - "explorer-postgres"
#
  explorer-frontend:
    image: provenanceio/explorer-frontend-generic:latest
    container_name: explorer-frontend
    ports:
      - 3000:3000
    environment:
      - REACT_APP_ENV=local
#    depends_on:
#      - explorer-service
#    links:
#      - "explorer-service"
```

1. Run `docker-compose -f docker/docker-compose.yml up -d`

Once it's up and running, you should be able to access the UI from [http://localhost:3000/dashboard](http://localhost:3000/dashboard).
