# Incident Report 001

## Summary

Multiple failed login attempts followed by successful authentication were observed.

A temporary user account was created and removed.

Several administrative commands were executed.

No evidence of malware or persistence mechanisms was identified.

---

## Timeline

09:00 - Failed Logon (4625)

09:02 - Successful Logon (4624)

09:05 - User Created (4720)

09:08 - Process Created (Sysmon Event 1)

09:12 - Network Connection (Sysmon Event 3)

---

## Indicators

- User: analyst

- Machine: WIN11-01

- Processes:

  - powershell.exe

  - notepad.exe

  - calc.exe

- Network:

  - google.com

---

## Conclusion

Activity was generated intentionally for SOC training.

No malicious activity detected.