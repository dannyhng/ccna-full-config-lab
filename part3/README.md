### Part 3: IP Addresses, L3 EtherChannel, HSRP

#### Step 1: R1 IP Addresses
Assigned IP addresses to R1’s interfaces.

**Commands (On R1):**
```bash
interface g0/0/0
ip address dhcp
no shutdown
interface g0/1/0
ip address dhcp
no shutdown
interface g0/0
ip address 10.0.0.33 255.255.255.252
no shutdown
interface g0/1
ip address 10.0.0.37 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.76 255.255.255.255
no shutdown
```

#### Step 2: Enable IPv4 Routing on Core/Distribution Switches
Enabled IP routing on CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, DSW-B2.

**Commands (Example on CSW1):**
```bash
ip routing
```

#### Step 3: L3 EtherChannel (PAgP)
Configured a Layer-3 EtherChannel between CSW1 and CSW2 using PAgP.

**Commands (On CSW1):**
```bash
interface range g1/1/1 - 2
channel-group 1 mode desirable
interface Port-channel1
no switchport
ip address 10.0.0.41 255.255.255.252
```

**Commands (On CSW2):**
```bash
interface range g1/1/1 - 2
channel-group 1 mode desirable
interface Port-channel1
no switchport
ip address 10.0.0.42 255.255.255.252
```

#### Steps 4, 5: CSW1, CSW2 IP Addresses
Assigned IP addresses to CSW1 and CSW2, disabled unused interfaces.

**Commands (On CSW1):**
```bash
interface g1/0/1
no switchport
ip address 10.0.0.34 255.255.255.252
no shutdown
interface g1/1/1
no switchport
ip address 10.0.0.45 255.255.255.252
no shutdown
interface g1/1/2
no switchport
ip address 10.0.0.49 255.255.255.252
no shutdown
interface g1/1/3
no switchport
ip address 10.0.0.53 255.255.255.252
no shutdown
interface g1/1/4
no switchport
ip address 10.0.0.57 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.77 255.255.255.255
no shutdown
interface range g1/0/2 - 24, g1/1/5 - 24
shutdown
```

**Commands (On CSW2):**
```bash
interface g1/0/1
no switchport
ip address 10.0.0.38 255.255.255.252
no shutdown
interface g1/1/1
no switchport
ip address 10.0.0.61 255.255.255.252
no shutdown
interface g1/1/2
no switchport
ip address 10.0.0.65 255.255.255.252
no shutdown
interface g1/1/3
no switchport
ip address 10.0.0.69 255.255.255.252
no shutdown
interface g1/1/4
no switchport
ip address 10.0.0.73 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.78 255.255.255.255
no shutdown
interface range g1/0/2 - 24, g1/1/5 - 24
shutdown
```

#### Steps 6, 7, 8, 9: Distribution Switch IP Addresses
Assigned IP addresses to Distribution switches.

**Commands (On DSW-A1):**
```bash
interface g1/1/1
no switchport
ip address 10.0.0.46 255.255.255.252
no shutdown
interface g1/1/2
no switchport
ip address 10.0.0.62 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.79 255.255.255.255
no shutdown
```

**Commands (On DSW-A2):**
```bash
interface g1/1/1
no switchport
ip address 10.0.0.50 255.255.255.252
no shutdown
interface g1/1/2
no switchport
ip address 10.0.0.66 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.80 255.255.255.255
no shutdown
```

**Commands (On DSW-B1):**
```bash
interface g1/1/1
no switchport
ip address 10.0.0.54 255.255.255.252
no shutdown
interface g1/1/2
no switchport
ip address 10.0.0.70 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.81 255.255.255.255
no shutdown
```

**Commands (On DSW-B2):**
```bash
interface g1/1/1
no switchport
ip address 10.0.0.58 255.255.255.252
no shutdown
interface g1/1/2
no switchport
ip address 10.0.0.74 255.255.255.252
no shutdown
interface Loopback0
ip address 10.0.0.82 255.255.255.255
no shutdown
```

