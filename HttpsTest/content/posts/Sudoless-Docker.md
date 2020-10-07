---
title = "Sudoless Docker"
date = "2020-10-06T16:30:11-04:00"
author = "Matt Higgins"
authorTwitter = "" #do not include @
cover = ""
tags = ["linux", "docker"]
keywords = ["linux", "docker", "configuration"]
description = "For personal reference, I can never remember how to configure Docker to run without sudo when I find myself at a new random dev box. Having this available for reference on my site has been helpful for me, and maybe it'll help somebody else."
showFullContent = false
draft = false
---

Using sudo with every docker command is incredibly annoying after a while. If you are configuring a development environment with something like docker-compose, you really won't want to keep writing a command, forgetting sudo, and then doing the command again. Thankfully, it is really easy to simply fix this problem. The instructions can be found on the docker documentation, aside from a small change for Arch.

1. Create the docker group: sudo groupadd docker
1. Add yourself to the group: sudo usermod -aG docker $USER (On arch: sudo gpasswd -a $USER docker)
1. Sign out and then back in. Alternatively, you may have to restart if group permissions aren't getting re-evaluated. (I had to do this on my laptop for some reason..)

Once this is done, using docker and docker-compose becomes much more seamless. It isn't a huge change functionally, but it makes development on docker feel significantly less cumbersome!