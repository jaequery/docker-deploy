# docker-deploy
Deploy your code to any Docker servers

### About ###
I wanted an absolute most simple way to deploy my local codebase on to remote servers with Docker running.
Since I now dockerize most of my projects, I found it cumbersome everytime I needed to deploy my code.
I've tried many approaches and there is the typical SSH/git approach, the docker-machine approach, ansible approach, etc... and the list goes on. They all had some quirky faults and I decided to make something simple for my needs.

### Requirements ###

* A project folder with a docker-compose file
* A docker server you wish to deploy to

### Usage ###
./docker-deploy [project_folder] [docker_server]

### Example ###

* Let's say ~/Projects/site.com is my local dev environment and it has a docker-compose.yml file
* And I have SSH access to jae@server.com

```
cd ~/Projects
docker-deploy site.com jae@server.com
```

### For best results ###

If you are a web developer and you have multiple websites to host, then you should have a proxy such as jwilder/proxy running on your Docker server.

```
docker run -d -p 80:80 -v /var/run/docker.sock:/tmp/docker.sock:ro jwilder/nginx-proxy
```

I do this to host dozens of sites in a single Docker container.

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
