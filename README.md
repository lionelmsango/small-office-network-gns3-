# Small Office Network - My First GNS3 Project

This was my first hands-on experience building and configuring a network from scratch.


---

## What I Built

I created a simple office network topology with 3 computers connected through a switch. The goal was to understand the fundamentals of IP addressing, switching, and basic network connectivity.

**Quick Stats:**
- **Network:** 192.168.1.0/24
- **Devices:** 3 PCs, 1 switch, 1 NAT gateway
- **Time Spent:** ~5 hours
- **Tools:** GNS3 2.2.48, VirtualBox 7.0, Wireshark

- ---

## Network Topology

Here's the final topology I built in GNS3:

![Network Topology](screenshots/05-testing/02-topology-diagram.png)


The logical layout is pretty straightforward:

```
        Internet
           │
        NAT Cloud
      (192.168.1.1)
           │
    Ethernet Switch
     ╱    │    ╲
  PC1   PC2   PC3
```

---

## Network Design

Before jumping into GNS3, I planned out the IP addressing scheme. I went with a standard Class C private network since this is a small office scenario.

### IP Address Table

| Device | IP Address | Subnet Mask | Gateway | Notes |
|--------|------------|-------------|---------|-------|
| NAT Cloud | 192.168.1.1 | 255.255.255.0 | - | Internet gateway |
| PC1 | 192.168.1.10 | 255.255.255.0 | 192.168.1.1 | First workstation |
| PC2 | 192.168.1.11 | 255.255.255.0 | 192.168.1.1 | Second workstation |
| PC3 | 192.168.1.12 | 255.255.255.0 | 192.168.1.1 | Third workstation |

**Network Information:**
- Network: 192.168.1.0/24
- Broadcast: 192.168.1.255
- Available IPs: 192.168.1.1 - 192.168.1.254

---

## Setting Up the Environment

### Installing GNS3

I started by setting up GNS3 on my Windows machine. This involved installing VirtualBox first (for the GNS3 VM), then GNS3 itself, and finally importing the GNS3 VM. The VirtualBox setup was straightforward - just a standard installation. GNS3 installation took a bit longer since it includes Wireshark and other networking tools.

![GNS3 Installed](https://github.com/lionelmsango/small-office-network-gns3-/blob/0fbd676ee9dd0682cc941952debf4d628831a373/Screenshots/screenshots01-installation02-gns3-installed.png..jpg)

I imported the GNS3 VM and allocated 4GB RAM and 2 CPU cores. Initially I only gave it 2GB but performance was sluggish, so I bumped it up.

![GNS3 VM Imported](screenshots/01-installation/03-gns3-vm-imported.png)



### First Connection

After installation, I launched GNS3 and went through the initial setup wizard. I connected GNS3 to the VirtualBox VM, and after about a minute, I saw the green "Connected" status.

![GNS3 Connected](screenshots/02-setup/01-gns3-connected.png)

Seeing that green indicator was satisfying - everything was working!

---

---

## Building the Topology

### Starting Fresh

I created a new blank project called "Small-Office-Network" and stared at the empty workspace for a moment, planning where to place everything.

![Blank Project](screenshots/03-build/01-blank-project.png)

### Adding Devices

I dragged devices from the left sidebar into the workspace:
1. NAT cloud (for internet access)
2. Ethernet switch (to connect everything)
3. Three VPCS devices (lightweight virtual PCs)

I arranged them in a logical layout - NAT at the top, switch in the middle, and the three PCs spread out below.

![Devices Added](screenshots/03-build/02-devices-added.png)

At this point nothing was connected yet, just positioned on the canvas.

---

## Building the Topology

### Starting Fresh

I created a new blank project called "Small-Office-Network" and stared at the empty workspace for a moment, planning where to place everything.

![Blank Project](screenshots/03-build/01-blank-project.png)

### Adding Devices

I dragged devices from the left sidebar into the workspace:
1. NAT cloud (for internet access)
2. Ethernet switch (to connect everything)
3. Three VPCS devices (lightweight virtual PCs)

I arranged them in a logical layout - NAT at the top, switch in the middle, and the three PCs spread out below.

![Devices Added](screenshots/03-build/02-devices-added.png)

At this point nothing was connected yet, just positioned on the canvas.
