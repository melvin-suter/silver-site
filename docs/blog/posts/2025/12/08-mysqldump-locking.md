---
title: mysqldump is locking the db
date: 2025-12-08
draft: false
tags:
- linux
- mysql
---

If your mysqldump command is locking the DB and you can’t edit the command (because another application is starting it), just add this to your `/etc/my.cnf`:

```
[mysqldump]
single-transaction
quick
skip-lock-tables
```