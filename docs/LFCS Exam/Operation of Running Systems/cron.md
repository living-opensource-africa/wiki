---
title: Schedule tasks to run at a set date and time
date: 2021-09-06
authors:
  - Muse Sisay
---

```text
# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
```

- Visit `man 5 cron` for explanation
- `/var/spool/cron` directory where cron checks for user jobs
- A field be specified as follows
    - list of comma separated values `1,4,6`
    - range of values (inclusive).  `8–11`
    - steps values in conjunction with range `0–4,8–12`
    - `*` to indicate all


!!! tip "Exam Tip"
    To have something to reference at when creating taks run the following:
    ```bash
    cat /etc/crontab >> /var/spool/cron/$USER
    ```

## Managing Access to crontab

cron.allow and cron.deny files can be used to specify who can access crontab command.

- If `/etc/cron.deny` exists it will implicity allow users to 
run `/etc/crontab -e` and will disallow listed users.

- If `cron.allow` exists it will only allow user listed in the file and explicity 
disallow user not listed from  running `crontab -e`

- If neither file exist only the  root user is allowed to run crontab -e

| .                         | cron.deny exists                           | cron.deny does not exist                   |
| ------------------------- | ------------------------------------------ | ------------------------------------------ |
| cron.allow exist          | only user listed in cron.allow are allowed | only user listed in cron.allow are allowed |
| cron.allow does not exist | user listed in cron.deny aren't allowed    | only root user can use crontab -e          |


## Examples

```bash
*/15 12 * * * logger "doing something"
```
This would run it every 15 minutes starting from 12:00 o'clock (24 hour format) till 13:00.

So, the task would run at `12:00`, `12:15`, `12:30` and  `12:45`
 

```bash
15 12 15 * Fri logger "doing something"
```
runs the command on the 15th of every month, and every Friday at 12:15 

!!! Note 
    When the Day of month and Day of week fields are both other than *,
    the command is executed when either of these two fields are satisfied.

## Resource
- [Cron Checker](https://crontab.guru)