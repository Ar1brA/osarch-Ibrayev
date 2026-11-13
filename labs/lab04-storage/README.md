# Lab 4 — Partition, mirror, fail, recover

Pair: Arman · Date:13 November 2026 · Hypervisor: VirtualBox

## Windows

Commands:

```powershell
Get-Disk
Initialize-Disk -Number 1 -PartitionStyle GPT
New-Partition -DiskNumber 1 -UseMaximumSize -AssignDriveLetter |
    Format-Volume -FileSystem NTFS -NewFileSystemLabel LAB04
Get-Disk | Select Number, FriendlyName, PartitionStyle, Size
Get-Volume | Where FileSystemLabel -eq 'LAB04'
```

The new disk was disk 1 (1.04 GB, Partition Style RAW before initialising); disk 0 is the 80 GB system disk and was not touched.

Output of the last two commands:

```
PS C:\Users\arman> Get-Disk | Select Number, FriendlyName, PartitionStyle, Size

Number FriendlyName  PartitionStyle        Size
------ ------------  --------------        ----
     0 VBOX HARDDISK GPT            85899345920
     1 VBOX HARDDISK GPT             1119191040

PS C:\Users\arman> Get-Volume | Where FileSystemLabel -eq 'LAB04'

DriveLetter FriendlyName FileSystemType DriveType HealthStatus OperationalStatus SizeRemaining    Size
----------- ------------ -------------- --------- ------------ ----------------- -------------    ----
E           LAB04        NTFS           Fixed     Healthy      OK                      1.01 GB 1.03 GB
```

MBR or GPT: I chose GPT, because Windows 11 boots with UEFI, which uses GPT; GPT also keeps a backup copy of the partition table and checksums it, while MBR is limited to 2 TiB and four primary partitions.

## Linux — layout before and after

### Before

```
$ lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
NAME   SIZE TYPE FSTYPE   MOUNTPOINTS
loop0  1.5M loop squashfs /snap/hwctl/123
loop1  1.5M loop squashfs /snap/hwctl/131
loop2 66.8M loop squashfs /snap/core24/1643
loop3 50.1M loop squashfs /snap/snapd/27710
sda     25G disk
├─sda1   1M part
└─sda2  25G part ext4     /
sdb      1G disk
sdc     25G disk
sdd    1.1G disk
sr0   1024M rom
```

After

`md0` is built on `sdb1` and `sdd1` and mounted on `/mnt/mirror`.

```
$ lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS
NAME   SIZE TYPE  FSTYPE            MOUNTPOINTS
loop0  1.5M loop  squashfs          /snap/hwctl/123
loop1 66.8M loop  squashfs          /snap/core24/1643
loop2  1.5M loop  squashfs          /snap/hwctl/131
loop3 50.1M loop  squashfs          /snap/snapd/27710
sda     25G disk
├─sda1   1M part
└─sda2  25G part  ext4              /
sdb      1G disk
└─sdb1   1G part  linux_raid_member
  └─md0  1G raid1 ext4              /mnt/mirror
sdc     25G disk
└─sdc1  25G part  linux_raid_member
sdd    1.1G disk
└─sdd1  1.1G part linux_raid_member
  └─md0  1G raid1 ext4              /mnt/mirror
sr0   1024M rom
```

## What I did

Disk names are from my VM: system disk `sda`, new disks `sdb`, `sdc`, `sdd`.

