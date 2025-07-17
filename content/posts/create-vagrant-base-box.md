+++
title = "Creating a vagrant base box"
date = "2025-07-16T21:37:00-04:00"
author = "Andre"
authorTwitter = "" #do not include @
cover = ""
coverCaption = ""
tags = ["vagrant", "virtualbox"]
keywords = ["", ""]
description = ""
showFullContent = false
readingTime = true
hideComments = false
color = "" #color from the theme settings
+++

### Steps

- First install a linux system and upgrade it.
- Removing old kernels

```bash
dnf install dnf-plugins-core
```

- Set root password to vagrant
- Create a vagrant user

```
user: vagrant
password: vagrant
```

- Add the default insecure ssh public key to the vagrant user account

```bash
mkdir -p /home/vagrant/.ssh
wget -O /home/vagrant/.ssh/authorized_keys https://raw.githubusercontent.com/hashicorp/vagrant/refs/heads/main/keys/vagrant.pub.ed25519
chown -R vagrant:vagrant /home/vagrant/.ssh
chmod 0700 /home/vagrant/.ssh
chmod 600 /home/vagrant/.ssh/authorized_keys
```

- Change the %sudo entry to require no password

```bash
visudo
```

- Use vagrant package command to export the VM into a .box file

```bash
➜ vagrant package --base "Oracle 10 Minimal" --output $HOME/Vagrant/oraclelinux-10.box
==> Oracle 10 Minimal: Exporting VM...
==> Oracle 10 Minimal: Compressing package to: /home/andre/Vagrant/oraclelinux-10.box

~ took 1m51s
```

- Add the file created as a box

```bash
➜ vagrant box add Vagrant/oraclelinux-10.box --name oraclelinux/10
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'oraclelinux/10' (v0) for provider:
    box: Unpacking necessary files from: file:///home/andre/Vagrant/oraclelinux-10.box
==> box: Successfully added box 'oraclelinux/10' (v0) for ''!

➜ vagrant box list
almalinux/10-kitten (virtualbox, 10.20241227.0, (amd64))
oraclelinux/10      (virtualbox, 0)
oraclelinux/8-btrfs (virtualbox, 8.10.658)
oraclelinux/9-btrfs (virtualbox, 9.5.662)
```

- In the Vagrantfile set the variable with the box name, e.g.:

```
config.vm.box = oraclelinux/10
```
