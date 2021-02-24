---
title: Samba Install and Configure
description: 
published: true
date: 2021-02-24T12:11:26.112Z
tags: 
editor: markdown
dateCreated: 2021-02-24T12:11:26.112Z
---

## Install samba

``` bash
sudo apt update
sudo apt install samba

# check status
sudo systemctl status smbd
```

## Configure firewall (optional)

``` bash
sudo ufw allow 'Samba'
```

## Create Backup of configuration files

``` bash
sudo cp /etc/samba/smb.conf{,.backup}
```

## Configuration

``` bash
sudo nano /etc/samba/smb.conf
```

Defailt values

* `server role = standalone server`
* `interfaces = 127.0.0.0/8 eth0` Listen on all interfaces
* `bind interfaces only = yes`

Ater changes you can test config file with `testparm`. If everythinq si OK you will see `Loaded services file OK`

Restart Samba services with:

``` bash
sudo systemctl restart smbd
sudo systemctl restart nmbd
```

## Folders

Instead of using default folder create new location for example `/samba`

``` bash
sudo mkdir /samba
```

And change group to default samba's group `sambashare`

``` bash
sudo chgrp sambashare /samba
```

## Create samba users

Create users with following command

``` bash
sudo useradd -M -d /samba/maymeow -s /usr/sbin/nologin -G sambashare maymeow
```

* `-M` do not create home directory
* `-d /samba/maymeow` set users directory to `/samba/maymeow`
* `-s /usr/sbin/nologin` disable shell access to th
* `-G sambashare` add user to `sambashare` group

Create home directory for this user and chage directory permission

``` bash
sudo mkdir /samba/maymeow
sudo chown maymeow:sambashare /samba/maymeow

sudo chmod 2770 /samba/josh
```
