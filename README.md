# How to back up your iTunes files to Nextcloud

This guide explains how to back up your iTunes files (music, videos, ring tones, e-books, iPhone backups, etc.) to your Nextcloud in macOS. It assumes that ...

* you are familiar with the Unix shell (aka Terminal)
* the Nextcloud client software is configured and running
* your Nextcloud files are in ~/Nextcloud

This guide was developed using:

* macOS Sierra
* iTunes 12.5.5.5
* Nextcloud 2.2.4

## Before you start

Do this at your own risk. Make sure you have a good backup in case you mess up something, or in case there's a bug in this document. Time Machine is your friend. Read and understand the shell command lines before executing them.

Make sure you have enough space available in your Nextcloud.

Please note that the following serves as a backup of your iTunes data. It is not suitable for sharing your iTunes files across several iTunes instances; trying to do so will most certainly mess things up badly.

Also, please note that the following is for backup, not for sharing. You will not be able to use your iTunes media on two different Macs, for example.

## How it does not work

The first idea is to create symlinks to the iTunes directories from your cloud-synced folder, for example:

```sh
$ cd ~/Dropbox
$ mkdir iTunes
$ cd iTunes
$ for i in Books Downloads "Home Videos" "Mobile Applications" Movies Music Tones "Voice Memos"; do
>   ln -s "~/Music/iTunes/iTunes Media/$i" .
> done
```

This will work fine with Dropbox (as shown above) but not with Nextcloud because the Nextcloud client software doesn't follow symlinks.

The second idea is to to it the other way around, move your iTunes folders into a cloud-synced directory and set up symlink to make iTunes find them:

```sh
$ cd ~/Nextcloud
$ mkdir iTunes
$ cd "~/Music/iTunes/iTunes Media"
$ for i in Books Downloads "Home Videos" "Mobile Applications" Movies Music Tones "Voice Memos"; do
>   mv "$i" ~/Nextcloud/iTunes && ln -s "~/Nextcloud/iTunes/$i" .
> done
```

This will back up your iTunes files to Nextcloud all right, but will break stuff in iTunes. For example, you'll still be able to play music, but dragging mp3 files into iTunes will fails with an error message. Bummer.

## Mobile Device Backups

The symlink-into-Nextcloud works fine with mobile backups, however:

```sh
$ cd "~/Library/Application Support/MobileSync"
$ mv Backup && ~/Nextcloud/iTunes && ln -s ~/Nextcloud/iTunes/Backup/ .
```

iTunes doesn't mind having a symlink here, so iPhone/iPad/iPod backups go into the cloud just fine.

## How it works

Still working it out. Please come back later.

*Wolfram Rösler • wolfram@roesler-ac.de • https://twitter.com/wolframroesler • https://github.com/wolframroesler*
