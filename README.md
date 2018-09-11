[linuxserverurl]: https://linuxserver.io
[forumurl]: https://forum.linuxserver.io
[ircurl]: https://www.linuxserver.io/irc/
[podcasturl]: https://www.linuxserver.io/podcast/
[appurl]: https://mariadb.org/
[hub]: https://hub.docker.com/r/lsioarmhf/mariadb/

[![linuxserver.io](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/linuxserver_medium.png)][linuxserverurl]

The [LinuxServer.io][linuxserverurl] team brings you another container release featuring easy user mapping and community support. Find us for support at:
* [forum.linuxserver.io][forumurl]
* [IRC][ircurl] on freenode at `#linuxserver.io`
* [Podcast][podcasturl] covers everything to do with getting the most from your Linux Server plus a focus on all things Docker and containerisation!

# lsioarmhf/mariadb
[![](https://images.microbadger.com/badges/version/lsioarmhf/mariadb.svg)](https://microbadger.com/images/lsioarmhf/mariadb "Get your own version badge on microbadger.com")[![](https://images.microbadger.com/badges/image/lsioarmhf/mariadb.svg)](https://microbadger.com/images/lsioarmhf/mariadb "Get your own image badge on microbadger.com")[![Docker Pulls](https://img.shields.io/docker/pulls/lsioarmhf/mariadb.svg)][hub][![Docker Stars](https://img.shields.io/docker/stars/lsioarmhf/mariadb.svg)][hub][![Build Status](https://ci.linuxserver.io/buildStatus/icon?job=Docker-Builders/armhf/armhf-mariadb)](https://ci.linuxserver.io/job/Docker-Builders/job/armhf/job/armhf-mariadb/)

One of the most popular database servers. Made by the original developers of MySQL

[![mariadb](https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/mariadb-git.png)][appurl]

## Usage

```
docker create \
--name=mariadb \
-p 3306:3306 \
-e PUID=<UID> \
-e PGID=<GID> \
-e MYSQL_ROOT_PASSWORD=<DATABASE PASSWORD> \
-e TZ=<timezone> \
-v </path/to/appdata>:/config \
lsioarmhf/mariadb
```

## Parameters

`The parameters are split into two halves, separated by a colon, the left hand side representing the host and the right the container side. 
For example with a port -p external:internal - what this shows is the port mapping from internal to external of the container.
So -p 8080:80 would expose port 80 from inside the container to be accessible from the host's IP on port 8080
http://192.168.x.x:8080 would show you what's running INSIDE the container on port 80.`


* `-p 3306` - mysql port
* `-v /config` - Contains the db itself and all assorted settings. 
* `-e MYSQL_ROOT_PASSWORD` - set this to root password for installation (minimum 4 characters)
* `-e PGID` for GroupID - see below for explanation
* `-e PUID` for UserID - see below for explanation
* `-e TZ` - for timezone information *eg Europe/London, etc*

It is based on ubuntu xenial with s6 overlay, for shell access whilst the container is running do `docker exec -it mariadb /bin/bash`.

### User / Group Identifiers

Sometimes when using data volumes (`-v` flags) permissions issues can arise between the host OS and the container. We avoid this issue by allowing you to specify the user `PUID` and group `PGID`. Ensure the data volume directory on the host is owned by the same user you specify and it will "just work" ™.

In this instance `PUID=1001` and `PGID=1001`. To find yours use `id user` as below:

```
  $ id <dockeruser>
    uid=1001(dockeruser) gid=1001(dockergroup) groups=1001(dockergroup)
```

## Setting up the application 

If you didn't set a password during installation, (see logs for warning) use 
`mysqladmin -u root password <PASSWORD>` 
to set one at the docker prompt...

NOTE changing the MYSQL_ROOT_PASSWORD variable after the container has set up the initial databases has no effect, use the mysqladmin tool to change your mariadb password. 

Unraid users, it is advisable to edit the template/webui after setup and remove reference to this variable.

Find custom.cnf in /config for config changes (restart container for them to take effect)
, the databases in /config/databases and the log in /config/log/myqsl

## Info

* Shell access whilst the container is running: `docker exec -it mariadb /bin/bash`
* To monitor the logs of the container in realtime: `docker logs -f mariadb`

* container version number 

`docker inspect -f '{{ index .Config.Labels "build_version" }}' mariadb`

* image version number

`docker inspect -f '{{ index .Config.Labels "build_version" }}' lsioarmhf/mariadb`

## Versions

+ **10.09.18:** Rebase to ubuntu bionic.
+ **12.01.18:** Fix continuation lines
+ **14.09.17:** Gracefully shut down mariadb
+ **11.01.16:** Initial Release.
