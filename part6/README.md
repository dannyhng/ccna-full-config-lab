### Part 6: Network Services: DHCP, DNS, NTP, SNMP, Syslog, FTP, SSH, NAT

#### Step 1: DHCP Pools
Configured DHCP pools on R1.

**Commands (On R1):**
```bash
ip dhcp excluded-address 10.0.0.1 10.0.0.10
ip dhcp pool A-Mgmt
network 10.0.0.0 255.255.255.240
default-router 10.0.0.1
domain-name dannyslab.com
dns-server 10.5.0.4
option 43 ip 10.0.0.7
ip dhcp excluded-address 10.1.0.1 10.1.0.10
ip dhcp pool A-PC
network 10.1.0.0 255.255.255.0
default-router 10.1.0.1
domain-name dannyslab.com
dns-server 10.5.0.4
ip dhcp excluded-address 10.2.0.1 10.2.0.10
ip dhcp pool A-Phone
network 10.2.0.0 255.255.255.0
default-router 10.2.0.1
domain-name dannyslab.com
dns-server 10.5.0.4
ip dhcp excluded-address 10.0.0.17 10.0.0.26
ip dhcp pool B-Mgmt
network 10.0.0.16 255.255.255.240
default-router 10.0.0.17
domain-name dannyslab.com
dns-server 10.5.0.4
option 43 ip 10.0.0.7
ip dhcp excluded-address 10.3.0.1 10.3.0.10
ip dhcp pool B-PC
network 10.3.0.0 255.255.255.0
default-router 10.3.0.1
domain-name dannyslab.com
dns-server 10.5.0.4
ip dhcp excluded-address 10.4.0.1 10.4.0.10
ip dhcp pool B-Phone
network 10.4.0.0 255.255.255.0
default-router 10.4.0.1
domain-name dannyslab.com
dns-server 10.5.0.4
ip dhcp excluded-address 10.6.0.1 10.6.0.10
ip dhcp pool Wi-Fi
network 10.6.0.0 255.255.255.0
default-router 10.6.0.1
domain-name dannyslab.com
dns-server 10.5.0.4
```

#### Step 2: DHCP Relay Agent (ip helper-address)
Configured DHCP relay on Distribution switches.

**Commands (On DSW-A1 and DSW-A2):**
```bash
interface vlan 10
ip helper-address 10.0.0.76
interface vlan 20
ip helper-address 10.0.0.76
interface vlan 40
ip helper-address 10.0.0.76
interface vlan 99
ip helper-address 10.0.0.76
```

**Commands (On DSW-B1 and DSW-B2):**
```bash
interface vlan 10
ip helper-address 10.0.0.76
interface vlan 20
ip helper-address 10.0.0.76
interface vlan 30
ip helper-address 10.0.0.76
interface vlan 99
ip helper-address 10.0.0.76
```

#### Step 3: DNS Records (SRV1)
Configured DNS records on SRV1 via Packet Tracer GUI.

**Settings:**
- google.com = 172.253.62.100
- youtube.com = 152.250.31.93
- dannyslab.com = 66.235.200.145
- www.dannyslab.com = dannyslab.com (CNAME)

#### Step 4: Domain Name, DNS Server Configuration
Configured domain name and DNS server on all routers and switches.

**Commands (Example on R1):**
```bash
ip domain-name dannyslab.com
ip name-server 10.5.0.4
```

#### Step 5: NTP (R1)
Configured R1 as an NTP server.

**Commands (On R1):**
```bash
ntp server 216.239.35.0
ntp master 5
```

**Notes:** NTP sync may take time in Packet Tracer; I moved forward without waiting.

#### Step 6: NTP (Switches), NTP Authentication
Configured switches to sync with R1’s loopback.

**Commands (Example on CSW1):**
```bash
ntp authenticate
ntp authentication-key 1 md5 ccna
ntp trusted-key 1
ntp server 10.0.0.76 key 1
```

**Notes:** Applied to CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, DSW-B2, ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, ASW-B3.

#### Steps 7, 8: SNMP, Syslog
Enabled SNMP and Syslog.

**Commands (Example on R1):**
```bash
snmp-server community SNMPSTRING RO
logging host 10.5.0.4
logging buffered 8192
```

**Notes:** Applied to all routers and switches.

#### Step 9: FTP, IOS Upgrade
Used FTP on R1 to download a new IOS version from SRV1.

**Commands (On R1):**
```bash
ip ftp username cisco
ip ftp password cisco
copy ftp://10.5.0.4/c2900-universalk9-mz.SPA.155-3.M4a.bin flash:
boot system flash:c2900-universalk9-mz.SPA.155-3.M4a.bin
write memory
reload
```

**Post-Reload Commands (On R1):**
```bash
delete flash:c2900-universalk9-mz.SPA.155-3.M.bin
```

**Notes:** Assumed the old IOS was `c2900-universalk9-mz.SPA.155-3.M.bin`.

#### Step 10: SSH
Enabled SSH on all routers and switches.

**Commands (Example on R1):**
```bash
ip domain-name dannyslab.com
crypto key generate rsa
2048
access-list 1 permit 10.1.0.0 0.0.0.255
line vty 0 15
access-class 1 in
transport input ssh
login local
logging synchronous
ip ssh version 2
```

**Notes:** 
- Used 2048-bit RSA keys (largest in Packet Tracer).
- Restricted SSH to Office A PCs subnet (10.1.0.0/24).
- Applied to all routers and switches.

#### Step 11: Static NAT
Configured static NAT on R1 for SRV1.

**Commands (On R1):**
```bash
ip nat inside source static 10.5.0.4 203.0.113.113
interface g0/0
ip nat inside
interface g0/1
ip nat inside
interface g0/0/0
ip nat outside
interface g0/1/0
ip nat outside
```

#### Step 12: Dynamic PAT (Pool-Based)
Configured dynamic PAT on R1.

**Commands (On R1):**
```bash
access-list 2 permit 10.1.0.0 0.0.0.255
access-list 2 permit 10.2.0.0 0.0.0.255
access-list 2 permit 10.3.0.0 0.0.0.255
access-list 2 permit 10.4.0.0 0.0.0.255
access-list 2 permit 10.6.0.0 0.0.0.255
ip nat pool POOL1 203.0.113.200 203.0.113.207 netmask 255.255.255.248
ip nat inside source list 2 pool POOL1 overload
```

*Verification and Failover Test:**
- Pinged dannyslab.com from a host (e.g., PC1) to confirm Internet access.
- Disabled G0/0/0: `interface g0/0/0`, `shutdown`.
- Removed and re-added OSPF default route:

```bash
router ospf 1
no default-information originate
default-information originate
```
- Pinged again to confirm failover.
- Re-enabled G0/0/0: `interface g0/0/0`, `no shutdown`.
- Repeated OSPF default route removal and re-addition.

#### Step 13: Disabling CDP, Enabling LLDP
Disabled CDP and enabled LLDP on all devices.

**Commands (Example on R1):**
```bash
no cdp run
lldp run
```

**Commands (On Access Switches - ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, ASW-B3):**
```bash
interface f0/1
no lldp transmit
```

**Notes:** Applied to all routers and switches; disabled LLDP transmit on access ports.
