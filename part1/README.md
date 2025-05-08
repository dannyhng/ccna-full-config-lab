### Part 1: Initial Setup

#### Step 1: Hostnames
Configured hostnames for all devices to identify them clearly.

**Commands (Example for R1):**
```bash
enable
configure terminal
hostname R1
```

**Applied to:** R1, CSW1, CSW2, DSW-A1, DSW-A2, DSW-B1, DSW-B2, ASW-A1, ASW-A2, ASW-A3, ASW-B1, ASW-B2, ASW-B3

#### Steps 2, 3, 4: Enable Secret, User Account, Console
Set up an enable secret password, a local user account, and secured console access.

**Commands (Example for R1):**
```bash
enable secret thuhoang
username cisco secret ccna
line console 0
login local
exec-timeout 30
logging synchronous
```

**Explanation:**
- **enable secret thuhoang:** This command sets the password to access privileged EXEC mode (enable mode). The password is encrypted to prevent unauthorized access.

- **username cisco secret ccna:** This creates a local user account named cisco with the password ccna. The password is encrypted.

- **line console 0:** This enters console line configuration mode and allows you to modify settings for console access.

- **login local:** This command makes the device use the locally configured user credentials (from username cisco) for console login.

- **exec-timeout 30:** This command sets the session timeout to 30 minutes. After 30 minutes of inactivity, the session will automatically log off.

- **logging synchronous:** Ensures that log messages don’t interrupt your command input, making the console easier to work with.
