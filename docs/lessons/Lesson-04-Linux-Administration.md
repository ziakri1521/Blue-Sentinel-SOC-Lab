# 🛡️ Lesson 04 – Linux Administration

## Blue Sentinel SOC Lab

**Phase:** 1 – Infrastructure Foundation  
**Lesson:** 04 – Linux Administration  
**Operating System:** Ubuntu Server 24.04.4 LTS  
**Hostname:** UBU-SRV-01  
**Author:** Ken Jie  
**Date:** _(Update with today's date)_

---

# Objective

The objective of this lesson is to build a strong Linux administration foundation by learning system management, networking, user administration, file permissions, service management, process monitoring, and log analysis.

These are essential skills for Security Operations Center (SOC) Analysts, Blue Team Engineers, and Linux System Administrators.

---

# Learning Objectives

By the end of this lesson, I was able to:

- Verify system health
- Understand Linux services
- Create and manage users
- Manage Linux groups
- Understand Linux file permissions
- Analyze Linux log files
- Monitor running processes
- Verify network connectivity
- Create a simple Bash script

---

# Lab Environment

| Component | Value |
|-----------|-------|
| Hypervisor | Oracle VirtualBox |
| Virtual Machine | UBU-SRV-01 |
| Operating System | Ubuntu Server 24.04.4 LTS |
| RAM | 4 GB |
| CPU | 2 vCPU |
| Disk | 40 GB Dynamic |
| Network | NAT |
| SSH | Enabled |

---

# Section 1 – System Health Checks

## Check Disk Usage

```bash
df -h
```

Purpose:
Displays mounted filesystems and available disk space.

Example Output

```
Filesystem      Size Used Avail Use% Mounted on
/dev/sda2        39G 6.1G 31G 17% /
```

---

## Check Memory Usage

```bash
free -h
```

Purpose:
Displays RAM and swap memory usage.

---

## Check CPU Information

```bash
lscpu
```

Purpose:
Displays processor architecture, cores, threads, and virtualization support.

---

## Check Uptime

```bash
uptime
```

Purpose:
Displays system uptime and system load.

---

## Display Host Information

```bash
hostnamectl
```

Purpose:
Displays hostname, operating system version, kernel version, and architecture.

---

# Section 2 – Service Management

Linux uses **systemd** to manage services.

## Check SSH Service

```bash
sudo systemctl status ssh
```

Purpose:
Verifies whether the SSH service is running.

Expected Status

```
Active: active (running)
```

---

## Start SSH

```bash
sudo systemctl start ssh
```

---

## Stop SSH

```bash
sudo systemctl stop ssh
```

---

## Enable SSH During Boot

```bash
sudo systemctl enable ssh
```

Purpose:
Ensures SSH starts automatically whenever the server boots.

---

# Section 3 – User Management

## Display Current User

```bash
whoami
```

---

## View Existing Users

```bash
cat /etc/passwd
```

Purpose:
Displays all local user accounts.

---

## Create a New User

```bash
sudo adduser analyst
```

Purpose:
Creates a new administrative user.

---

## Add User to sudo Group

```bash
sudo usermod -aG sudo analyst
```

Purpose:
Allows the user to execute administrative commands.

---

## Verify Group Membership

```bash
groups analyst
```

Expected Output

```
analyst : analyst sudo
```

---

# Section 4 – Linux Groups

Display current user groups

```bash
groups
```

Display all system groups

```bash
cat /etc/group
```

Purpose:
Linux uses groups to simplify permission management.

---

# Section 5 – File Permissions

Create a working directory

```bash
mkdir Lesson4
cd Lesson4
```

Create a file

```bash
touch report.txt
```

Display permissions

```bash
ls -l
```

Example

```
-rw-rw-r--
```

Permission Breakdown

| Symbol | Meaning |
|---------|----------|
| r | Read |
| w | Write |
| x | Execute |
| - | No Permission |

Permission Sets

Owner

```
rw-
```

Group

```
rw-
```

Others

```
r--
```

---

## Change Permissions

```bash
chmod 600 report.txt
```

Result

```
-rw-------
```

Only the owner has read and write permissions.

---

## Change Ownership

```bash
sudo chown analyst report.txt
```

Purpose:
Transfers ownership of the file to the analyst user.

---

# Section 6 – Linux Log Analysis

Navigate to the log directory

```bash
cd /var/log
```

Display log files

```bash
ls
```

Common log locations include:

- syslog (if present)
- auth.log (on some systems)
- dpkg.log
- apt/
- journal/

> **Note:** Ubuntu 24.04 primarily uses the systemd journal, so some traditional log files may not exist.

---

## View Recent Log Entries

```bash
sudo journalctl -n 20
```

Purpose:
Displays the latest 20 system log entries.

---

## Monitor Logs in Real Time

```bash
sudo journalctl -f
```

Purpose:
Monitors logs as they are generated.

Press:

```
CTRL + C
```

to stop monitoring.

---

## Search for SSH Events

```bash
sudo journalctl | grep ssh
```

Purpose:
Filters journal entries related to the SSH service.

---

# Section 7 – Networking

Display IP Address

```bash
ip addr
```

Display Routing Table

```bash
ip route
```

Display Listening Ports

```bash
ss -tuln
```

Test Internet Connectivity

```bash
ping 8.8.8.8
```

Test DNS Resolution

```bash
ping google.com
```

Purpose:
Confirms network connectivity and DNS functionality.

---

# Section 8 – Process Management

View running processes

```bash
ps aux
```

Interactive process viewer

```bash
htop
```

Exit

```
Q
```

Find SSH process

```bash
ps aux | grep ssh
```

Purpose:
Confirms that the SSH daemon is running.

---

# Section 9 – Bash Scripting

Create a script

```bash
nano system-info.sh
```

Contents

```bash
#!/bin/bash

echo "===================="
echo "Blue Sentinel SOC Lab"
echo "===================="

hostname

echo

hostname -I

echo

uptime

echo

free -h

echo

df -h
```

Make executable

```bash
chmod +x system-info.sh
```

Run

```bash
./system-info.sh
```

Purpose:
Introduces basic automation through shell scripting.

---

# Validation Checklist

| Task | Status |
|--------|--------|
| Ubuntu Updated | ✅ |
| SSH Running | ✅ |
| User Created | ✅ |
| sudo Configured | ✅ |
| File Permissions Tested | ✅ |
| Ownership Changed | ✅ |
| Networking Verified | ✅ |
| Log Analysis Completed | ✅ |
| Bash Script Created | ✅ |

---

# Lessons Learned

- Linux uses **systemd** to manage services.
- User privileges are controlled using users and groups.
- File permissions determine who can access files.
- Most modern Ubuntu systems use **journalctl** instead of traditional log files.
- SSH enables secure remote administration.
- Monitoring disk space, memory, and processes is an important administrative task.
- Bash scripting helps automate repetitive tasks.

---

# Troubleshooting Notes

### Issue

SSH service not running.

### Resolution

```bash
sudo systemctl start ssh
sudo systemctl enable ssh
```

---

### Issue

Unable to monitor logs using `/var/log/syslog`.

### Resolution

Ubuntu 24.04 stores many logs in the **systemd journal**.

Use:

```bash
sudo journalctl -f
```

instead.

---

# SOC Relevance

Understanding Linux administration is essential for Blue Team operations.

SOC Analysts frequently perform the following tasks:

- Review authentication logs
- Verify running services
- Investigate suspicious processes
- Check listening ports
- Monitor system resource utilization
- Validate user accounts
- Analyze system logs
- Perform initial incident triage

These skills form the foundation for future lessons involving Wazuh SIEM, Sysmon, Active Directory, and Threat Hunting.

---

# Next Lesson

**Lesson 05 – Windows 11 Endpoint Deployment**

Topics include:

- Windows installation
- Windows networking
- Windows Event Viewer
- User management
- Windows services
- Preparing the endpoint for Sysmon and Wazuh Agent installation

---

**Status:** ✅ Lesson 04 Completed