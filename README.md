# Automount USB drives with systemd

  * [Overview](#overview)
  * [Requirements](#requirements)
  * [Install](#install)
  * [Uninstall / removal](#uninstall---removal)

## Overview

Based on https://github.com/raamsri/automount-usb (you may also be interested in https://github.com/Ferk/udev-media-automount which saw some bug fixes in 2022)

On inserting an USB drive, automounts the drive at /media/ as a
directory named by device label; just the device name if label is
empty: /media/usbtest, /media/sdd

Can also call usb-mount.sh manually without the automatic udev/systemd rules.
Logs to stderr and system log (see logger notes below).

Tracks the list of mounted drives in `/var/log/usb-mount.track`

When device is unplugged (without umount), script will auto-unmount and remove the mount point directory under /media

Uses `logger` to log the actions in /var/log/messages (`/var/log/syslog`, if using logrotate check `/etc/logrotate.conf`, likely logs moved to `/var/log.hdd/`) with tag 'usb-mount.sh'

Please do not expect it to perfectly handle all your needs.
Be warned, minimally tested; okay for temporary plug-ins but certainly
not recommended for enclosures with longer TTL.

## Requirements

  * bash (dash might work, but currently expects bash to be installed)
  * logger
  * any file system driver/tools (e.g. ntfs-3g, exFAT, etc.)
  * udev
  * systemd

Debian based distributions (like Armbian) typically have all requirements already installed with the exception of ntfs-3g file system support/

    sudo apt-get install ntfs-3g exfat-fuse exfat-utils

## Install


To setup, run `CONFIGURE.sh` with sudo or as root:

    sudo ./CONFIGURE.sh

This will update/add (or create if not already present) `/etc/udev/rules.d/99-local.rules`

## Quick sanity checks

    sudo systemctl status|rg -i usb-mount
    sudo cat /var/log/syslog | rg -i usb-mount

## Uninstall / removal

`REMOVE.sh` to undo the setup:

    sudo ./REMOVE.sh

This will update `/etc/udev/rules.d/99-local.rules`

