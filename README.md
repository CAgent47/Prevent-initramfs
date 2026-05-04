# Prevent-initramfs

---
# Introduction

Many Linux users, especially on Ubuntu, faced this problem.

**Now let's define initramfs and see what it is and why it happens:**
initramfs is a small, temporary system that Linux runs before it mounts your main hard drive, 
and it is probably stuck on the screen due to the following problems:
1. Partition Problem
2. The file system has a problem
3. The latest kernel update messed something up.

---
**The good news is that this problem can be solved with a few simple commands. Below we will provide you with a solution that will reduce the likelihood of encountering this problem again by 95%.**
---
# 1. Identifying the root driver

Well, first of all, we need to find out which of the listed partitions is the main operating system partition.

```bash
blkid
```

This command will show the partitions, file system type, and UUID.
If there are multiple partitions, look for the larger size or the one with TYPE="ext4" written on it. This is called the root partition.
---
# 2. Check And Repair File System

Now we see that, assuming your root partition is /dev/sda5 **(enter the command carefully)**

```bash
fsck -y /dev/sda5
```
---
# 3. try to exit

```bash
exit
```
---
# 4. If he doesn't come back

This means that the root partition is not mounted correctly and we have to mount it manually.

```bash
mkdir /mnt/root
mount /dev/sda5 /mnt/root
```

Now that you have installed the system, you need to revert the last changes.

```bash
chroot /mnt/root
update-initramfs -u -k all
exit
reboot
```
---
**This came from this system, but we need to do something to avoid this problem again.**

```bash
sudo nano /etc/fstab
```

**The last line related to Root should be written :**
*errors=remount-ro*

```bash
sudo update-initramfs -u
```

**Clean the partition first, remove extra files.**

```bash
sudo journ --vacuum-time=3d
```

if use APT:

```bash
sudo apt clean
```

**check again partition**

```bash
sudo touch /fourcefsck
```

**Restart the system and the system will check the hard drive during boot and the file will be deleted after loading.**
# Good Lock

---

