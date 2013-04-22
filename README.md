btrfsrotate
===========

This tools makes a snpahot of a btrfs partition and rotate those snapshots like 'logrotate' does.

Setup
-----

Follow this to add it to your system:
```shell
git clone https://github.com/rd2b/btrfsrotate.git
cd btrfsrotate
ln -sf $(pwd)/btrfsrotate /usr/local/bin/btrfsrotate
```

Run
----

Rotate on 7 snapshots with the tag daily:
```shell
btrfsrotate  -b /data -c 7 -t 'daily'
````

The following detects every btrfs mounted partition and makes snapshots on them:
```shell
for btfs in `/bin/mount | sed -n 's/.* on \(.*\) type btrfs.*/\1/p'`
    do /usr/local/bin/btrfsrotate -b $btfs  -c 7 -t 'Daily'
done
```

What's next ?
-------------

Make it a cron job:
```shell
SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

30 *  * * *             root    for btfs in `/bin/mount | sed -n 's/.* on \(.*\) type btrfs.*/\1/p'`; do /usr/local/bin/btrfsrotate -q -b $btfs  -c 24 -t "hourly" > /dev/null ; done
22 16 * * *             root    for btfs in `/bin/mount | sed -n 's/.* on \(.*\) type btrfs.*/\1/p'`; do /usr/local/bin/btrfsrotate -q -b $btfs  -c 30 -t "daily"  > /dev/null ; done
00 13 * * 1             root    for btfs in `/bin/mount | sed -n 's/.* on \(.*\) type btrfs.*/\1/p'`; do /usr/local/bin/btrfsrotate -q -b $btfs  -c 10 -t "Weekly" > /dev/null ; done
00 13 1 * *             root    for btfs in `/bin/mount | sed -n 's/.* on \(.*\) type btrfs.*/\1/p'`; do /usr/local/bin/btrfsrotate -q -b $btfs  -c 12  -t "Monthly" > /dev/null ;   done
``

List your snapshots:
```shell
$ sudo ls /home/myuser/.snapshots/
myuser-daily-1
myuser-daily-2
myuser-daily-3
...
````

