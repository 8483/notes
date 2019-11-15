Each user has his own crontab i.e. cron table i.e table of scheduled processes.

```bash
# List of tasks
crontab -l

# Edit crontabs i.e Add a cron job that runs as root. Opens editor.
sudo crontab -e

# List of crontabs
sudo less /var/spool/cron/crontabs

# Systemwide crontab
sudo less /etc/crontab
```

# Format

```bash
# minute, hour, day-of-month, month-of-year, day-of-week, command
# *   = Every
# ,   = List of values
# x-y = Range of values
# /x  = Recurring i.e. every x times

# Append hello every minute.
* * * * * echo "hello" >> home/user/log.txt

# At 03:00 on Sunday.
0 3 * * 0 echo "hello" >> home/user/log.txt

# At every 20th minute past hour 22 on day-of-month 1 and 15.
*/20 22 1,15 * * echo "hello" >> home/user/log.txt

# At 10:15 on every 2nd day-of-month from 1 through 10 and on Friday.
15 10 1-10/2 * 5 echo "hello" >> home/user/log.txt
```
The timing doesn't overlap meaning it happens in both cases, not a combination of the two.