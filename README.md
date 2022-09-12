# Quimby

An open-source small tool written in GO for automating backups of FreeBSD jails managed with 
[Bastille](https://bastillebsd.org/).

(Quimby: a beginner surfer who is usually annoying)

## Requirements

- [FreeBSD](https://freebsd.org/)
- [BastilleBSD](https://bastillebsd.org/)
- ZFS filesystem for live backups.

## Disclaimer

I am a beginner programmer that wanted to automate the backup of all of my deployed jails on my servers.
I use GO as I find it to be the best programming language for my use case and abilities. Please use this 
tool as your own risk knowing that I probably will not have the time to work on this project full time on 
a regular basis.

## Features

- If no options are provided, it determines the list of current jails and backup them live in .gz format via ZFS snapshot
- If on UFS filesystem, it will warn you that jails can be only safely backup (start/stop)
- By default it removes backup files that are older than 2 days in /usr/local/bastille/backups/
- If specified with flags (i.e. "quimby safe 4") it can optinally safely stop/start jails (for UFS filesystems) and remove
  backup files according to the retention period provided in number of days

## TODO:
- Backup running jails only
- Do not start jails that were already stopped (current known bug)

## Installation

#### Git
```shell
git clone https://github.com/tofazzz/quimby.git
cd quimby
- on FreeBSD:
go build -o quimby

- on other OS:
env GOOS=freebsd GOARCH=amd64 go build -o quimby

Then move the executable where needed and setup crontab for execution:

0 1 * * * /usr/local/bin/quimby
```

## Sample Usages

```shell
quimby < safe | live > < days >

Options:

none: hot backup jails and remove backups older than 2 days
safe: it safely stop jails before backing them up. Required if using UFS filesystem
live: hot backup jails without stopping them
days: number of days of data retention

Examples:

root@server#quimby safe 9 - safely backup jails and remove backup files older than 9 days
root@server#quimby live 0 - hot backup jails and remove all backup files


Logs are stored under /var/log/quimby.log
```
