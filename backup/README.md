# Backup of SD card

There are two obvious options to create a backup of an RPi SD card:

1. Use `dd` to make an identical image
2. Use `rsync` to make a traditional backup

## Using `dd`

Using `dd` is best when you have a completely separate partition that
you car write to, write speed is not a problem, and when you want to
make a copy that is a complete clone of the original image.

## Using `rsync`

Here is a bash script to do the backup. (My partition is
`PiData`, mounted on `/mnt/PiData`
```
#!/bin/bash
# script to synchronise Pi files to backup
BACKUP_MOUNTED=$(mount | awk '/PiData/ {print $6}' | grep "rw")
if [ $BACKUP_MOUNTED ]; then
    echo $BACKUP_MOUNTED
    echo "Commencing Backup"
    rsync -avH --delete-during --delete-excluded
    --exclude-from=/usr/bin/rsync-exclude.txt / /mnt/PiData/PiBackup/
else
    echo "Backup drive not available or not writable"
fi
```


to restore use
```
sudo rsync -avH /mnt/PiData/PiBackup/ /
```

The `rsync-exclude.txt` file shown below lists the files that are unnecessary to backup.

```
/proc/*
/sys/*
/dev/*
/boot/*
/tmp/*
/run/*
/mnt/*

.Trashes
._.Trashes
.fseventsd
.Spotlight-V100
.DS_Store
.AppleDesktop
.AppleDB
Network Trash Folder
Temporary Items

/etc/fake-hwclock.data
```
