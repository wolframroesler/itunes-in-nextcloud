# How to back up your iTunes files to Nextcloud

This guide explains how to back up your iTunes files (music, videos, ring tones, e-books, iPhone backups, etc.) to your Nextcloud in macOS. It assumes that ...

* you are familiar with the Unix shell (aka Terminal)
* the Nextcloud client software is configured and running
* your Nextcloud files are in ~/Nextcloud

This guide was developed using:

* macOS Sierra
* iTunes 12.5.5.5
* Nextcloud 2.2.4

## How it works

To back up the iTunes files to Nextcloud we'll first move them into Nextcloud, and then we'll set up symbolic links to make them accessable from where iTunes expects them.

Note that we cannot do it the other way around (as we could with Dropbox), i. e. put a symbolic link in the Nextcloud folder that links to the iTunes files, since the Nextcloud client software won't follow symbolic links.

## Before you start

Do this at your own risk. Make sure you have a good backup in case you mess up something, or in case there's a bug in this document. Time Machine is your friend. Read and understand the shell command lines before executing them.

Make sure you have enough space available in your Nextcloud.

Please note that the following serves as a backup of your iTunes data. It is not suitable for sharing your iTunes files across several iTunes instances; trying to do so will most certainly mess things up badly.

## How to do it

First of all, shut down iTunes.

Make the directory where we will put the iTunes files. Name this whatever you want, but put it inside your ~/Nextcloud folder (optionally in a sub-directory):

````sh
$ cd ~/Nextcloud
$ mkdir iTunes
````

Now, move the folders where iTunes stores its files into that directory, and make symbolic links so iTunes still finds them:

```sh
$ cd "~/Music/iTunes/iTunes Media"
$ for i in Books Downloads "Home Videos" "Mobile Applications" Movies Music Tones "Voice Memos"; do
>   mv "$i" ~/Nextcloud/iTunes && ln -s "~/Nextcloud/iTunes/$i" .
> done
```

This will process your iTunes media files (videos, music, etc.) Leave out what you don't want in your Nextcloud (for example, if you don't want purchased movies in Nextcloud, remove Movies from the "for" line).

Your folder now looks something like this:

```
$ ls -l
total 32
drwxr-xr-x    4 wolfram  staff    136 21 Jan 22:42 Automatically Add to iTunes.localized
lrwxr-xr-x    1 wolfram  staff     44 20 Feb 18:28 Home Videos -> /Users/wolfram/Nextcloud/iTunes/Home Videos/
lrwxr-xr-x    1 wolfram  staff     38 19 Feb 15:00 Music -> /Users/wolfram/Nextcloud/iTunes/Music/
lrwxr-xr-x    1 wolfram  staff     38 19 Feb 14:23 Tones -> /Users/wolfram/Nextcloud/iTunes/Tones/
lrwxr-xr-x    1 wolfram  staff     44 19 Feb 14:27 Voice Memos -> /Users/wolfram/Nextcloud/iTunes/Voice Memos/
...
```

Then, do the same for the mobile backups, which are in a separate directory:

```sh
$ cd "~/Library/Application Support/MobileSync"
$ mv Backup && ~/Nextcloud/iTunes && ln -s ~/Nextcloud/iTunes/Backup/ .
```

So you get this:

```
$ ls -l
total 8
lrwxr-xr-x  1 wolfram  staff  39  2 Mär 19:41 Backup -> /Users/wolfram/Nextcloud/iTunes/Backup/
```

As soon as you move a directory, the Nextcloud software will start uploading it to your cloud server.

Finally, start iTunes again. Enjoy.