# Plopr
Continuously sync your project's changes to your remote hosts.

### What is it? 
Plopr is a very simple tool. It watches your project for file & folder changes then efficiently syncs the change delta to your hosts. It's like Parse.com's CLI `parse deploy` in continuous mode.

### How do I use it? 

1. clone this repo!
2. symlink the binary
        cd /usr/local/bin
        ln -s /path/to/plopr/bin/plopr
3. Within your project create a `ploprfile.json` file. 
4. Configure Plopr for your project through `ploprfile.json` See the example in the repo. 
4. call `plopr` from within the desired level in your project
5. That's it! Your code changes will sync to your hosts!

**Options**

`--watch=0` = Turn off watch and just run plopr 1 time


**ploprfile.json**

See the ploprfile example for configuration options..

### What does this solve? 

Killer multi-server software development. With Plopr, you can write your code on your local machine and instantly have your changes be reflected on 3+ dev hosts. It makes testing distributed architecture a lot easier. 

Vagrant and docker are great for development but I wanted way to just write code on my local machine and have it update all of my physical test nodes instantly. 

**I wouldn't advise using this as a production deployment tool.** Just in the same way I would advise using SFTP. It's just a great way to test code on multiple nodes. 

### Other options

**Develop through Shell on the box!** - I'm not a VIM guy, I love my Sublime Text and I still don't know how developing off the node directly will sync those changes to other nodes. 

**SFTP** - Code resides on the server not my local machine, I don't want my git repo on the server, just my files necessary to run

**FUSE** - Same sort of thing as SFTP really. I have had issues with blocking on files saves and cludgy behavior

**Dropbox** - I tried using dropbox as a syncing tool. However, I ran into problems excluding files and folders from the sync. Dropbox has options to exclude files but I found them limiting. I ended up thinking about using rsync to transfer files to my Dropbox, then Plopr was born. 

### How does this work? 

It's really pretty simple actually. 

- rsync - does the heavy lifting of syncing changes incrementally to your hosts
- fswatch - `brew install fswatch`, watches file & folder changes in your project then (in batch) fires rsync. 
- Plopr - is the sugar that makes the configuration and implementation much easier. 