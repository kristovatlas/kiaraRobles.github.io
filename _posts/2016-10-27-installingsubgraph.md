---
layout: post
comments: true
title: Installing Subgraph OS From a Bootable USB on Mac OS X
tags: [OS, DevOps, Mac]
---

# Installing Subgraph OS From a Bootable USB on Mac OS X
*From Bootable USB on a Mac*

**Requires:** 

* GPG
* OpenSSL
* 2 GB USB
* Subgraph OS Capable Hardware

## Download and Verify Subgraph OS

All downloads are done over Tor:

1. Download the [Subgraph OS Alpha ISO](https://subgraphqov3womk.onion/sgos/alpha/subgraph-os-alpha_2016-06-16_2.iso)
2. Download the [Subgraph OS SHA256](https://subgraphqov3womk.onion/sgos/alpha/subgraph-os-alpha_2016-06-16_2.iso.sha256)
3. Download the [Subgraph OS SHA256 Signature](https://subgraphqov3womk.onion/sgos/alpha/subgraph-os-alpha_2016-06-16_2.iso.sha256.sig)
4. Verify the signature of the shasum file:

	```
	gpg --recv-key B55E70A95AC79474504C30D0DA11364B4760E444
	gpg --verify subgraph-os-alpha_2016-06-16_2.iso.sha256.sig subgraph-os-alpha_2016-06-16_2.iso.sha256
	```
	
	Signature looks good:
	
	```
	> gpg: Signature made Thu Jun 16 17:19:46 2016 PDT using RSA key ID F999D968
> gpg: Good signature from "Subgraph Release Signing Key <release@subgraph.com>" [unknown]
> gpg: WARNING: This key is not certified with a trusted signature!
> gpg: There is no indication that the signature belongs to the owner.
> Primary key fingerprint: B55E 70A9 5AC7 9474 504C  30D0 DA11 364B 4760 E444
> Subkey fingerprint: AB6C 7E34 4F63 3E10 4377  D595 E1AE 39C4 F999 D968
	```
	
	The download walkthrough on Subgraph ([here](https://subgraph.com/sgos/download/index.en.html)) will give you the linux command:
	
	```
	sha256sum -c subgraph-os-alpha_2016-06-16_2.iso.sha256
	```
	
	On mac the command is:
	
	```
	shasum -a 256 -c subgraph-os-alpha_2016-06-16_2.iso.sha256
	```
	
	If everything is good, this should print "OK"
	
	
## Create a Bootable USB

### *Gods of Linux forgive me for trying to use UNetbootin.*

I first tried it to create bootable Live USB drive for the Subgraph ISO. (More on using UNetbootin on Mac OS [here](https://www.ubuntu.com/download/desktop/create-a-usb-stick-on-mac-osx).) But I had a number of problems with this approach. The boot process got stuck in a "Will automaticly boot in 10 secs" loop and was frozen. Basically the comupter has no idea how to boot or power down. No good. 

Instead of UNetbootin we'll use the dd command.

### Burn an ISO from the Command Line with dd

In case I have to mention it, you should probs insert the USB key at this point... 

Erase and Format with default setting in Disk Utility GUI.

*I tried to do this in command line but apparently the proper way is in the GUI*

Find the newly formatted disk from the command line:

```
> diskutil list
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *62.1 GB    disk2
   1:                        EFI EFI                     209.7 MB   disk2s1
   2:                  Apple_HFS Subgraph                61.7 GB    disk2s2
```
Unmount the disk.

```  
> diskutil unmountDisk /dev/disk2s1
Unmount of all volumes on disk2 was successful
```

Next we use the dd command to copy the ISO over.

```
dd if=/Users/username/Downloads/subgraph-os-alpha_2016-06-16_2.iso of=/dev/disk2
```
On the command line we specify the Input File using if= and the Output File using of= and dd will copy the data from input to output.

This process can take a few minutes, so you can press `Ctrl + t` to view the process.

When the process is complete you may get a pop up telling you the disk is not longer readable by the computer.

> The disk you inserted was not readable by this computer.

Eject the USB and insert it into the computer where you'll being installing subgraph.


## Install Subgraph OS from Bootable USB

The system requirements for the OS are detailed on the Subgraph [download page](https://subgraph.com/sgos/download/index.en.html), but I'll repeat them here in case you're anything like me and totally skipped over reading that section and went around googling for it instead. 

> Anything that can comfortably run GNOME 3:
>
> 64-bit machine (Core2Duo or over)
> 
> 2GB of RAM (4GB recommended)
> 
> At least 20GB of hard disk space

I'm running this on a Toshiba Chrome book. 

When you power on the machine and enter BIOS, choose to BOOT from USB then enter the terminal.

### Mount the USB to the CDROM

Enter the terminal with `ctrl + alt + F2` 

We can look through the list of drives with the `blkid` command to see what the UUIS of out drive is.

```
> blkid
> ...
> /dev/sda1: UUID="2016-06016-22-37-30-00" LABEL="Subgraph live 06162016" TYPE="iso9660" PTUUID="52a23f4a" PTTYPE="dos"
```

Mount the USB to the CD Drive:

```
> umount /cdrom
> mount /dev/sda1 /cdrom
```

Exit out of terminal with `ctrl + alt + F1`. And now you can continue on with the install process as normal. 

yay.
