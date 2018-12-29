---
title: Turn on automatic apt-get updates and upgrades
---

Often times when I login to a VPS I get this a message like this:

```
32 packages can be updated.
11 updates are security updates.
```

I don't want to type `sudo apt-get update` and `sudo apt-get upgrade` all the time.

But `sudo apt-get update` is interactive by default so you have to hit `enter` or type `y`. So I did a little reseach on how to put `apt-get` in noninteractive mode and it's quite simple. Just append `-y` (or `--yes` to it).

So when I want to auto upgrade my packages I can install a cronjob as root (`sudo crontab -e`) with this line in it:

```
20 4 * * * sudo apt-get update && sudo apt-get -y upgrade
```

Which will update and upgrade my packages at 4:20 AM.

To log it to syslog:

```
20 4 * * * sudo apt-get update && sudo apt-get --with-new-pkgs --yes upgrade 2>&1 | /usr/bin/logger -t UPDATEUPGRADE
```

To update new packages you can run `--with-new-pkgs` (it's a [little safer](https://askubuntu.com/questions/694403/is-upgrade-with-new-pkgs-safer-than-dist-upgrade) than dist-upgrade)

Read [my other blog post](/log-cronjob-output-to-syslog-in-ubuntu.html) on the logger function.
