### Part 5: Static and Dynamic Routing

#### Step 1: OSPF
Configured OSPF on R1, CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, DSW-B2.

**Commands (On R1):**
```bash
interface Loopback0
ip ospf 1 area 0
interface g0/0
ip ospf 1 area 0
ip ospf network point-to-point
interface g0/1
ip ospf 1 area 0
ip ospf network point-to-point
router ospf 1
router-id 10.0.0.76
passive-interface Loopback0
```

**Commands (On CSW1):**
```bash
router ospf 1
router-id 10.0.0.77
network 10.0.0.41 0.0.0.0 area 0
network 10.0.0.34 0.0.0.0 area 0
network 10.0.0.45 0.0.0.0 area 0
network 10.0.0.49 0.0.0.0 area 0
network 10.0.0.53 0.0.0.0 area 0
network 10.0.0.57 0.0.0.0 area 0
network 10.0.0.77 0.0.0.0 area 0
passive-interface Loopback0
interface range g1/0/1, g1/1/1 - 4
ip ospf network point-to-point
```

**Commands (On CSW2):**
```bash
router ospf 1
router-id 10.0.0.78
network 10.0.0.42 0.0.0.0 area 0
network 10.0.0.38 0.0.0.0 area 0
network 10.0.0.61 0.0.0.0 area 0
network 10.0.0.65 0.0.0.0 area 0
network 10.0.0.69 0.0.0.0 area 0
network 10.0.0.73 0.0.0.0 area 0
network 10.0.0.78 0.0.0.0 area 0
passive-interface Loopback0
interface range g1/0/1, g1/1/1 - 4
ip ospf network point-to-point
```

**Commands (On DSW-A1):**
```bash
router ospf 1
router-id 10.0.0.79
network 10.0.0.46 0.0.0.0 area 0
network 10.0.0.62 0.0.0.0 area 0
network 10.0.0.79 0.0.0.0 area 0
network 10.1.0.2 0.0.0.0 area 0
network 10.2.0.2 0.0.0.0 area 0
network 10.6.0.2 0.0.0.0 area 0
passive-interface Loopback0
interface range g1/1/1 - 2
ip ospf network point-to-point
```

**Commands (On DSW-A2):**
```bash
router ospf 1
router-id 10.0.0.80
network 10.0.0.50 0.0.0.0 area 0
network 10.0.0.66 0.0.0.0 area 0
network 10.0.0.80 0.0.0.0 area 0
network 10.1.0.3 0.0.0.0 area 0
network 10.2.0.3 0.0.0.0 area 0
network 10.6.0.3 0.0.0.0 area 0
passive-interface Loopback0
interface range g1/1/1 - 2
ip ospf network point-to-point
```

**Commands (On DSW-B1):**
```bash
router ospf 1
router-id 10.0.0.81
network 10.0.0.54 0.0.0.0 area 0
network 10.0.0.70 0.0.0.0 area 0
network 10.0.0.81 0.0.0.0 area 0
network 10.3.0.2 0.0.0.0 area 0
network 10.4.0.2 0.0.0.0 area 0
network 10.5.0.2 0.0.0.0 area 0
passive-interface Loopback0
interface range g1/1/1 - 2
ip ospf network point-to-point
```

**Commands (On DSW-B2):**
```bash
router ospf 1
router-id 10.0.0.82
network 10.0.0.58 0.0.0.0 area 0
network 10.0.0.74 0.0.0.0 area 0
network 10.0.0.82 0.0.0.0 area 0
network 10.3.0.3 0.0.0.0 area 0
network 10.4.0.3 0.0.0.0 area 0
network 10.5.0.3 0.0.0.0 area 0
passive-interface Loopback0
interface range g1/1/1 - 2
ip ospf network point-to-point
```

**Notes:** 
- Used process ID 1, Area 0.
- Router IDs match loopback IPs.
- On switches, used exact IP matches with `network` command.
- On R1, enabled OSPF in interface mode.
- Made the loopback interface passive.
- Set physical connections (except PortChannel between CSW1/CSW2) to `point-to-point` network type (no DR/BDR election).

#### Step 2: Static Routing (Default Routes)
Configured default routes on R1.

**Commands (On R1):**
```bash
ip route 0.0.0.0 0.0.0.0 203.0.113.1
ip route 0.0.0.0 0.0.0.0 203.0.113.5 2
```

**Commands (On R1 for OSPF):**
```bash
router ospf 1
default-information originate
```

**Notes:** 
- First route via G0/0/0 (203.0.113.1), second via G0/1/0 (203.0.113.5) with AD 2 (floating).
- R1 acts as OSPF ASBR, advertising the default route.