#### Step 10: SRV1 IP Settings
Manually configured SRV1’s IP settings in Packet Tracer GUI.

**Settings:**
- IPv4 Address: 10.5.0.4
- Subnet Mask: 255.255.255.0
- Default Gateway: 10.5.0.1

#### Step 11: Access Switch Management IP Addresses
Assigned management IP addresses to Access switches on VLAN 99.

**Commands (On ASW-A1):**
```bash
interface vlan 99
ip address 10.0.0.4 255.255.255.240
ip default-gateway 10.0.0.1
```

**Commands (On ASW-A2):**
```bash
interface vlan 99
ip address 10.0.0.5 255.255.255.240
ip default-gateway 10.0.0.1
```

**Commands (On ASW-A3):**
```bash
interface vlan 99
ip address 10.0.0.6 255.255.255.240
ip default-gateway 10.0.0.1
```

**Commands (On ASW-B1):**
```bash
interface vlan 99
ip address 10.0.0.20 255.255.255.240
ip default-gateway 10.0.0.17
```

**Commands (On ASW-B2):**
```bash
interface vlan 99
ip address 10.0.0.21 255.255.255.240
ip default-gateway 10.0.0.17
```

**Commands (On ASW-B3):**
```bash
interface vlan 99
ip address 10.0.0.22 255.255.255.240
ip default-gateway 10.0.0.17
```

**Note:**
- An IP address is assigned to the interface (such as a Layer 3 Port-Channel or VLAN interface) to enable remote management or routing functionality. In the case of a Layer 3
EtherChannel or routed port, the IP allows the switch or router to participate in IP routing. For a VLAN interface (SVI), the IP provides Layer 3 access to the switch for
management purposes, such as SSH or SNMP.

#### Steps 12, 13, 14, 15: HSRP (Office A)
Configured HSRPv2 for Office A.

**Commands (On DSW-A1 - Group 1, VLAN 99):**
```bash
interface vlan 99
ip address 10.0.0.2 255.255.255.240
standby version 2
standby 1 ip 10.0.0.1
standby 1 priority 105
standby 1 preempt
```

**Commands (On DSW-A2 - Group 1, VLAN 99):**
```bash
interface vlan 99
ip address 10.0.0.3 255.255.255.240
standby version 2
standby 1 ip 10.0.0.1
```

**Commands (On DSW-A1 - Group 2, VLAN 10):**
```bash
interface vlan 10
ip address 10.1.0.2 255.255.255.0
standby version 2
standby 2 ip 10.1.0.1
standby 2 priority 105
standby 2 preempt
```

**Commands (On DSW-A2 - Group 2, VLAN 10):**
```bash
interface vlan 10
ip address 10.1.0.3 255.255.255.0
standby version 2
standby 2 ip 10.1.0.1
```

**Commands (On DSW-A1 - Group 3, VLAN 20):**
```bash
interface vlan 20
ip address 10.2.0.2 255.255.255.0
standby version 2
standby 3 ip 10.2.0.1
```

**Commands (On DSW-A2 - Group 3, VLAN 20):**
```bash
interface vlan 20
ip address 10.2.0.3 255.255.255.0
standby version 2
standby 3 ip 10.2.0.1
standby 3 priority 105
standby 3 preempt
```

**Commands (On DSW-A1 - Group 4, VLAN 40):**
```bash
interface vlan 40
ip address 10.6.0.2 255.255.255.0
standby version 2
standby 4 ip 10.6.0.1
```

**Commands (On DSW-A2 - Group 4, VLAN 40):**
```bash
interface vlan 40
ip address 10.6.0.3 255.255.255.0
standby version 2
standby 4 ip 10.6.0.1
standby 4 priority 105
standby 4 preempt
```

**Note:**
- Then we do the same with Office B but opposite priorities for the Distribution Switches


