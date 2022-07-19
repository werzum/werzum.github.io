---
layout: post
title: "Mobile Sync for Obsidian"
date: 2022-02-13
categories: tech
author: Carl Orge Retzlaff
---

### Goal

Obsidian is a great markdown based note-taking app which can not be recommended enough - have a more detailed look at it [here](https://obsidian.md/).
Since Obsidian leaves it up to you (optionally, there is also a paid service which is very nice) how to sync your generated markdown files, using `git` is a popular approach.
The desktop-version of obsidian also has a great [git-plugin](https://github.com/denolehov/obsidian-git), which enables hassle-free backups and just works.
If you also want to have your knowledge base on mobile however, things become a little more complicated.
In this guide, I want to show you how to set up such a git-based, automatic workflow for syncing your Obsidian database on Android.
The idea is to use Termux on Android to manage your git repository and create a shortcut which triggers a complete pull-push action for quick syncing via one click.
The process is as follows:

### Steps

1. **Install termux, termux widget and (obviously) obsidian**

From [Github Termux](https://github.com/termux/termux-app), [Github Termux Widget](https://github.com/termux/termux-widget) download the APKs and install them. Obsidian can be found on the play store.

2. **Install git, open-ssh and clone repo**

Open termux on your phone and
`apt install git, open-ssh`
and
`git clone <your-github-repo>`

This will clone your knowledge base and install the required applications.
I recommend to also create a .gitignore with the following content:
```
.obsidian/workspace
.obsidian/app.json
```
in order to prevent your desktop /workspace configuration from messing up or changing your android config and vice versa.

3. **Create shortcut folders**

According to the termux widget instruction, create a directory for the shortcuts:
```
mkdir -p /data/data/com.termux/files/home/.shortcuts
chmod 700 -R /data/data/com.termux/files/home/.shortcuts

mkdir -p /data/data/com.termux/files/home/.shortcuts/tasks
chmod 700 -R /data/data/com.termux/files/home/.shortcuts/tasks
```
4. **Create the sync script**

Type  `nano /data/data/com.termux/files/home/.shortcuts/tasks/sync_cript.sh`  to enter the nano text editor and create the `sync_script.sh` . 
Then, add the following content:
```
#!/bin/bash  
cd /storage/emulated/0/repositories/Obsidian-Knowledge-Base  
git pull && git add * && git commit -a -m "commit from android" && git push
```
This script first goes to the folder where I have stored my obsidian data (change this if needed).
Then, it tries to pull new changes and will then add all your changes to Github. This step could probably be more refined, send me a message if you have comments or ideas on how to make this a little more resilient.

5. **Create SSH keys and add them to your Github account**

Now, you will want to authenticate to Github over SSH to allow the synchronization to happen without having to provide your password every time.
To do that, run the following (choose a password for your SSH key if you wish):
```
ssh-keygen -t ed25519
```
And then "print" the generated public key.
```
cat ~/.ssh/id_rsa.pub
```
Copy this key and add it over the Github website to your account (broadly under /Settings/Access/SSH Keys).

6. **Add widget with shortcut to home screen and enjoy**

Now, you just have to create a termux widget on your android main screen, and should be able to execute the `sync_script.sh`.

### Wrapup
Let me know if it worked, I hope this helped! I can only recommend to give Obsidian a try, it certainly helped me a lot during my studies and work.
Also, there are probably several smaller steps I missed in this write-up, let me know if you encountered anything major I missed.
