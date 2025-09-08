# SSH Connection to Klipper Printer on Armbian

## Introduction

SSH (Secure Shell) is a protocol for secure remote device management over a network. In the context of 3D printing, SSH is used to manage single-board computers (Orange Pi, Raspberry Pi, etc.) with Armbian installed, running Klipper firmware.

## What is SSH in the Context of 3D Printing

SSH allows you to:
- Remotely control the printer's computer from the command line
- Edit Klipper configuration files
- Install and update software
- Diagnose system problems
- Transfer files between your computer and the printer

## Preparation for Connection

### Requirements
- Single-board computer with Armbian installed
- Klipper installed on the computer
- Network connection (Wi-Fi or Ethernet)
- SSH client on your computer

### Getting the Printer's IP Address
**Method 1: From the printer menu**
1. Settings (gear icon)
2. Network
<img src="/docs/images/network-screen.jpg" width="300">

**Method 2: Via Mainsail/Fluidd web interface**
1. Open the printer's web interface
2. Go to "System" section
3. Find network information and note the IP address

**Method 3: Via router**
1. Access your router's admin panel (usually 192.168.1.1 or 192.168.0.1)
2. Find the list of connected devices
3. Locate your printer by name or MAC address

**Method 4: Network scanning**
```bash
# On Linux/macOS
nmap -sn 192.168.1.0/24

# On Windows (if nmap is installed)
nmap -sn 192.168.1.0/24
```

## SSH Connection

### Windows

**Using built-in SSH client (Windows 10/11):**
1. Open Command Prompt (cmd) or PowerShell
2. Execute the command:
```cmd
ssh mks@192.168.1.100
```
Where `192.168.1.100` is your printer's IP address

**Alternative SSH clients:**
- **PuTTY** - popular SSH client: https://www.putty.org/
- **MobaXterm** - multifunctional terminal with SSH: https://mobaxterm.mobatek.net/

### macOS and Linux

Open terminal and execute:
```bash
ssh mks@192.168.1.100
```

### First Connection

On first connection, you'll see a warning about unknown host:
```
The authenticity of host '192.168.1.100 (192.168.1.100)' can't be established.
ECDSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Type `yes` and press Enter.

### Authorization

**Default credentials for QIDI printers with MKS board:**
- Username: `mks`
- Password: `makerbase`

**For other images with pre-installed Klipper:**
- Username: `pi` or `klipper`
- Password: usually specified in the image documentation

**Important:** When entering the password, characters are not displayed on screen - this is normal SSH behavior for security. Just type the password and press Enter.

## ⚠️ Important Warnings for QIDI Printers

**DO NOT UPDATE THE SYSTEM VIA APT!** QIDI printers use a modified version of Klipper and a specially configured system. Running `apt update` and `apt upgrade` commands may break the printer system. All updates should be performed only through official QIDI software.

## Basic Commands for Klipper Management

### File System Navigation
```bash
# Go to home directory
cd ~

# Go to Klipper configuration directory
cd ~/printer_data/config

# View directory contents
ls -la

# View current location
pwd
```

### Configuration File Management
```bash
# Edit main Klipper config
nano ~/printer_data/config/printer.cfg

# Create backup of config
cp ~/printer_data/config/printer.cfg ~/printer_data/config/printer.cfg.backup

# View Klipper logs
tail -f ~/printer_data/logs/klippy.log
```

### Service Management
```bash
# Restart Klipper
sudo systemctl restart klipper

# Check Klipper status
sudo systemctl status klipper

# Restart Moonraker
sudo systemctl restart moonraker

# Restart web interface (nginx)
sudo systemctl restart nginx
```

## File Transfer

### SCP (Secure Copy)
```bash
# Copy file from computer to printer
scp ~/Desktop/printer.cfg mks@192.168.1.100:~/printer_data/config/

# Copy file from printer to computer
scp mks@192.168.1.100:~/printer_data/logs/klippy.log ~/Desktop/
```

### Using SFTP
```bash
# Connect via SFTP
sftp mks@192.168.1.100

# SFTP session commands:
# ls - list files on remote server
# lls - list files on local computer
# get filename - download file
# put filename - upload file
# exit - quit
```

## Security
<details>
<summary>If you're really interested</summary>

### Security Recommendations
1. Change the default user password
2. Use SSH keys instead of passwords
3. Disable root login via SSH
4. Change the default SSH port (22) to another
5. Configure firewall to restrict access

### Changing SSH Port
```bash
# Edit SSH configuration
sudo nano /etc/ssh/sshd_config

# Find line #Port 22 and change to:
Port 2222

# Restart SSH service
sudo systemctl restart ssh
```

After this, connection is made with port specification:
```bash
ssh -p 2222 mks@192.168.1.100
```

## SSH Key Setup

For convenience and security, it's recommended to set up SSH key authentication:

### Key Generation
```bash
# On your computer
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

### Copy Public Key to Printer
```bash
ssh-copy-id mks@192.168.1.100
```

After this, you can connect without entering a password.
</details>

## Troubleshooting Common Issues

### Printer Not Responding via SSH
1. Check network connection: `ping 192.168.1.100`
2. Ensure SSH service is running on the printer
3. Verify the IP address is correct
4. Try connecting to a different port if SSH is configured non-standard

### Access Denied
1. Check username and password correctness
2. Ensure SSH service allows connections for this user
3. Check if your IP address is blocked

### Connection Drops
1. Check network connection stability
2. Configure keep-alive parameters in SSH client
3. Increase connection timeout

### Permission Issues
```bash
# Change file owner
sudo chown mks:mks filename

# Change file permissions
sudo chmod 755 filename

# Add user to group
sudo usermod -a -G dialout mks
```

## Useful Scripts

### Automatic Configuration Backup
```bash
#!/bin/bash
# backup_config.sh
DATE=$(date +%Y%m%d_%H%M%S)
cp ~/printer_data/config/printer.cfg ~/printer_data/config/backups/printer_${DATE}.cfg
echo "Backup created: printer_${DATE}.cfg"
```

### Quick Service Status Check
```bash
#!/bin/bash
# check_services.sh
echo "=== Klipper Status ==="
sudo systemctl status klipper --no-pager -l
echo "=== Moonraker Status ==="
sudo systemctl status moonraker --no-pager -l
echo "=== Nginx Status ==="
sudo systemctl status nginx --no-pager -l
```

## Conclusion

SSH is a powerful tool for managing Klipper printers on Armbian. Proper SSH setup and usage allows effective printer administration, quick problem solving, and keeping the system up to date. Remember the importance of following security measures when working with remote access.
