---
layout: post
title:  "Installing Windows 7 the hard way"
date:   2013-05-21 12:09:35 +1100
categories: microsoft windows
---
Have you ever found yourself wanting to reinstall the factory OEM Windows image back onto a fresh hard drive, but the "recovery media" just doesn't work and all you have to go on is some .zip or .wim files you manually copied off a hidden partition?

This post gives the commands required to partition a drive, install a WIM (Windows Image) file and make it bootable, from the command line.

This post is unlikely to be of interest to many, but it was really hard for me to find this information anywhere else.

Steps 1-5 are run on a separate machine as preparation. Steps 6-10 are run on the machine where you want to perform the install.

### Step 1: Download a WinPE  ISO.

WinPE is a minimal, portable version of Windows, that just boots up enough to give you a command line. You can find various versions of this tool online if you search around.

### Step 2: Download an ISO-to-bootable-USB tool

There are loads of these; the official Microsoft one is probably a good one to start with, but other worked fine for me too.

### Step 3: Download imagex.exe

This is part of a Microsoft toolkit, but rather than grabbing the entire thing, you can find a copy of just imagex.exe if you search around.

### Step 4: Install WinPE ISO to a USB stick

### Step 5: Copy imagex.exe and your .wim file to the USB stick

### Step 6: Boot the stick up and get to a cmd prompt

### Step 7: Partition drive

Run these commands to partition the disk into two partitions, one of which is the reserved boot partition (100MB).

```
diskpart
select disk 0
clean
create partition primary size=100
select partition 1
format fs=ntfs label="System Reserved"
assign letter=S
active
create partition primary
select partition 2
format fs=ntfs label="Windows"
assign letter=C
exit
```

### Step 8: Install WIM file

This installs your chosen .wim file to the C: drive. You'll probably need to switch drives until you find the letter that applies to the USB stick, because by default you're inside the WinPE  boot drive.

`imagex /apply /verify Cdrivebackup.wim 1 c:\`

Note that if you have a serial of install.swm, install2.swm etc files, then you'll need a slightly different command:

`imagex /apply /verify install.swm /ref install*.swm 1 c:\`


### Step 9: Setup bootloader

This makes the 100M partition a valid bootloader.

Note that this assumes you assigned the letter S to the 100M partition, above, and that your Windows install ended up in the standard location of c:\windows

Otherwise, change letters and paths as appropriate.

`bcdboot c:\windows /s s:`

### Step 10: Reboot
