# MultiMC TeamCity bootstrapping docker thing!
Docker compose config to create working [TeamCity](https://www.jetbrains.com/teamcity/) server with PostgreSQL

Based on [Egregors/teamcity-docker-compose](https://github.com/Egregors/teamcity-docker-compose) and modified for ... purposes.

Original license is GPL 3, but this is not interesting for actual redistribution. Go to the original source instead.

## Configuration

* Copy `env.example` as `.env`.
* Set a decent Postgres username and password variables in `.env`
* Don't push `.env`, obviously.

## Startup

Build images first:

```
cd teamcity-docker-compose
docker-compose build
```

Then start the service:

```
docker-compose up
```

After initialisation Web Interface will be available on `https://teamcity.multimc.org/`, provided everything is set up correctly on CF and deth001.

### Setup DB

Open `https://teamcity.multimc.org/`

Set PostgreSQL as database type, upload [JDBC driver](https://jdbc.postgresql.org/download/postgresql-42.2.4.jar) into
 `/opt/teamcity/data/lib/jdbc/` then click «Refresh JDBC drivers»

![Alt text](raw/img/1.png?raw=true)

Configure DB connection:

![Alt text](raw/img/2.png?raw=true)

Authorize your Agent:

![Alt text](raw/img/3.png?raw=true)

## Backup / restore

You may use JetBrains way to [backup](https://confluence.jetbrains.com/display/TCD10/TeamCity+Data+Backup) 
or [restore](https://confluence.jetbrains.com/display/TCD10/Restoring+TeamCity+Data+from+Backup) your server


## Update

If you see a notice that a new version is available, you may update your TeamCity that way:

```
# build new version
docker-compose build --pull --no-cache

# stop and remove old containers
docker-compose stop
docker-compose rm

# create and up new containers
docker-compose up -d
```

After an update, you need to reauthorize your agents.

### Updating maintenance

Sometimes, during update you may get «maintenance is required» message instead of login page. 
It's ok! To login in a maintenance mode you need to enter an authentication token. You may find it in the logs:
`docker-compose logs -f`

Try to find something like this:

```
teamcity-server_1                    | [TeamCity] Administrator can login from web UI using authentication token: 8755994969038184734
```
