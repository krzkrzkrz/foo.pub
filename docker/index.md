# Docker

[Docker](https://docker.com/) Accelerate how you build, share, and run modern applications.

## Tutorial building out of Rails application

[Docker Rails](https://docs.docker.com/samples/rails)

## Build the project

```
docker-compose run --no-deps web rails new <path_to_project> --force --database=postgresql
```

First, Compose builds the image for the web service using the Dockerfile. The `--no-deps` tells Compose not to start linked services

Then it runs `rails new` inside a new container, using that image. Once it's done, you should have generated a fresh app.

## Build the image

Now that you’ve got a new Gemfile, you need to build the image again. (This, and changes to the Gemfile or the Dockerfile, should be the only times you'll need to rebuild.)

```
docker compose build
```

## Boot the app

```
docker compose up
```

Rebuilding:

```
docker compose up --build
```

## Stop the app

```
docker compose down
```

## Restart Docker

```
docker compose restart
```

## Rebuild the application

If you make changes to the Gemfile or the Compose file to try out some different configurations, you need to rebuild. Some changes require only `docker-compose up --build`, but a full rebuild requires a re-run of `docker-compose run web bundle install` to sync changes in the `Gemfile.lock` to the host, followed by `docker-compose up --build`.

Here is an example of the first case, where a full rebuild is not necessary. Suppose you simply want to change the exposed port on the local host from 3000 in our first example to 3001. Make the change to the Compose file to expose port 3000 on the container through a new port, 3001, on the host, and save the changes:

In `docker-compose.yml`:

```
ports:
  - "3001:3000"
```

Now, rebuild and restart the app with `docker-compose up --build`.

## Creating migrations

```
docker compose run api bundle exec rake db:create --trace
```

## Running migrations

```
docker compose run api rake db:create --trace
```

## Remove all local images

```
docker rmi -f $(docker images -aq)
```

## Run a specific docker compose file

```
docker compose --file docker-compose.development.yml up --build
```

docker ps
docker exec -ti --user root <docker-container-id> /bin/bash
docker exec -it d8c35b5c5f08 psql -U postgres -d template1
docker-compose up -d --force-recreate db
docker-compose run api bundle exec rake db:create --trace
docker-compose run api bundle exec rails c
```
docker build -t rails-toolbox -f Dockerfile.rails .
```

```
docker build -t rails-toolbox -f Dockerfile.rails .
```

