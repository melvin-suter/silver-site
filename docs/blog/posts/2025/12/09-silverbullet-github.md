---
title: Silverbullet to GitHub
date: 2025-12-09
draft: false
tags:
- git
- silverbullet
---

This website is hosted on GitHub Pages, and the content is directly synced from my Silverbullet instance, to GitHub. I’ve set it up this way:

## Setup

To set this up, you’ll have to create a ssh key-pair, and put it into your space somewhere. I’ve put it into a folder `Z Public/.keys` inside my instance. Then add that public key to your GitHub account, or to your repository, so it can be used to sync the content.

I also added a `Z Public/.gitignore` to exclude the key, like this:

```.gitignore
.keys/*
```

Then set this folder up as a new git repo, and add the origin to it.

Now you need to create a github workflow and a config to make it work with mkdocs. Both can be found in the [GitHub Repo](http://github.com/melvin-suter/silver-site/).

## Sync Command

After that, create a new page in your Silverbullet (for me it lives in `meta/command-git-sync-github`) and put this script in it:

```lua
command.define{
  name = "Public-Site: Sync",
  run = function()

    local function gitRun(args)
      local ok = shell.run("bash", {"-c" ,args .. " >> /tmp/git-log 2>&1"})
      return ok
    end

    gitRun("GIT_SSH_COMMAND=\"ssh -i '/space/Z Public/.keys/id_rsa' -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null\" git -C '/space/Z Public' pull --no-commit --rebase origin master")
    
    local ok1 = gitRun("git -C '/space/Z Public' add . --all")
    local ok2 = gitRun("git -C '/space/Z Public' commit -m auto")
    local ok3 = gitRun("GIT_SSH_COMMAND=\"ssh -i '/space/Z Public/.keys/id_rsa' -o IdentitiesOnly=yes -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null\" git -C '/space/Z Public' push origin master")
      
    if ok1 and ok2 and ok3 then
      editor.flashNotification("synced")
    else
      editor.flashNotification("sync did not work")
    end
  end
}
```