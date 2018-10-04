# Fixing SD vcard after lost power


I was able to fix the issue using these steps: 

1. Modify: `/etc/fstab`

        #/dev/mmcblk0p2  / ext4 errors=remount-ro 0 1
        /dev/mmcblk0p2  /  ext4 ro 0 1
 This should mount the `rootfs` as `read-only` on the next reboot.
2. Reboot system using: `shutdown -rF now`
3. Run `fsck -fy` if the system hasn't already
5. Remount the `rootfs` as `read-write` using:
      
        mount -o remount,rw /dev/mmcblk0p2

 We need to do this in-order to complete step 4.
4. **(IMPORTANT)** Change `/etc/fstab` back to normal, thus:

        /dev/mmcblk0p2  / ext4 errors=remount-ro 0 1
        #/dev/mmcblk0p2  /  ext4 ro 0 1

 If `/etc/fstab` doesn't get changed back to normal, then the system will always mount the `rootfs` as `read-only`.
5. Profit.
