---
title: move-docker-root
date: 2025-12-24
draft: true
tags:
- linux
- docker
---

Moving a docker root is not hard, but comes with 1 or 2 gotchas. In my case, I wanted to attach a disk to the docker root placed in `/srv/docker`, so I had to move the data out of the way, mount the disk, and than move it back.

**Gotchas**:
- Just running `systemctl stop docker` won’t actually stop docker
- File permissions for overlay2 is VERY important (you will run into permission denied on /tmp or similar)
- Finding the right commands :)

**How To**:
```bash
# Getting Docker to shut up
systemctl disable --now docker.service
systemctl disable --now docker.socket

# Getting the data out of the way
mv /srv/docker /root/docker

# Now do what you must

# Lets get the data back
cp -R -a /root/docker/* /srv/docker/

# Start docker and check that all is good
systemctl enable --now docker.socket
systemctl enable --now docker.service

# Now the fun part, somehow overlay2 mounts the folder from your moved files. fun right? just umount those
umount /root/docker/overlay2/*/merged

# If so, cleanup after yourself
rm -rf /root/docker
```