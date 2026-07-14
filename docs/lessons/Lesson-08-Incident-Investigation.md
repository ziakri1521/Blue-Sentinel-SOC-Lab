# 🛡️ Lesson 08 – Incident Investigation

## Blue Sentinel SOC Lab

**Phase:** 2 – Windows Security  
**Lesson:** 08 – Attack Simulation & Incident Investigation  
**Operating System:** Windows 11 Enterprise Evaluation  
**Hostname:** WIN11-01  
**Author:** Ken Jie  
**Date:** _(Update with today's date)_

---

# Objective

The objective of this lesson is to simulate common Windows activities that generate security events and investigate those events using Windows Event Viewer and Microsoft Sysmon.

This lesson introduces the workflow of a Security Operations Center (SOC) analyst by correlating user activity with Windows Security Logs and Sysmon telemetry.

---

# Learning Objectives

By completing this lesson, I was able to:

- Generate Windows Security Events
- Generate Microsoft Sysmon Events
- Investigate authentication events
- Investigate process creation events
- Investigate PowerShell activity
- Investigate network activity
- Build an incident timeline
- Identify Indicators of Compromise (IOCs)
- Create an Incident Report

---

# Lab Environment

| Component | Value |
|-----------|-------|
| Hypervisor | Oracle VirtualBox |
| Endpoint | WIN11-01 |
| Operating System | Windows 11 Enterprise Evaluation |
| Monitoring | Windows Event Viewer |
| Endpoint Monitoring | Microsoft Sysmon |
| User | analyst |

---

# Investigation 1 – Failed Login Attempts

## Objective

Generate failed authentication attempts.

## Activity

Locked the workstation and attempted to log in with an incorrect password five times.

## Event Viewer

```
Windows Logs
↓
Security
```

Filtered Event ID

```
4625
```

## Evidence Collected

| Item | Observation |
|------|-------------|
| Username | analyst |
| Event ID | 4625 |
| Description | Failed Logon |
| Time | _(Record actual time)_ |
| Logon Type | _(Record value)_ |

---

# Investigation 2 – Successful Login

Filtered

```
4624
```

## Evidence

| Item | Observation |
|------|-------------|
| Username | analyst |
| Event ID | 4624 |
| Authentication Package | _(Example: Negotiate)_ |
| Workstation | WIN11-01 |
| Logon Type | _(Example: 2)_ |

---

# Investigation 3 – User Creation

## Activity

Executed:

```powershell
New-LocalUser TempAdmin -Password (Read-Host -AsSecureString)
```

Observed Event ID

```
4720
```

### Evidence

| Item | Observation |
|------|-------------|
| User Created | TempAdmin |
| Created By | analyst |
| Machine | WIN11-01 |

---

Deleted the account:

```powershell
Remove-LocalUser TempAdmin
```

Observed

```
4726
```

---

# Investigation 4 – Process Creation

Executed

```cmd
notepad
calc
mspaint
```

Investigated

```
Sysmon Event ID 1
```

## Information Collected

- Image
- Parent Image
- Process ID
- Command Line
- SHA256 Hash
- User

---

# Investigation 5 – PowerShell Activity

Executed

```powershell
Get-Process

Get-Service

Get-ChildItem C:\
```

Observed

```
Sysmon Event ID 1
```

## Findings

PowerShell execution was successfully logged by Sysmon.

Command line information was available.

Parent-child process relationship was recorded.

---

# Investigation 6 – Network Activity

Executed

```powershell
ping google.com
```

Executed

```powershell
curl https://example.com
```

Observed

```
Sysmon Event ID 3
```

## Evidence

| Field | Observation |
|--------|-------------|
| Process | powershell.exe |
| Destination | _(Record IP)_ |
| Destination Port | _(Record Port)_ |
| User | analyst |

---

# Investigation 7 – Scheduled Task

Created

```powershell
schtasks /create /tn "BlueSentinelTest" /tr "notepad.exe" /sc once /st 23:59
```

Observed

```
4698
```

## Findings

The scheduled task creation was recorded successfully.

Deleted using

```powershell
schtasks /delete /tn "BlueSentinelTest" /f
```

---

# Investigation 8 – Windows Defender

Reviewed

- Virus & Threat Protection
- Protection History
- Firewall Status

No malicious activity detected.

---

# Timeline

| Time | Event | Event ID |
|------|---------|---------|
| _(Time)_ | Failed Login | 4625 |
| _(Time)_ | Successful Login | 4624 |
| _(Time)_ | User Created | 4720 |
| _(Time)_ | User Deleted | 4726 |
| _(Time)_ | Notepad Executed | Sysmon Event 1 |
| _(Time)_ | PowerShell Executed | Sysmon Event 1 |
| _(Time)_ | Network Connection | Sysmon Event 3 |
| _(Time)_ | Scheduled Task Created | 4698 |

---

# Indicators of Compromise (Training Scenario)

Although the activities were intentionally generated for training, the following artifacts could be considered Indicators of Compromise (IOCs) during a real investigation:

## User Accounts

- analyst
- TempAdmin

## Processes

- powershell.exe
- cmd.exe
- notepad.exe
- calc.exe
- mspaint.exe

## Network Activity

- google.com
- example.com

## Event IDs

- 4624
- 4625
- 4698
- 4720
- 4726
- Sysmon Event ID 1
- Sysmon Event ID 3

---

# Lessons Learned

This lesson demonstrated how Windows Event Viewer and Sysmon complement each other during incident investigations.

Windows Security Logs provide authentication, account management, and audit events, while Sysmon provides detailed endpoint telemetry such as process creation, command-line arguments, and network connections.

Understanding both log sources is essential before integrating them into a SIEM such as Wazuh.

---

# Challenges Encountered

## Issue

Example:

Unable to locate Event ID 4698 immediately.

## Resolution

Verified that the Scheduled Task was successfully created before checking Event Viewer.

---

# Validation Checklist

| Task | Status |
|--------|--------|
| Failed Login Generated | ✅ |
| Successful Login Generated | ✅ |
| User Created | ✅ |
| User Deleted | ✅ |
| Process Creation Logged | ✅ |
| PowerShell Logged | ✅ |
| Network Activity Logged | ✅ |
| Scheduled Task Logged | ✅ |
| Timeline Created | ✅ |
| Incident Report Created | ✅ |

---

# Screenshots

Store screenshots under:

```
screenshots/
└── lesson-08/
```

Recommended screenshots:

- Event Viewer Security Log
- Event ID 4624
- Event ID 4625
- Event ID 4720
- Sysmon Event ID 1
- Sysmon Event ID 3
- PowerShell Commands
- Scheduled Task
- Timeline
- Incident Report

---

# SOC Analyst Notes

During this lesson I practiced the initial workflow of a Security Operations Center analyst:

1. Generate activity.
2. Identify related Windows Event IDs.
3. Correlate Sysmon events.
4. Build an event timeline.
5. Identify Indicators of Compromise.
6. Document findings.
7. Produce an incident report.

These activities mirror the first steps performed by Tier 1 SOC Analysts when investigating endpoint alerts.

---

# Next Lesson

**Phase 3 – Lesson 09**

Deploy **Wazuh SIEM** on **UBU-SRV-01** and onboard **WIN11-01** as the first monitored endpoint.

After Lesson 09, all Windows and Sysmon events will be centrally collected and analyzed through the Wazuh Dashboard instead of manually using Event Viewer.

---

**Status:** ✅ Lesson 08 Completed