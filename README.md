# docker-deploy
Deploy your code to any Docker servers

### About ###
I wanted an absolute most simple way to deploying my code to remote servers.
Since I dockerized most of my projects, I still found it cumbersome to deploy my code.
I've tried docker-machine and I quickly realized to deploy my code changes, it gets difficult as I'd need to re-build the image from the Dockerfile over and over.
And because I tend to volumize the codebase, docker-machine wasn't suitable for my needs.

I just needed a simple solution so here it is.

### Requirements ###

1) A directory with a docker-compose file
1) A docker server you wish to deploy to

### Usage ###
./docker-deploy [folder-with-a-docker-compose-file] [server]

### Example ###

* Let's say ~/Projects/site.com is my local dev environment and it has a docker-compose.yml file
* And I have SSH access to jae@server.com

```
cd ~/Projects
docker-deploy site.com jae@server.com
```

### Tip ###

- You can run the docker-deploy to deploy your codebase
- Run the command again and it should update your codebase
- It is a good idea to have a proxy running in your Docker server.
- That way you can virtual host as many sites as you want
