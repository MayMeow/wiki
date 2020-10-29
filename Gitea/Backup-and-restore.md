---
title: Gitea backup and restore
description: 
published: true
date: 2020-08-21T06:04:13.479Z
tags: 
editor: undefined
---

# Gitea backup and restore

Use following script. It's need tu be started with same user as gitea is. This is in app.ini file as `RUN_USER` variable.

```bash
docker exec -u git -it -w /tmp $(docker ps -qf "name=gitea_gitea_1") bash -c '/app/gitea/gitea dump -c /data/gitea/conf/app.ini'
```

This script will create .zip file in /tmp of your gitea image so do not forge to copy it to presistent storage. I have mountoed volume `/gitea-backup`.

```bash
docker exec -it -w /tmp $(docker ps -qf "name=gitea_gitea_1") bash -c 'cp -r /tmp/* /gitea-backup'
```

## Restore

Restore must be made manually 

```bash
unzip gitea-backup-file.zip -d restore_tmp
```

```bash
cd restore_tmp
ls -la

#output
drwxr-xr-x 6 root root   4096 Aug 20 13:43 .
drwxr-xr-x 7 root root   4096 Aug 20 13:43 ..
-rw-r--r-- 1 root root   2143 Aug 16 12:53 app.ini
drwxr-xr-x 7 root root   4096 Aug 20 13:43 data
-rw------- 1 root root 620222 Aug 16 12:59 gitea-db.sql
drwx------ 2 root root   4096 Aug 16 12:55 lfs
drwxr-xr-x 2 root root   4096 Aug 16 12:55 log
drwxr-xr-x 3 root root   4096 Aug 16 12:14 repos
```

next copy folders and files

```bash
cp app.ini /home/emma/gitea/data/gitea/gitea/conf/app.ini
cd data && cp -r . /home/emma/gitea/data/gitea/gitea/
cd repos && cp -r . /home/emma/gitea/git/repositories/
cp -r lfs /home/emma/gitea/data/gitea/gitea
cp -r log /home/emma/gitea/data/gitea/gitea
```

Restore database

```bash
cat gitea-db.sql | docker exec -i <PostgreSQL-container-id> psql -U postgres
```

or use adminer.

Next restart gitea container

```bash
docker-compose restart gitea
```

My `docker-compose.yml` file

```yaml
version: "2"

networks:
  frontend:
    external: true

services:
  gitea:
    image: gitea/gitea:latest
    environment:
      - USER_UID=1000
      - USER_GID=1000
    restart: unless-stopped
    networks:
      - frontend
    volumes:
      - ./data/gitea:/data
      - ./data/gitea-backup:/gitea-backup
    ports:
      - "222:22"
    # depends_on:
    #   - gitea-db
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.gitea.entrypoints=web"
      - "traefik.http.routers.gitea.rule=Host(`git.domain.tld`)"
      - "traefik.http.middlewares.gitea-websecure-redirect.redirectscheme.scheme=https"
      - "traefik.http.routers.gitea.middlewares=gitea-websecure-redirect"
      - "traefik.http.routers.gitea-secure.entrypoints=websecure"
      - "traefik.http.routers.gitea-secure.rule=Host(`git.domain.tld`)"
      - "traefik.http.routers.gitea-secure.tls=true"
      - "traefik.http.routers.gitea-secure.tls.certresolver=le"
      - "traefik.http.routers.gitea.service=gitea"
      - "traefik.http.services.gitea.loadbalancer.server.port=3000"
      - "traefik.docker.network=frontend"
```

This manual is customized for gitea hosted in docker (look file above) with external database. 