```bash
mkdir -p ~/lab04/evidence && cd ~/lab04
{ echo '$ lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS'; lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINTS; } > evidence/lsblk-before.txt

# partition the three disks (GPT, one partition, raid flag)
for d in sdb sdc sdd; do
  sudo parted -s /dev/$d mklabel gpt mkpart primary 1MiB 100% set 1 raid on
done
lsblk

# create the RAID 1 mirror from two partitions (answered y to bitmap and "Continue creating array?")
sudo mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb1 /dev/sdc1
cat /proc/mdstat
{ echo '$ cat /proc/mdstat'; cat /proc/mdstat; echo; echo '$  sudo mdadm --detail /dev/md0'; sudo mdadm --detail /dev/md0; } > evidence/array-healthy.txt

# file system, mount, proof file, checksum before the failure
sudo mkfs.ext4 -L mirror /dev/md0
sudo mkdir -p /mnt/mirror
sudo mount /dev/md0 /mnt/mirror
sudo chown $USER: /mnt/mirror
echo "Lab 4 proof file - Arman - $(date)" > /mnt/mirror/proof.txt
head -c 20M /dev/urandom > /mnt/mirror/random.bin
cd /mnt/mirror && { echo '$ sha256sum proof.txt random.bin'; sha256sum proof.txt random.bin; } > ~/lab04/evidence/checksum-before.txt; cd ~/lab04

# fail one disk on purpose
sudo mdadm /dev/md0 --fail /dev/sdc1
sudo mdadm /dev/md0 --remove /dev/sdc1
{ echo '$ cat /proc/mdstat'; cat /proc/mdstat; echo; echo '$ sudo mdadm --detail /dev/md0'; sudo mdadm --detail /dev/md0; } > evidence/array-degraded.txt
cat /proc/mdstat

# prove the data survived
cat /mnt/mirror/proof.txt
cd /mnt/mirror && { echo '$ sha256sum proof.txt random.bin'; sha256sum proof.txt random.bin; } > ~/lab04/evidence/checksum-after-failure.txt; cd ~/lab04
diff <(tail -n +2 evidence/checksum-before.txt) <(tail -n +2 evidence/checksum-after-failure.txt) && echo "checksums match"

# add the third disk and rebuild
sudo mdadm /dev/md0 --add /dev/sdd1
cat /proc/mdstat
{ echo '$ cat /proc/mdstat'; cat /proc/mdstat; echo; echo '$ sudo mdadm --detail /dev/md0'; sudo mdadm --detail /dev/md0; } > evidence/array-rebuilt.txt

How I failed the disk: with `mdadm --fail /dev/sdc1` followed by `--remove` (software failure; I did not detach the virtual disk).

## Three states

| State | What /proc/mdstat showed | File |
| --- | --- | --- |
| Healthy | `md0 : active raid1 sdb1[0] sdc1[1]` — `[2/2] [UU]` | evidence/array-healthy.txt |
| Degraded | `md0 : active raid1 sdb1[0]` — `[2/1] [U_]` | evidence/array-degraded.txt |
| Rebuilt | `md0 : active raid1 sdd1[2] sdb1[0]` — `[2/2] [UU]`, sdd1 replaced sdc1 | evidence/array-rebuilt.txt |

Checksums before and after the failure: match (`diff` printed nothing and the shell printed `checksums match`).

## RAID is not a backup

Your colleague deleted the wrong file from the mirror an hour ago. Does RAID 1 help? Why?

No, RAID 1 does not help here. It writes every change to both disks at the same moment, so the deletion happens on both copies and the file is gone from the whole mirror. RAID only protects against a disk failing, not against mistakes, malware or corrupted files, so for this you need a separate backup or snapshots.

## What did not work the first time

1. **Typo in the path.** After computing the checksums after the failure I got `-bash: cd: /home/arman/lav04: No such file or directory`. I had typed `lav04` instead of `lab04` at the end of the command. The checksum file itself was still written (the `cd` is the last part of the line), so I only had to run `cd ~/lab04` and continue. In the same first attempt I had also typed `sha256sum proof.txt random.txt random.bin` by mistake (a file `random.txt` does not exist), so I re-ran the whole command with the correct file names before comparing with `diff`.
2. **A 25 GB disk in the mirror.** `lsblk` showed `sdc` as 25G instead of 1G (the other two were 1G and 1.1G). When creating the array mdadm warned `largest drive (/dev/sdc1) exceeds size (1089536K) by more than 1%`. The array still worked, but its size is limited by the smallest member (1064 MiB), so most of `sdc` is not used.
