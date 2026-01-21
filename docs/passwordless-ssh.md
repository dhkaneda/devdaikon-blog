---
slug: passwordless-ssh
title:  "Passwordless SSH Access"
tags: [ ssh ]
sidebar_position: 3
---

Passwordless SSH makes it easy to automate tasks and run tools like Ansible without being prompted for passwords. This guide walks you through generating SSH keys, distributing them, and setting up your environment for seamless, secure access.

### Requirements

* A local Linux/macOS system (your control device)
* Remote machines accessible via SSH
* OpenSSH installed on all machines

Before beginning, you will first need to be able to SSH to your target machines WITH a password. This might be done with a user on that machine you have a password for and network access with a hostname.

For example:

```bash
ssh user@hostname
```

This assumes, that the target machine is on the local network and has the OpenSSH daemon running.

### 🗝️ Generate an SSH key (if you haven't already)

```bash
ssh-keygen -t ed25519 -f ~/.ssh/homelab
```

> Hit Enter when prompted for a passphrase (or add one for security).

### Copy your public key to each remote host

```bash
ssh-copy-id -i ~/.ssh/homelab.pub username@10.0.0.2
ssh-copy-id -i ~/.ssh/homelab.pub username@10.0.0.3
ssh-copy-id -i ~/.ssh/homelab.pub username@10.0.0.4
```

> Replace `user@IP` with the correct user and IP of each node.

### Configure your SSH client

Create or edit `~/.ssh/config`:

```bash
vim ~/.ssh/config
```

Paste the following:

```ssh
AddKeysToAgent yes

Host github.com
  IdentityFile ~/.ssh/github
  UseKeychain yes  # macOS

Host node-1
  HostName 10.0.0.2
  User username
  IdentityFile ~/.ssh/homelab
  Port 22

Host node-2
  HostName 10.0.0.3
  User username
  IdentityFile ~/.ssh/homelab
  Port 22

Host node-3
  HostName 10.0.0.4
  User username
  IdentityFile ~/.ssh/homelab
  Port 22
```

> Now you can run `ssh node-1` instead of `ssh username@10.0.0.2`.

### Test it out

Restart your terminal and try logging into one of your remote hosts:

```bash
ssh node-1
```

You should now be able to log in without entering a password or passphrase.
