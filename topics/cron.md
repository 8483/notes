Each user has his own crontab i.e. cron table i.e table of scheduled processes.

List

```bash
# list of tasks for current user
crontab -l

# list of tasks for specific user
crontab –u username –l
```

Edit

```bash
# edit crontabs for current user
crontab -e

# edit tasks for specific user
crontab –u username –e
```

Jobs

```bash
# list of crontabs
sudo less /var/spool/cron/crontabs

# root systemwide crontab
sudo less /etc/crontab
```

# Format

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

```bash
# Append hello every minute.
* * * * * echo "hello" >> /home/user/log.txt

# At 23:59 every day.
59 23 * * * echo "hello" >> /home/user/log.txt

# At 23:59 on Sunday.
59 23 * * 0 echo "hello" >> /home/user/log.txt

# At every 20th minute past hour 22 on day-of-month 1 and 15.
*/20 22 1,15 * * echo "hello" >> /home/user/log.txt

# At 10:15 on every 2nd day-of-month from 1 through 10 and on Friday.
15 10 1-10/2 * 5 echo "hello" >> /home/user/log.txt
```

The timing doesn't overlap meaning it happens in both cases, not a combination of the two.
