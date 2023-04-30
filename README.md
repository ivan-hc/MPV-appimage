This repository provides the script to create and install the latest version from https://launchpad.net/~savoury1 and an AppImage ready to be used.

# Requirements
The only requirement is `ffmpeg` that must be installed on your system

# How to integrate MPV AppImage into the system
The easier way is to install "AM" on your PC, see [ivan-hc/AM-application-manager](https://github.com/ivan-hc/AM-application-manager) for more.

Alternatively, you can install it this way:

    wget https://raw.githubusercontent.com/ivan-hc/AM-Application-Manager/main/programs/x86_64/mpv
    chmod a+x ./mpv
    sudo ./mpv
The AppImage will be installed in /opt/mpv as `mpv`, near other files.
### Update

    /opt/mpv/AM-updater
### Uninstall

    sudo /opt/mpv/remove

------------------------------------
# About Rob Savoury's PPA 
SITE: https://launchpad.net/~savoury1

This is a collection of PPAs giving significant upgrades for the past 6+ years of Ubuntu (LTS) releases. Popular software here: Blender, Chromium, digiKam, FFmpeg, GIMP, GPG, Inkscape, LibreOffice, mpv, Scribus, Telegram, and VLC.

***"SavOS project 3 year milestones: 20,000 uploads and 20,000 users"***
               https://medium.com/@RobSavoury/bade09fa042e

Fun stats: Over 23,000 uploads since August 2019 of 5,100 unique packages!
(3 Nov 2022) With now 460+ unique packages published for 22.04 Jammy LTS, 1,730+ for 20.04 Focal LTS, and a whole lot extra for Xenial & Bionic LTS!

If software at this site is useful to you then please consider a donation:

***Donations: https://paypal.me/Savoury1 & https://ko-fi.com/Savoury1***

***Also https://patreon.com/Savoury1 & https://liberapay.com/Savoury1***

UPDATE (8 May 2022): See new https://twitter.com/RobSavoury for updates on the many Launchpad PPAs found here, ie. new packages built, bugfixes, etc.

***Bugs: File bug reports @ https://bugs.launchpad.net/SavOS/+filebug***

[ "SavOS" is the project heading for all packages at https://launchpad.net/~savoury1 ]
