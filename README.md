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

I imported the GNS3 VM and allocated 4GB RAM and 2 CPU cores. Initially, I only gave it 2GB but performance was sluggish, so I bumped it up.

![GNS3 VM Imported](https://github.com/lionelmsango/small-office-network-gns3-/blob/16d901ab11d855f814882e6e43b1b60890ab7e11/Screenshots/screenshots01-installation03-gns3-vm-imported.png.jpg)



### First Connection

After installation, I launched GNS3 and went through the initial setup wizard. I connected GNS3 to the VirtualBox VM, and after about a minute, I saw the green "Connected" status.

![GNS3 Connected](https://github.com/lionelmsango/small-office-network-gns3-/blob/3dffa3f2d830748ec5083e059518ec9f88f2a3cd/Screenshots/screenshots02-setup01-gns3-connected.png.jpg)

Seeing that green indicator was satisfying that everything was working!


---


## Building the Topology

### Starting Fresh

I created a new blank project called "Small-Office-Network" and stared at the empty workspace for a moment, planning where to place everything.

![Blank Project](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots03-build01-blank-project.png.jpg)

### Adding Devices

I dragged devices from the left sidebar into the workspace:
1. NAT cloud (for internet access)
2. Ethernet switch (to connect everything)
3. Three VPCS devices (lightweight virtual PCs)

I arranged them in a logical layout - NAT at the top, switch in the middle, and the three PCs spread out below.

![Devices Added](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots03-build02-devices-added.png.jpg)

At this point nothing was connected yet, just positioned on the right place.

---


At this point nothing was connected yet, just positioned on the canvas.

### Making Connections

Next, I connected everything with virtual Ethernet cables:
- NAT cloud to the switch (port Ethernet0)
- Each PC to the switch (ports Ethernet1, 2, and 3)

I clicked on each device to reveal the connection points (green dots) and linked them together.

![Devices Connected](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots03-build03-devices-connected.png.jpg)

The topology was starting to look like an actual network.

### Final Touches

I renamed the devices to make them more meaningful:
- VPCS-1 → Office-PC-1
- VPCS-2 → Office-PC-2
- VPCS-3 → Office-PC-3
- Ethernet switch → Office-Switch

I also added a text note with the network details (192.168.1.0/24) for quick reference.

![Topology Final](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots03-build04-topology-final.png.jpg)


### Powering On

Time to bring the network to life. I clicked the green "Start" button and watched as all devices turned from red to green.

![All Running](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots03-build05-all-running.png.jpg)

Seeing all those green indicators meant the devices were running and ready to be configured.


---

## Configuring the Network

### PC1 Setup

I right-clicked on Office-PC-1 and opened its console. A terminal window popped up, and I was greeted with a simple `PC1>` prompt.

First, I checked if it had any IP address:
```bash
show ip
```

As expected: "No IP address assigned."

Time to configure it:
```bash
ip 192.168.1.10 255.255.255.0 192.168.1.1
save
```

Then verified:
```bash
show ip
```

Perfect! PC1 now had its IP address, subnet mask, and knew where the gateway was.

![PC1 Configured](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots04-configure01-pc1-ip-configured.png.jpg)


### Configuring PC2 and PC3

I repeated the same process for the other two PCs:

**PC2:**
```bash
ip 192.168.1.11 255.255.255.0 192.168.1.1
save
```

**PC3:**
```bash
ip 192.168.1.12 255.255.255.0 192.168.1.1
save
```

All three PCs were now configured with static IPs in the same subnet.

---

## Testing Connectivity

### The Moment of Truth

Would the PCs actually be able to talk to each other? I went to PC1's console and tried pinging PC2:

```bash
ping 192.168.1.11 -c 5
```

The terminal started showing responses:
```
84 bytes from 192.168.1.11 icmp_seq=1 ttl=64 time=0.435 ms
84 bytes from 192.168.1.11 icmp_seq=2 ttl=64 time=0.389 ms
...
```

Success! 0% packet loss. The PCs could communicate.

![Successful Ping](https://github.com/lionelmsango/small-office-network-gns3-/blob/1ffa908bcd05c0f7db874d51ab08120d2e5db24c/Screenshots/screenshots04-configure02-successful-ping.png.jpg)


---

## What I Learned

### Technical Skills

**GNS3 Basics**
- How to install and configure GNS3 with VirtualBox
- Using the GNS3 VM for running network devices
- Building topologies by dragging and connecting devices
- Starting/stopping devices and managing projects

**IP Addressing**
- Planning an IP addressing scheme for a network
- Understanding subnet masks (255.255.255.0 = /24)
- The role of default gateway in routing traffic
- Private IP address ranges (192.168.x.x)

**Switching**
- How switches forward traffic based on MAC addresses
- The concept of MAC address tables
- Broadcast vs. unicast traffic
- Switch ports and connections

**NAT (Network Address Translation)**
- How NAT allows multiple devices to share one public IP
- Private vs. public IP addresses
- NAT's role in home/office networks

**Network Testing**
- Using ping (ICMP) to test connectivity
- Reading ping statistics (packet loss, latency, TTL)
- What successful vs. failed pings look like
- Testing at different layers (local network, gateway, internet)

---

## Challenges I Faced

### GNS3 VM Performance Issues

**Problem:** When I first started the GNS3 VM with 2GB RAM, it was really slow. Devices took forever to start, and the whole application felt sluggish.

**Solution:** I increased the VM's RAM allocation to 4GB in VirtualBox settings. This made a huge difference - everything became responsive.

**Lesson:** Virtual machines need adequate resources. Skimping on RAM allocation just creates frustration.

### Understanding NAT vs. Router

**Problem:** I was initially confused about whether to use a NAT cloud or a router device. I wasn't sure what the difference was.

**Solution:** I read through GNS3 documentation and realized that for basic internet access, NAT cloud is simpler. It's essentially a pre-configured gateway. A router would require more configuration but give more control.

**Lesson:** Start simple. NAT cloud is perfect for learning basic connectivity before diving into router configuration.


---

## Next Steps

Now that I've got the basics down, I want to expand this project:

**Short-term:**
- Packet analysis: Capturing network traffic with Wireshark
- Add DHCP server instead of static IPs
- Configure VLANs to segment the network
- Add a second switch to create a more complex topology

**Medium-term:**
- Replace NAT cloud with an actual router (like Cisco IOS)
- Learn router configuration commands
- Implement inter-VLAN routing

**Long-term:**
- Build a multi-site network with VPN
- Add firewall rules and security policies
- Create a complete enterprise network simulation




