# Lab 1 — Your first virtual machine

Pair: Arman Ibrayev   Driver first half: Arman
Machine: ASUS, Windows, guest Ubuntu Server 26.04.1
Date: <19.09.2026>

## What I did
1. Installed <hypervisor> <version>. Licence: <link>
2. Created VM lab01-ubuntu: <2> vCPU, <2> GB RAM, 25 GB dynamic disk, NAT.
3. Installed Ubuntu Server, took snapshot clean-install.
4. Broke it:  sudo rm /etc/hostname
5. Rolled back to clean-install: cat /etc/hostname   

## Resource justification
<One paragraph, against your host's real totals from Lab 0.>

## Result
The VM boots; the break-and-rollback cycle is shown in the evidence below.

## What did not work the first time
The virtual machine did not install correctly because I stopped the machine during the previous installation process.

## Evidence
- [evidence/vm-settings.png](evidence/vm-settings.png) — CPU, memory and disk as configured
- [evidence/guest-release.txt](evidence/guest-release.txt) — Ubuntu Server is running in the guest
- [evidence/broken.png](evidence/broken.png) — the guest in its broken state
- [evidence/snapshot-list.png](evidence/snapshot-list.png) — the snapshot, after the rollback
