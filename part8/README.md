#### Step 1: IPv6 Addresses
Enabled IPv6 routing and configured addresses on R1, CSW1, CSW2.

**Commands (On R1):**
```bash
ipv6 unicast-routing
interface g0/0/0
ipv6 address 2001:db8:a::2/64
interface g0/1/0
ipv6 address 2001:db8:b::2/64
interface g0/0
ipv6 address 2001:db8:a1::/64 eui-64
interface g0/1
ipv6 address 2001:db8:a2::/64 eui-64
```

**Commands (On CSW1):**
```bash
ipv6 unicast-routing
interface g1/0/1
ipv6 address 2001:db8:a1::/64 eui-64
interface Port-channel1
ipv6 enable
```

**Commands (On CSW2):**
```bash
ipv6 unicast-routing
interface g1/0/1
ipv6 address 2001:db8:a2::/64 eui-64
interface Port-channel1
ipv6 enable
```

**Notes:** Used `ipv6 enable` on Port-channel1 to auto-generate a link-local address.

#### Step 2: IPv6 Static Routing (Default Routes)
Configured IPv6 default routes on R1.

**Commands (On R1):**
```bash
ipv6 route ::/0 2001:db8:a::1
ipv6 route ::/0 g0/1/0 2001:db8:b::1 2
```

**Notes:** 
- First route is recursive via 2001:db8:a::1.
- Second is fully-specified via G0/1/0 to 2001:db8:b::1 with AD 2 (floating).
