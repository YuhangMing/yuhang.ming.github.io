---
layout: post
title:  Linux command mix
date:   2022-03-16 12:40:16
description: cheat sheet.
tags: linux
categories: mix-posts
---

# Table of Content
- [Table of Content](#table-of-content)
- [Copy](#copy)
  - [`cp`](#cp)
  - [`scp`](#scp)
  - [`rsync`](#rsync)
- [Disk Partition](#disk-partition)
  - [`lsblk`](#lsblk)
  - [`mkfs`](#mkfs)
    - [File systems](#file-systems)
  - [`mount`](#mount)


# Copy

## `cp`
copy only files in the a directory dir1 to another directory dir2:
`cp dir1/* dir2/`

## `scp`
TL;DR: copy from local to remote, use `scp -r dir/ [USER@]HOST:dest/`.

SCP (secure copy) is a command-line utility that allows you to securely copy files and directories between two locations. It relies on `ssh` for data transfer.

Usage: `scp [OPTION] [USER@]SRC_HOST:SRC [USER@]DEST_HOST:DEST`

Common options:
- -P, Specifies the remote host ssh port.
- -p, Preserves files modification and access times.
- -q, Use this option if you want to suppress the progress meter and non-error messages.
- -C, This option forces scp to compresses the data as it is sent to the destination machine.
- -r, This option tells scp to copy directories recursively.

## `rsync`
TL;DR: copy dir to dest with progress bar, use `rsync -a --progress dir dest`.

`rsync` is a fast and versatile command-line utility for synchronizing files and directories between two locations over a remote shell, or from/to a remote Rsync daemon. It provides fast incremental file transfer by transferring only the differences between the source and the destination.

Rsync can be used for mirroring data, incremental backups, copying files between systems, and as a replacement for `scp` , `sftp` , and `cp` commands.

Usage:
```
Local to Local:  rsync [OPTION]... [SRC]... DEST
Local to Remote: rsync [OPTION]... [SRC]... [USER@]HOST:DEST
Remote to Local: rsync [OPTION]... [USER@]HOST:SRC... [DEST]
```

Common options (case sensitive):
- -a, --archive, archive mode, equivalent to -rlptgoD. This option tells rsync to syncs directories recursively, transfer special and block devices, preserve symbolic links, modification times, groups, ownership, and permissions.
- -r, --recursive, this tells rsync to copy directories recursively. See also --dirs (-d).
- -d, --dirs, transfer directories without recursing
- -z, --compress. This option forces rsync to compresses the data as it is sent to the destination machine. Use this option only if the connection to the remote machine is slow.
- -P, equivalent to --partial --progress. When this option is used, rsync shows a progress bar during the transfer and keeps the partially transferred files. It is useful when transferring large files over slow or unstable network connections.
- --delete. When this option is used, rsync deletes extraneous files from the destination location. It is useful for mirroring.
- -q, --quiet. Use this option if you want to suppress non-error messages.
- -e. This option allows you to choose a different remote shell. By default, rsync is configured to use ssh.

[Back to Top](#table-of-content)



# Disk Partition

## `lsblk`
Displays block devices, which are files that represent devices such as hard drives, RAM disks, USB drives, and CD/ROM drives. To further display the file systems, use `lsblk -f`.

## `mkfs`
Format any partition to desired file system type.

Usage: `mkfs [options] [-t type fs-options] device [size]`

For example, `sudo mkfs -t ext4 /dev/sdb1` to format it to ext4. Use `vfat` for FAT32 and `ntfs` for NTFS.

### File systems
| File system | Supported File Size | Compatibility | Ideal Usage |
| FAT32       | up to 4 GB          | Windows, Mac, Linux | For maximum compatibility |
| NTFS        | 16 EiB – 1 KB       | Windows, Mac (read-only), most Linux distributions | For internal drives and Windows system file |
| Ext4        | 16 GiB – 16 TiB     | Windows, Mac, Linux (requires extra drivers to access)  | For files larger than 4 GB |

## `mount`

To mount a device, run `mount -t type device dir`. Commonly `device` is in `\dev` and the preferred mount point would be in `\usr\media\`. If the type is unknown, you can run with `mount -t auto device dir` or just `mount device dir`

To unmount a device, run `umount device`.

[Back to Top](#table-of-content)
