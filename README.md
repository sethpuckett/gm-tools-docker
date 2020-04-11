# GM Tools

GM Tools is a suite of services for game masters running role-playing games. These services are are accessible through a set of APIs. The GM Tools suite simplifies common activities like rolling dice, generating treasure hauls, and creating random encounters.

## APIs

Documentation for specific APIs can be found in their respective repositories.

* [GM Dice](https://github.com/sethpuckett/gm-dice): Generate dice rolls - port `4000`
* [GM Treasure](https://github.com/sethpuckett/gm-treasure): Generate treasure hauls - port `4010`

## Installation and Initial Setup

The GM Tools suite is designed as a set of interconnected microservices. The suite is Dockerized, so deploying and running all the services is a simple matter. You only need to follow these steps during initial setup. See [Common Commands](#common-commands) for instructions on starting and stopping the services after installation.

Clone all the microservice repositories:
```
git clone https://github.com/sethpuckett/gm-tools-docker.git && \
git clone https://github.com/sethpuckett/gm-dice.git && \
git clone https://github.com/sethpuckett/gm-treasure.git
```

Navigate to the directory for `gm-tools-docker`:
```
cd gm-tools-docker
```

Build the images:
```
docker-compose build
```

Start the containers; setup and run the services:
```
docker-compose up -d
```

Initialize application databases:
```
docker-compose run dice rake db:create db:setup && \
docker-compose run treasure rake db:create db:setup
```

At this point the containers are up and running and the APIs are accessible via the ports listed in [APIs](#apis).

When you're finished clean up the containers with:
```
docker-compose down
```

## Common Commands

Run these in the gm-tools-docker directory.

Create and start the GM Tools suite:
```
docker-compose up
```

Stop and remove containers, networks, images, and volumes:
```
docker-compose down
```
---
The commands below are for individual services; e.g. `dice`, `treasure`

Access Rails console
```
docker-compose run [service] rails c
```

Run `rubocop`
```
docker-compose run [service] rubocop
```

Run `rspec` tests
```
docker-compose run [service] rspec
```

## Additional Notes

This Docker setup is based on the [Compose and Rails](https://docs.docker.com/compose/rails/) guide from Docker. This guide provides additional information and background about the configuration.

Changes to the `docker-compose.yml` file require a rebuild of the suite with:
```
docker-compose up --build
```

Changes to the `Gemfiles` for individual services also require a `bundle install` to update `Gemfile.lock` and sync the file to the host:
```
docker-compose run [service] bundle install
docker-compose build
```
