#### Step 1: Extended ACLs
Configured ACL to control traffic between Office A and Office B.

**Commands (On DSW-B1):**
```bash
access-list 100 permit icmp 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
access-list 100 deny ip 10.1.0.0 0.0.0.255 10.3.0.0 0.0.0.255
access-list 100 permit ip any any
interface vlan 10
ip access-group 100 in
```

**Notes:** 
- Applied on DSW-B1’s VLAN 10 interface (closest to Office B PCs subnet).
- Follows extended ACL best practice (apply near the destination).

#### Step 2: Port Security
Enabled port security on Access switches’ F0/1 ports.

**Commands (Example on ASW-A1 for PC1):**
```bash
interface f0/1
switchport port-security
switchport port-security maximum 1
switchport port-security violation restrict
switchport port-security mac-address sticky
```

**Notes:** 
- Maximum 1 MAC address per port (PCs, Phones, SRV1 use single MAC).
- `restrict` mode drops invalid traffic, sends notifications.
- Applied to all F0/1 ports (PC1, PC2, PC3, IP-Phone1, IP-Phone2, IP-Phone3, SRV1).

#### Step 3: DHCP Snooping
Enabled DHCP snooping on Access switches.

**Commands (On ASW-A1, ASW-A2, ASW-A3):**
```bash
ip dhcp snooping
ip dhcp snooping vlan 10,20,40,99
ip dhcp snooping information option
interface range g0/1-2
ip dhcp snooping trust
interface f0/1
ip dhcp snooping limit rate 15
```

**Commands (On ASW-A1 to interface connects to WLC)**
```bash
interface f0/2
ip dhcp snooping limit rate 100
```

**Commands (On ASW-B1, ASW-B2, ASW-B3)**
```bash
ip dhcp snooping
ip dhcp snooping vlan 10,20,30,99
ip dhcp snooping information option
interface range g0/1-2
ip dhcp snooping trust
interface f0/1
ip dhcp snooping limit rate 15
```

**Notes:** 
- Enabled for active VLANs in each LAN.
- Trusted uplink ports to Distribution switches.
- Set rate limit to 15 pps on untrusted ports, 100 pps on ASW-A1’s WLC1 connection.

#### Step 4: Dynamic ARP Inspection (DAI)
Enabled DAI on Access switches.

**Commands (On ASW-A1,ASW-A2-ASW-A3):**
```bash
ip arp inspection vlan 10,20,40,99
ip arp inspection validate src-mac dst-mac ip
interface range g0/1-2
ip arp inspection trust
```

**Commands (On ASW-B1,ASW-B2-ASW-B3):**
```bash
ip arp inspection vlan 10,20,30,99
ip arp inspection validate src-mac dst-mac ip
interface range g0/1-2
ip arp inspection trust
```
**Notes:** 
- Enabled for active VLANs.
- Trusted uplink ports.
- Enabled all validation checks (source MAC, destination MAC, IP).
