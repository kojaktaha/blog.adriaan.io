---
title: Turn on automatic apt-get updates and upgrades
---

Often times when I login to a VPS I get this a message like this:

```
32 packages can be updated.
11 updates are security updates.
```

I don't want to type `sudo apt-get update` and `sudo apt-get upgrade` all the time.

But `sudo apt-get update` is interactive by default so you have to hit `enter` or type `y`. So I did a little reseach on how to put `apt-get` in noninteractive mode and it's quite simple. Just append `-y` (or `-yes` to it).

So when I want to auto upgrade my packages I can install a cronjob as root (`sudo crontab -e`) with this line in it:

```
20 4 * * * sudo apt-get -y update && sudo apt-get -y upgrade
```

Which will update and upgrade my packages at 4:20 AM.
