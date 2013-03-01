btrfsrotate
===========

This tools makes a snpahot of a btrfs partition and rotate those snapshots like 'logrotate' does.

= Setup =
Follow this to add it to your system:
```shell
git clone https://github.com/rd2b/btrfsrotate.git
cd btrfsrotate
ln -sf $(pwd)/btrfsrotate /usr/local/bin/btrfsrotate
```

= Run =

Rotate on 7 snapshots with the tag daily:
``` btrfsrotate  -b /data -c 7 -t 'daily'

The following detects every btrfs mounted partition and makes snapshots on them:
```shell
for btfs in `/bin/mount | sed -n 's/.* on \(.*\) type btrfs.*/\1/p'`
    do /usr/local/bin/btrfsrotate -b $btfs  -c 7 -t 'Daily'
done
```



