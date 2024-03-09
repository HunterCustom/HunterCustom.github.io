---
title: Home Server Specs
author: hunter
date: 2024-01-01 11:43:00 +0800
categories: [HomeLab, Server]
tags: [Dell, R730, Home Lab, Server, PC Specs]
---

# Here's the improved version of your blog post:

---
title: Home Server Specs
author: Hunter
date: 2024-01-01 11:43:00 +0800
categories: [HomeLab, Server]
tags: [Dell, R730, Home Lab, Server, PC Specs]

# My Home Lab Specs & Plans

- iDRAC Enterprise
- All 3 Risers 
- 2 x 1100-watt power supply
- 10 Gb networking 
- 2 x 2683 V4 CPUs 
- 128GB DDR4 ECC 2400MHz (4x 32GB Sticks) 
- 16 bay with 4x 1TB Samsung 870 SSDs
- H730p
- GTX 1070

Connected to a Dell MD1200 via a Dell H810:
- MD1200
- 12 3.5" HGST 8TB 7200RPM Drives

Currently, the system is set up on Unraid, running a Jellyfin Media Server. However, due to power usage, I've shifted 24/7 tasks to a Lenovo M900 with an i7-6700 and 32GB of DDR4 RAM.

The M900 Runs:
- Vaultwarden
- Mealie
- Cloudflare
- This website
- Portainer
- CasaOS
- Guacamole

Current Plans are to convert my original server into a 2u Chassis and load the M900 features onto this as my original server supports ECC RAM.

The specs for that server are currently:
- Supermicro X10SAE
- i7-4709k
- 32GB of non-ECC DDR3 RAM
- 250GB Samsung 870 EVO
- All inside of an old mid-tower PC case I had lying around.

The plan will be to swap parts to:
- Supermicro X10SAE
- Xeon E3-1285L v4
- 32GB of DDR3 ECC RAM
- Dell H830 Raid Controller in HBA Mode? Might not do this as it's known to create issues, but I'd like to utilize the 12 GB/s from the MD1200 instead of my current 6 GB/s setup.

I'd also like to swap the 1070 to an RTX A2000 in the R730 as the A2000 has more transcoding capability than the 1070 (11 vs. 6 streams of 4k downscaled to 1080p).

The goal of my homelab is to be able to run a few programs 24/7 such as Vaultwarden, the website, Mealie, and Jellyfin on my low-power server that would be connected to the MD1200 for storage. While the R730 would be used to automatically download and convert video files to .h265 via Tdarr for more storage savings, even if I have 78TB of storage.

Hopefully, I can make it so that the R730 and the Low Power server can easily share files between each other using the MD1200; this should be super simple. The R730 will most likely end up running Proxmox, while the Low Power will run Unraid as well. I will also convert my Unraid OS to a Samsung Bar Plus 128GB USB Stick instead of the current unknown name brand 32GB stick that has been in my house since at least 7th grade.

The reason for the R730 to be running Proxmox is that I want the ability to create VMs easily as well as mess with new environments that I learn about as I grow in my field.
