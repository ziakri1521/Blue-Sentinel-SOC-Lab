# Lesson 07 - Microsoft Sysmon

## Objective

Install Sysmon and understand enhanced Windows endpoint logging.

## Installation

- Sysmon64.exe
- SwiftOnSecurity configuration

## Event IDs Investigated

- 1
- 3
- 11
- 22

## Commands Executed

```powershell
.\Sysmon64.exe -accepteula -i .\sysmonconfig-export.xml
Get-Service Sysmon64
```

## Validation

- Sysmon service running
- Event ID 1 generated
- Event ID 3 generated
- Logs visible in Event Viewer

## Lessons Learned

(Add your observations.)