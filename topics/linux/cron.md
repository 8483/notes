# Troubleshooting

1. **LEAVE AN EMPTY NEW LINE AT THE END OF THE FILE!**

    - Cron jobs may fail to execute correctly if there's no newline at the end of the crontab file because the cron daemon expects each line to end with a newline character. If the last line does not end with a newline, the cron daemon might not read it properly, which can result in that job not being executed.

2. Not using absolute paths

```bash
* * * * * /bin/echo "cron works" >> /tmp/file
```

3. Not using a PATH variable

```bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
* * * * * echo "cron works" >> /tmp/file
```

4. Not escaping characters

```bash
# mysqldump --databases db > /home/user/backup-$(date +%Y%m%d-%H%M%S)
mysqldump --databases db > /home/user/backup-$(date +\%Y\%m\%d-\%H\%M\%S)
```

NOTE: `echo` won't work because `cron` runs in its own shell.

# Cron

Each user has his own crontab i.e. cron table i.e table of scheduled processes.

**List**

```bash
# list of tasks for current user
crontab -l

# list of tasks for specific user
crontab –u username –l
```

**Edit**

```bash
# edit crontabs for current user
crontab -e

# edit tasks for specific user
crontab –u username –e

# edit tasks for root user
sudo crontab -e
```

**Jobs**

```bash
# list of crontabs
sudo less /var/spool/cron/crontabs

# root systemwide crontab
sudo less /etc/crontab
```

# Format

**NOTE: Cron doesn't do seconds. Minimum is every minute.**

```
.---------------- minute (0 - 59)
|  .------------- hour (0 - 23)
|  |  .---------- day of month (1 - 31)
|  |  |  .------- month (1 - 12) OR jan, feb, mar, apr...
|  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun, mon, tue, wed, thu, fri, sat
|  |  |  |  |
*  *  *  *  * command to be executed

*     all
,     list of values
x-y   range of values
/x    recurring i.e. every x times
```

**NOTE: The timing doesn't overlap, meaning it happens in both cases, not a combination of the two.**

```bash
# Every minute
* * * * *

# Every hour
0 * * * *

# Every 2 hours
0 */2 * * *

# Every day @ 23:59
59 23 * * *

# Sunday @ 23:59
59 23 * * 0

# At every minute past every hour from 7 through 18 on every day-of-week from Monday through Friday
* 7-18 * * 1-5

# At every 20th minute past hour 22 on day-of-month 1 and 15
*/20 22 1,15 * *

# At 10:15 on every 2nd day-of-month from 1 through 10 and on Friday
15 10 1-10/2 * 5

# Every 2 hours between 06:00 and 18:00, on Monday - Friday.
0 6-18/2 * * 1-5
```
