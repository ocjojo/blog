---
title: 'MariaDB: Out of memory error'
date: 2017-03-29 18:27:01
tags: [mariadb, sysadmin]
---

## TL;DR
For low memory VPS set `performance_schema = off` and make sure you have a swapfile!

## Story
I recently had an issue with my MariaDB/MySQL Database doing random shutdowns.

```
[PID] [time] InnoDB: Fatal error: cannot allocate memory for the buffer pool
[PID] [time] [ERROR] Plugin 'InnoDB' init function returned error.
[PID] [time] [ERROR] Plugin 'InnoDB' registration as a STORAGE ENGINE failed.
[PID] [time] [ERROR] mysqld: Out of memory (Needed 128917504 bytes)
[PID] [time] [Note] Plugin 'FEEDBACK' is disabled.
[PID] [time] [ERROR] Unknown/unsupported storage engine: InnoDB
[PID] [time] [ERROR] Aborting
```

Doing a google search took quite some time - it seems this is a quite uncommon problem - and yielded [a result][1] from the mariadb blog about starting mysql on low memory virtual machines.

## Performance Schema

Setting `performance_schema = off` in the `[mysqld]`-Section (for CentOS 7 in `/etc/my.cnf.d/server.cnf`), as suggested, allowed me to start up the mariadb daemon again.

Unfortunately this did not last long. And the next time the daemon did not start up despite the new setting.

## Swap File

As it turns out I forgot to set a swapfile. Without it the OS may randomly shut down processes, if the memory runs out.

Creating a swapfile allows the OS to swap the memory to the harddrive instead of killing processes.

Keep in mind: This decreases the overall performance!

For me these were the needed commands
```bash
sudo fallocate -l 512M /swapfile # create file, check if size fits for your system
sudo chmod 600 /swapfile # more secure access rights
sudo mkswap /swapfile # make swap file ?
sudo swapon /swapfile # enable file
```

Finally in `/etc/fstab` add the entry
`/swapfile   swap    swap    sw  0   0`
to enable swap on restart.

[1]: https://mariadb.com/resources/blog/starting-mysql-low-memory-virtual-machines