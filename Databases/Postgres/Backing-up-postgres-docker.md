---
title: Backing up postgres docker
description: 
published: true
date: 2020-08-20T13:28:18.131Z
tags: 
editor: markdown
---

# Backing up postgres docker

To backup Postgres running on Docker image use

```bash
docker exec -t clouddb_postgres_1 pg_dumpall -c -U postgres > dump_`date +%d-%m-%Y"_"%H_%M_%S`.sql
```