---
layout: "post"
title: "Ubuntu Server Initial Setup"
categories: ubuntu server
---

Here is the song & dance I do every time I start a new Ubuntu server. Nice cheat sheet for me to refer back to.
<!--more-->

## Install Tailscale

``` bash
curl -fsSL https://tailscale.com/install.sh | sh
```

## Create non-root user & enable SSH

Create `ubuntu` user:
``` bash
adduser ubuntu
usermod -aG sudo ubuntu
```

Most hosts allow keys to be uploaded at time of vps creation. Copy the keys to the new user.

``` bash
rsync --archive --chown=ubuntu:ubuntu ~/.ssh /home/ubuntu
```
## Login as `ubuntu`

``` bash
ssh ubuntu@example
```

## Quiet the login message

``` bash
touch .hushlogin
```

## For Meteor Apps

``` bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt-get install -y nodejs jq build-essential
sudo npm install -g pm2
pm2 startup
```

## Install docker

``` bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce
sudo usermod -aG docker $USER
```
Log out and back in!