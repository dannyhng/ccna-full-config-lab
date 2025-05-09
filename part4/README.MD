### Part 4: Rapid Spanning Tree Protocol

#### Step 1: Enable Rapid PVST+
Enabled Rapid Per-VLAN Spanning Tree (PVST+) on all Access and Distribution switches.

**Commands (Example on DSW-A1):**
```bash
spanning-tree mode rapid-pvst
```

#### Steps 1a, 1b: Root Bridge Alignment with HSRP
Aligned Root Bridge with HSRP Active router by setting STP priorities.

**Commands (On DSW-A1 - VLANs 10, 99):**
```bash
spanning-tree vlan 10,99 priority 0
```

**Commands (On DSW-A2 - VLANs 20, 40):**
```bash
spanning-tree vlan 20,40 priority 0
```

**Commands (On DSW-A2 - VLANs 10, 99):**
```bash
spanning-tree vlan 10,99 priority 4096
```

**Commands (On DSW-A1 - VLANs 20, 40):**
```bash
spanning-tree vlan 20,40 priority 4096
```

**Commands (On DSW-B1 - VLANs 10, 99):**
```bash
spanning-tree vlan 10,99 priority 0
text
```

**Commands (On DSW-B2 - VLANs 20, 30):**
```bash
spanning-tree vlan 20,30 priority 0
```

**Commands (On DSW-B2 - VLANs 10, 99):**
```bash
spanning-tree vlan 10,99 priority 4096
```

**Commands (On DSW-B1 - VLANs 20, 30):**
```bash
spanning-tree vlan 20,30 priority 4096
```

**Notes:** 
- Priority 0 is the lowest possible, making the switch the Root Bridge.
- Priority 4096 is one increment above (default is 32768).

#### Step 2: PortFast, BPDU Guard
Enabled PortFast and BPDU Guard on ports connected to end hosts.

**Commands (Example on ASW-A2 for PC1):**
```bash
interface f0/1
spanning-tree portfast
spanning-tree bpduguard enable
```

**Notes:** 
- Applied to all access ports (e.g., PC1, PC2, PC3, IP-Phone1, IP-Phone2, IP-Phone3, SRV1, Laptop1, Laptop2).
- Also applied to ASW-A1’s f0/2 (connected to WLC1).
