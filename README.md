# Requirements

Docker

# Run

To start the project, just run:

```bash
docker-compose up
```

If you want to start containers in background (as a daemon), add the `-d` flag:

```bash
docker-compose up -d
```

If this is your first run, then after containers are up and ready, run the next command to setup/install the project dependencies:

```bash
docker-compose run --rm app ./build-deploy/first-run.sh
```

# Stop

You can stop containers by typing `Cmd + C` on Mac or `Ctrl + C` on Windows/Linux. 

If you started the project in background, then run:

```bash
docker-compose stop
```
# Force Running Project

There are many unexpected problems to stop running project. ex. Low version Ubuntu, OpenVPN connect problem.
```bash
sudo service docker restart
docker-compose down --rmi=local -v -f
```
This command will stop docker and restart, also remove all docker containers.
And Follow above steps again. Important : 1.if you use vpn tool, disable it. 2. Maintain Memory 4GB: Run ElasticSearch docker Container.

# Cleanup

If you want to cleanup your Docker instances for a fresh start, run:

```bash
docker-compose down --rmi=local -v
```

This command will stop and delete the containers, local images and volumes.

# Updating external images

If you want to get latest versions of your external images, run:

```bash
docker-compose pull
```

# Queues

Once your containers are started, you can start the queue listeners when needed.

### Mailing Queue

```bash
docker-compose run --rm app php artisan queue:work sqs_mail
```

### SMS Queue

```bash
docker-compose run --rm app php artisan queue:work sqs_sms
```

# Tests

To run the test suite:
```bash
docker-compose run --rm app ./build-deploy/test.sh
```

# ElasticSearch Indexes

To regenerate the elastic search indexes:
```bash
docker-compose run --rm app php artisan elastic:setup-indexes
```

# Docker Setup

```Docker Setup

    1. Install the docker
    2. Configure .env file and change mysql db_host to 127.0.0.1 and also change in docker.yaml file. mysql Environment to `MYSQL_HOST=127.0.0.1` and `app: aliases: - 127.0.0.1`
    3. Inside your app run `composer commands` to update all packages
    4. after that run make sure you have the righ permissions for that
        go to app->build-deploy
            run `sudo chmod  755 first_run.sh`
            run `sudo chmod  755 run.sh`
        next go to app->build-deploy->image
            run `sudo chmod  +x run.sh`

    5. Now run  `docker-compose build` and  inside your app
    6. Start Laravel server to access app
    ```
