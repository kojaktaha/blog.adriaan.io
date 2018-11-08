---
title: Log CRON to syslog on Ubuntu
---

I got this message in by logs

```
No MTA installed, discarding output
```

This is because CRON (crontab) is sending it's data to a mail client. Ubuntu does not come with a mail client, so it throws this error. But I don't want mail for error logging, so I looked online and found that you can use logger and pipe your CRON command to it.

I use a date function, so make sure to escape the `%`'s.

```
02 4 * * * (/usr/bin/pg_dump your_database -U your_user -Fc > "/your/backup/folder/$(date '+\%Y-\%m-\%d').sql") 2>&1 | /usr/bin/logger -t BACKUP
```

So after is has been run you can find the relevant log messages.

```bash
sudo grep BACKUP /var/log/syslog
```
