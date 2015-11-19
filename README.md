# docker-deploy
Deploy your code to any Docker servers

### About ###
This script provides a super simple way of deploying your projects onto remote Docker hosts by only utilizing rsync, SSH, and docker-compose.

### Requirements ###

* A project folder with a docker-compose.yml file
* An SSH access to a server with Docker and docker-compose installed

### Usage ###
./docker-deploy [project_folder] [docker_server]

### Example ###

* Let's say ~/Projects/site.com is my local dev environment and it has a docker-compose.yml file
* And I have SSH access to jae@server.com which has Docker installed
* Simply run this to deploy your project to your target Docker server

```
cd ~/Projects
docker-deploy site.com jae@server.com
```

### What does it do? ###
The docker-deploy script is simply a wrapper around a couple bash scripts.
Basically it does the following:
* rsync -avzr ./site.com jae@server.com:~/sites/site.com
* ssh jae@server.com "cd ~/sites/site.com && docker-compose up -d"

### That's it! ###
Simple, right?
Sometimes, I feel people tend to over-engineer and deployment is one of those areas.
Unless you are serving Google-traffic, this deployment style will work fine for most of your projects.
Hope you enjoy it.

### For Virtual Hosting ###

I am often surprised to come across those who have not yet realized the power of jwilder/proxy.
If you have multiple websites you want to host, then you should definitely consider having jwilder/proxy running on your Docker server listening to your port 80.

```
docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
```

Then in the docker compose file, simply assign an environmental variable called VIRTUAL_HOST with the hostname you wish to host.
Take a look at the docker compose file below to see what I mean.

### docker-compose.yml ###

Here is how my typical docker-compose.yml looks:

##### php #####
```
app:
image: jaequery/php
command: /start.sh
environment:
- VIRTUAL_HOST=site.app,site.jaequery.com
restart: always
volumes:
- .:/web
tty: true
```

##### ruby #####

```
app:
image: jaequery/honeybadger
links:
- db:honeybadger-postgres
command: /init_container.sh
environment:
- VIRTUAL_HOST=honeybadger.app,honeybadger.jaequery.com
restart: always
db:
image: postgres
environment:
- POSTGRES_PASSWORD=j
restart: always
```

### Tip ###

* You may want to place docker-deploy script to your /usr/local/bin for global access
