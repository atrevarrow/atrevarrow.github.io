---
layout: post
title:  "Goodbye Windows Home Server"
tags: backup
---
If you care about your digital stuff, you should back it up - if you don't, you're living on borrowed time.
[Scott Hanselman](http://www.hanselman.com/blog/TheComputerBackupRuleOfThree.aspx) sums it up better than I could.
Until recently I've been using [Windows Home Server](https://en.wikipedia.org/wiki/Windows_Home_Server), but now I'm following a different path.
<!--more-->

# Windows Home Server - good
For homes like mine with multiple (Windows) computers, WHS had some great features:

- shared folders with user-level authorisation and multi-disk redundancy
- arbitrary storage expansion: you could remove and add disks of any capacity and it would redistribute files to ensure no data was lost if a disk failed
- all PCs just needed the connector software installed and they would then be automatically backed up every night to the server

And when it worked, it was great - I had at least a couple of occasions when a system disk failed in one of our PCs and it was just a case of booting into the recovery software and restoring from the latest backup on the server.

But the key phrase there is _when it worked_...

# Windows Home Server - not so good
All the backups of the client PCs went into a big, opaque database. This was fine, apart from the fact that WHS had a tendency to corrupt the database every now and then, for no apparent reason.
This was solved to some degree by a third party extension, [BDBB](http://www.mediasmartserver.net/wiki/index.php/WHS_BDBB), which allows you to create a backup of your backup database.
I did use BDBB now and then, but you still have the problem of potentially losing any client PC backups that happened since the last database backup.

But the worst thing is, WHS doesn't tell you that the database is corrupt. It merrily continues on its way, backing up all the client PCs **without any error!**
And so, of course, you don't realise until you need to restore a file from one of those backups that occured since the corruption (whenever that was) and it fails - the backup is there, you can browse the files within it, but you just can't get at them.

It's worth pointing out that WHS is no longer supported, so ordinarily you shouldn't expect Microsoft to fix this - except it's **always** been broken and it just never got addressed.

# Goodbye Windows Home Server, hello CrashPlan
So after being bitten by the corrupted database problem (not for the first time) I hunted around for an alternative backup strategy and found that CrashPlan seems to have mostly positive comments - and if you don't need the online component
you can use it for free to just backup between PCs (which is all I was really doing with WHS).
So I figured I could just setup CrashPlan on what was my WHS box and make it the destination for all the other computers to backup to.

Before doing anything I backed up all the shared folders from WHS - these are unaffected by the backup database corruption.
Obviously I couldn't do anything about the backups so all the home PCs were unprotected for a while... though realistically they hadn't had backups that I could actually restore for some time!

I nuked the old WHS box and started off with a barebones Windows 7 SP1 installation (I had an unused licence kicking around and I don't need anything fancier).
It has 3 disks in it - one 500GB (C:) and two 2TB (D: and E:) (nothing new here, that was my WHS setup) - so I figured I could just have all the data on one of the 2TB disks and mirror everything to the other (more below).

Once Windows had installed its required updates (all 120 of them!) I setup user accounts on the new "server" for everyone at home,
copied the shared folders back to the D: drive and shared them with the appropriate user permissions.

I then installed CrashPlan on my desktop PC (creating a new CrashPlan account in the process) and also on the server (with the same account).
On the server I changed its backup location to the D: drive...

![Crashplan server backup location](/images/crashplan_server_backup_location.png "Crashplan server backup location")

...and then on my desktop I added the server as a CrashPlan backup destination.
(Once I'd got everything working with this one PC I set all the others up the same.)

# Duplication's what you need
So now I have all the client PCs effectively backing up to _D:\Backups_ on the server, and all the various shared folders also on D:

```
D:\
  - Backups\
  - Shares\
    - Share1
    - Share2
  ...
```

But obviously if D: fails I'm screwed.
So to make sure I have duplication I created a simple robocopy script to mirror the entire D: drive (including the CrashPlan backups) to E:

```
robocopy D:\ E:\  /MIR /FFT /Z /W:5 >mirror.log
```

I could run this every night, but instead I only run it every 3 days - as it's a true mirror,
if anything gets accidentally deleted from one of the shares on D: that file is also removed from E:, so this way I have 3 days to spot it and recover the file!

# Potential improvements
Although a bit DIY, this seems to work very well so far.

One thing I still need to do is create regular external backups - however, this will be a simple variation on the mirror script above, but with an external drive as the destination.
This external drive can then be stored somewhere (on rotation) as an offsite backup.

And if I feel the need to go for cloud backups, I can just start paying for CrashPlan and turn that on.
