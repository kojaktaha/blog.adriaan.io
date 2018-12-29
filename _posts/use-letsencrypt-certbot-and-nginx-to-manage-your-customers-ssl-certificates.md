---
title: Use Let's Encrypt, certbot, and NGINX to manage your customers' SSL certificates
---

Sometimes you need to setup SSL certificates for your customers. You want to do this without any downtime and automate it as much as possible. It's not super easy and I didn't find a guide on how to do it, so here is how I did it.

I'm using Ubuntu 18.04 in this tutorial

1. Setup file structure
2. Create a bash script which does request the SSL for a domain
3. Link NGINX to bash script with FastCGI
4. Give NGINX the powed to execute the sudo commands

## Setup file structure

Normally NGINX config lives in `/etc/nginx/`. It reads the website out of `/etc/nginx/sites`

## Create a bash script which does request the SSL for a domain

```
chmod +x ./request-ssl.sh
```

## Give NGINX the powed to execute the sudo commands

```
sudo visudo
```

## Read more
- [certbot.eff.org/docs/using.html](https://certbot.eff.org/docs/using.html)
- [nginx.com/resources/wiki/start/topics/examples/fastcgiexample/](https://www.nginx.com/resources/wiki/start/topics/examples/fastcgiexample/)
