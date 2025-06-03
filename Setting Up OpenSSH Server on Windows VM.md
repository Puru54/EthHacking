
#linux #ssh #server
## **1. Check if OpenSSH is Installed**
Run the following command in **PowerShell (Admin):**
```powershell
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'
```

## **2. Install OpenSSH Server**
If OpenSSH is not installed, run:
```powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```

## **3. Start and Enable SSH Service**
```powershell
Start-Service sshd
Set-Service -Name sshd -StartupType Automatic
```

## **4. Allow SSH in Windows Firewall**
```powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

## **5. Verify SSH Service is Running**
```powershell
Get-Service sshd
```
Expected output:
```
Status   Name               DisplayName
------   ----               -----------
Running  sshd               OpenSSH SSH Server
```

## **6. Connect from Kali Linux**
On your Kali VM, run:
```bash
ssh <Windows_Username>@<Windows_IP>
```
Enter the Windows password when prompted.

---

### **1. Configure Network Settings**

You have a few options for networking between the VMs:

#### **Option 1: Use Host-Only Network (For VM-to-VM Only)**

1. Open **VMware Workstation**.
    
2. Go to **Edit** → **Virtual Network Editor**.
    
3. Select **VMnet1 (Host-Only)** and ensure DHCP is enabled.
    
4. Set both VMs to use **Host-Only Adapter**:
    
    - Right-click each VM → **Settings** → **Network Adapter**.
        
    - Choose **Custom** → Select **VMnet1 (Host-Only)**.
        
    - Click **OK** and restart the VMs.
        

#### **Option 2: Use NAT (For VM-to-VM + Internet Access)**

1. Go to **Edit** → **Virtual Network Editor**.
    
2. Select **VMnet8 (NAT)**.
    
3. In **VM Settings**, choose **Custom** → **VMnet8 (NAT)** for both VMs.
    
4. Restart the VMs.
    

#### **Option 3: Use Bridged Network (For VM-to-VM + LAN Access)**

1. Set both VMs to **Bridged** mode.
    
2. This will assign them IPs from your physical network.
    
3. Run `ipconfig` or `ifconfig` to check their IPs.
    

---

### **2. Test Connectivity**

Once networking is set up, verify the VMs can see each other:

1. **Find VM IP Addresses**
    
    - On Windows VM: Run `ipconfig`
        
    - On Kali Linux: Run `ifconfig` or `ip a`
        
2. **Ping the Other VM**
    
    - From Windows: `ping <Kali_VM_IP>`
        
    - From Kali: `ping <Windows_VM_IP>`
        

If pings fail, disable firewalls temporarily:

- Windows: `netsh advfirewall set allprofiles state off`
    
- Kali: `ufw disable`
    

---

### **3. Enable SSH for Remote Access (Kali to Windows)**

1. Install OpenSSH Server on Windows:
    
    powershell
    
    CopyEdit
    
    `Add-WindowsFeature -Name OpenSSH-Server`
    
2. Start SSH Service:
    
    powershell
    
    CopyEdit
    
    `Start-Service sshd`
    
3. Connect from Kali:
    
    bash
    
    CopyEdit
    
    `ssh username@<Windows_IP>`