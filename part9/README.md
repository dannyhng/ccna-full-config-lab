### Part 9: Wireless

#### Step 1: Accessing WLC1
Accessed WLC1 GUI from a PC (e.g., PC1) at https://10.0.0.7.

**Credentials:**
- Username: admin
- Password: adminPW12

#### Step 2: Dynamic Interface Configuration
Configured a dynamic interface for the Wi-Fi WLAN on WLC1 via GUI.

**Settings:**
- Name: Wi-Fi
- VLAN: 40
- Port number: 1
- IP address: 10.6.0.4
- Gateway: 10.6.0.1
- DHCP server: 10.0.0.76

**Notes:** Used 10.6.0.4 to avoid conflict with DSW-A1’s 10.6.0.2.

#### Step 3: WLAN Configuration
Configured and enabled the Wi-Fi WLAN on WLC1 via GUI.

**Settings:**
- Profile name: Wi-Fi
- SSID: Wi-Fi
- ID: 1
- Status: Enabled
- Security: WPA2 Policy, AES encryption, PSK: cisco123

#### Step 4: LWAP Confirmation
Verified that LWAP1 and LWAP2 associated with WLC1 via GUI.

**Notes:** Packet Tracer limitation prevents wireless clients from leasing IP addresses.

## Conclusion

This lab provided hands-on experience with a wide range of CCNA topics, from basic device configuration to advanced routing, security, and wireless setups. All configurations were tested in Packet Tracer to ensure functionality.

## Credits

This lab is based on a tutorial from [Jeremy’s IT Lab on YouTube](https://www.youtube.com/c/JeremysITLab). I recreated the lab for personal study and practice purposes.
