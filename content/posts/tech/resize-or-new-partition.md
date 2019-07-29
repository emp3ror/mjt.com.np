+++
title = "Resize or new partition for /var"
date = "2019-07-28T15:07:51Z"
tags = ["linux","shell", "partition"]
draft = false
author = "admin"
enableEmoji = true
+++

### why?

Thought `/var` wouldnt need more than 5G of space, so I foolishly created partition of only 5G and set `/var` to it. You know when you got new `ssd` with only 256G space, you try to act cool and think you can optimize but the thing is I am too lazy to optimize everytime :sweat_smile: :sweat_smile: I checked my old hard disk `the HDD` and found out, `/var` was given `21G` space and it was `15G` filled already... Then my adrenaline rush of need to resize came strong and I am changing this state while writting this blog :sweat_smile: , if something goes wrong, I would be needing bootable pendrive <span class="emojis"> :sweat_smile: :sweat_smile: </span>

### /var 

`/var` is a standard subdirectory of the root directory in Linux and other Unix-like operating systems that contains files to which the system writes data during the course of its operation.
    - source [http://www.linfo.org/var.html](http://www.linfo.org/var.html)


### Lets resize

Steps 

    // list available partitions
    $ df -h 

Now lets delete the increase the partition, we will use `fdisk` to pseudo-delete the partition without writing it to disk then increase the partition size and write it to the disk.

    $ sudo fdisk /dev/nvme0n1

    // press `p` to select partition, `d` to delete it and `n` to create new partition with new size

Oh NOOOOOOOO moment

![Medium image](/img/screenshot-partition.png)

Partition I am tring to remove is `/nvme0n1p6` and `/nvm0n1p9` has the free space I am trying to extend to, Since `sector Ending` of p6 partition was not adjacent to `sector begining` of p9 partition, it does not allow to resize partition p6 to grow more than `5G` space :expressionless:

### Fallback

I moved every thing of `/var` to new partition and rewrite `/etc/fstab`

    // mount the partition to /mnt
    $ sudo mount /dev/nvme0np19 /mnt
    
    // copy every thing to /mnt => new partition p9
    $ cp -apx /var/* /mnt

    // check uuid of partition
    $ ls -l /dev/disk/by-uuid/

    //  modify /etc/fstab
    $ sudo leafpad /etc/fstab

Here is screenshot of my modified `/etc/fstab`
![Medium image](/img/screenshot-fstab.png)

lets reboot and see if it works, if it does not work, I will have to uncomment uuid of previous partition on `fstab` with live usb boot <span class="emojis"> :sweat_smile: :sweat_smile: </span>

    $ sudo reboot

voila!! It works and smooth

### Next

I have given root `/` only 10G and my previous `the HDD` had 50G of space and 21G was filled, So it needs change some modification too. I will write about it in new post

<div class="alert alert-warning">
Thanks for reading! if you have better suggestion or just Hi in comment <span class="emojis"> :sweat_smile: :sweat_smile: </span>
</div>
