---
title: How To Use Multiple Git Accounts in Terminal
date: 2024-07-10 12:00:00 -500
categories: [Tutorials]
tags: [alperenkocyigit,howto]
---

# How To Use Multiple Git Accounts in Terminal

## 1.Generate a SSH Key
```bash
ssh-keygen -t rsa -C "github-email" -f "github-username"
```

## 2.Copy the SSH Key and add to github account
Next, log in to your second GitHub account, click on the drop-down next to the profile picture at the top right, select Settings, and click on SSH and GPG keys.

Next, add the key you created earlier. Feel free to give it any title you wish.

## Add SSH Key to the Agent
```bash
ssh-add ~/.ssh/<username>
```

## Edit the config file
Edit config file with `sudo nano ~/.ssh/config`

Add following content

Host github-<username>
  HostName github.com
  User git
  IdentityFile ~/.ssh/<username>

# Usage

Use
```bash
git@github-username
```

Instead of 
```bash
git@github.com
```
for second account

Let's change the remote origin with second users repo.

```bash
git remote add origin git@github-username:alperen/Test.git
```