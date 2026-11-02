Setting Up Virtual Machines (Ubuntu Server and Windows 11) in VirtualBox

Hello Aigerim! To run virtual machines correctly, you need a laptop with at least 8 GB of RAM, 150 GB of storage, and an internet connection. Virtual machines are operating systems that run inside your main system. You can break them as much as you like, and nothing will happen to your main system.

Step 1. Check whether virtualization is enabled
First, open Task Manager with Ctrl + Shift + Esc. Go to Performance => CPU. Virtualization should be shown as enabled.
Why: Without virtualization enabled, VMs won’t start or will run very slowly.

Step 2. Install the hypervisor
Go to https://www.virtualbox.org/, download it, and install it with all default settings.
Why: All the following steps are done in VirtualBox, and it is completely free.

Step 3. Download the Ubuntu Server installer (ISO)
Download Ubuntu Server <<FILL: version, e.g. 24.04 LTS>> from https://documentation.ubuntu.com/release-notes/26.04/
Why Server and LTS: Server has no desktop, so it needs less memory; LTS means long-term support (security updates for years).

Step 4. Download the Windows 11 Enterprise Evaluation installer (ISO)
Go to https://www.microsoft.com/en-us/evalcenter/download-windows-11-enterprise, choose Windows 11 Enterprise, ISO, English (or your language).
Why the evaluation edition: It is free and legal for testing. It works for about 90 days and then reminds you to activate; for the next developer you build a fresh one. Do not use it for real office work.

Step 5. Create a new VM for Ubuntu
Click New, name it ubuntu-test, select the Ubuntu ISO, and set the type to Linux / Ubuntu 64-bit. Also uncheck the box “Perform configuration automatically”.
Why: Naming it clearly lets you tell the two machines apart later. Manual configuration is better optimized than automatic, since the program might allocate too much or too little memory and too many or too few cores.

Step 6. Configure Ubuntu
In the settings, set 2048 MB of RAM, 2 CPU cores, and a 25 GB disk. During the installation of the system itself, choose all the defaults.
Why: These are the optimal settings for your device, so that there is still room left for Windows.

Step 7. Configure Windows
Do the same for Windows, but allocate 4 GB of RAM and 60 GB of disk space. During the installation of Windows itself, also change nothing and choose all the defaults. Enable EFI, Secure Boot, and a virtual TPM 2.0.
Why: Windows requires more resources because it is heavier than Ubuntu Linux.

Step 8. Check the IP address on Linux and Windows
Linux: to find your IP address, type ip addr. The IP address should look something like 10.0.2.x.
Windows: press Win + X, type powershell, and in it enter ipconfig. Look for IPv4; it should look something like 10.0.2.x.
Why: The console is the fastest way to find the IP address.

Step 9. Make the machines see each other on the network
In VirtualBox, at the top left, click File, then Tools, then Network. In the new tab, click Create. Then shut down both virtual machines (in Linux, type sudo poweroff; in Windows, shut down as usual). After shutting them down, open your machine’s settings, go to Network, and in Adapter 1 set “Attached to” to NAT Network, with the name NatNetwork. Do this for both machines.
Why: This is needed so the two virtual machines can see each other on the network, because by default the application deliberately isolates them from each other to avoid any conflicts.

Step 10. Check Windows
Start Windows, open PowerShell, and type ping (Linux IP). It should send packets without loss. Then type ping 8.8.8.8, which checks that the system has internet access.
Why: We use the ping command to see whether the system has network access and whether it can see another device. 8.8.8.8 is Google’s regular DNS server, which can be used to check for internet access.

Step 11. Check Linux
First, type ping ubuntu.com to check internet access. Then type ping (Windows address); most likely it won’t work. Now go to the Windows machine, open PowerShell as administrator, and enter (the exact fix you used, e.g. PowerShell as Administrator: New-NetFirewallRule -DisplayName "Allow ICMPv4" -Protocol ICMPv4 -IcmpType 8 -Action Allow).
Then check the ping again; it should now work.
Why: The first attempt didn’t get a response from Windows because its firewall is enabled by default and blocked the signal, and it can only be changed with administrator rights.

Step 12. Snapshot
To create a snapshot, select the VM, click Snapshots, right-click Current State, choose Take, and give it a name. To restore a snapshot, also go to Snapshots, right-click the snapshot’s name, and click Restore. It will automatically load your snapshot, discarding everything that came after it.
Why: Snapshots are made so you can safely roll back to a previous version if something breaks.
