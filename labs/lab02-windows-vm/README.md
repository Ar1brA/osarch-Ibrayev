# Lab 2 — A Windows guest, and a network between them

Pair: <Имя Фамилия 1> / <Имя Фамилия 2>   Driver first half: <Имя>
Machine: <Модель ноутбука, например: Asus Zenbook, Windows 11, 16 GB RAM, 8 cores>, guests Ubuntu Server + Windows 11 Enterprise eval
Date: 27.10.2026

## What I did
1. Downloaded Windows 11 Enterprise evaluation. Licence: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise
2. Created lab02-windows: 2 vCPU, 4 GB, 64 GB thin, UEFI + Secure Boot, vTPM.
3. Network mode: VirtualBox NAT Network (NatNetwork), both guests. Why: Standard VirtualBox NAT isolates each VM behind an independent router, preventing them from seeing each other. Bridged mode fails on campus Wi-Fi due to MAC filtering. NAT Network provides an internal shared subnet with outbound Internet access.
4. Ping Ubuntu → Windows timed out because Windows Defender Firewall blocks inbound ICMPv4 Echo Requests by default on Public network profiles. Fixed with: `New-NetFirewallRule -DisplayName "Allow ping in (ICMPv4)" -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow`
5. Overcommitment: set 5 GB + 5 GB on a 8 GB host.

## Network diagram
```text
              Internet
                 |
      Host laptop (Campus Wi-Fi)
                 |
   [VirtualBox NAT Network · 10.0.2.0/24]
          /                        \
lab01-ubuntu                  lab02-windows
 10.0.2.X                       10.0.2.Y
