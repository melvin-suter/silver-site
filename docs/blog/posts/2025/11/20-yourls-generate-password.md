---
title: Yourls – Generate Password
date: 2025-11-20
tags:
- php
- linux
---

```
php -r "echo 'phppass:'. password_hash('yourpassword', PASSWORD_BCRYPT) . PHP_EOL;"
```