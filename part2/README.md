### Part 2: VLANs, L2 EtherChannel

#### Step 1: L2 EtherChannel (PAgP) - Office A
Configured a Layer-2 EtherChannel named PortChannel1 between DSW-A1 and DSW-A2 using PAgP (Cisco-proprietary).

**Commands (On DSW-A1 and DSW-A2):**
```bash
interface range g1/0/4-5
channel-group 1 mode desirable
```

#### Step 2: L2 EtherChannel (LACP) - Office B
Configured a Layer-2 EtherChannel named PortChannel1 between DSW-B1 and DSW-B2 using LACP (open standard).

**Commands (On DSW-B1 and DSW-B2):**
```bash
interface range g1/0/4-5
channel-group 1 mode active
```

#### Step 3: Trunk Configuration
Configured trunk links between Access and Distribution switches, including EtherChannels.

**Commands (Example on DSW-A1/DSW-A2 for interfaces that connect to the DSW-A2/DSW-A1 and for PortChannel1):**
```bash
interface range g1/0/1-3
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,40,99
```

```bash
interface po1
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,40,99
```

**Commands (Example on ASW-A1/ASW-A2/ASW-A3 for interfaces that connect to the DSW-A1/DSW-A2):**
```bash
interface range g0/1-2
switchport mode trunk
switchport nonegotiate
switchport trunk native vlan 1000
switchport trunk allowed vlan 10,20,40,99
```

**Notes:** 
- Applied `switchport nonegotiate` to disable DTP on all trunk ports.
- Set native VLAN to 1000 (unused).
- Allowed VLANs 10, 20, 40, 99 for Office A; VLANs 10, 20, 30, 99 for Office B.
- Repeated for all links between Access and Distribution switches (e.g., DSW-B1 to DSW-B2, DSW-B1 to ASW-B1).

#### Step 4: VTP
Configured VTPv2 with DSW-A1 and DSW-B1 as servers.

**Commands (On DSW-A1 and DSW-B1):**
```bash
vtp domain DannysLab
vtp version 2
```

**Verification (On other switches):**
```bash
show vtp status
```

**Commands (On Access Switches - ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, ASW-B3):**
```bash
vtp mode client
```

#### Steps 5, 6: VLAN Configuration
Created VLANs on DSW-A1 (Office A) and DSW-B1 (Office B).

**Commands (On DSW-A1 for Office A):**
```bash
vlan 10
name PCs
vlan 20
name Phones
vlan 40
name Wi-Fi
vlan 99
name Management
```

**Commands (On DSW-B1 for Office B):**
```bash
vlan 10
name PCs
vlan 20
name Phones
vlan 30
name Servers
vlan 99
name Management
```

**Notes:** VTP propagated the VLANs to other switches in the domain.

#### Step 7: Access Port Configuration
Configured access ports on Access switches for end devices.

**Commands (Example on ASW-A1 connects to Access Point):**
```bash
interface f0/1
switchport mode access
switchport nonegotiate
switchport access vlan 99
```
**Commands (On ASW-A1/ASW-A2/ASW-B2 for iphone):**
```bash
interface f0/1
switchport mode access
switchport nonegotiate
switchport access vlan 10
switchport access voice vlan 20
```

**Commands (On ASW-B3 for SRV1):**
```bash
interface f0/1
switchport mode access
switchport access vlan 30
switchport nonegotiate
```

**Notes:** 
- LWAPs don’t use FlexConnect, so they connect via trunks (configured later).
- PCs in VLAN 10, Phones in VLAN 20, SRV1 in VLAN 30.
- Disabled DTP with `switchport nonegotiate`.

  #### Step 8: WLC Connection Configuration (Trunk)
Configured ASW-A1’s connection to WLC1.

**Commands (On ASW-A1):**
```bash
interface f0/2
switchport mode trunk
switchport trunk native vlan 99
switchport trunk allowed vlan 40,99
switchport nonegotiate
```

**Notes:** 
- Supports VLAN 40 (Wi-Fi) and VLAN 99 (Management, untagged).
- Disabled DTP.
  
#### Step 9: Disabling Unused Ports
Disabled unused ports on Access and Distribution switches.

**Commands (Example on ASW-A1):**
```bash
interface range g1/0/6-24,g1/1/3-4
shutdown
```

**Notes:** Repeated for all unused ports on ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, ASW-B3, DSW-A1, DSW-A2, DSW-B1, DSW-B2.
