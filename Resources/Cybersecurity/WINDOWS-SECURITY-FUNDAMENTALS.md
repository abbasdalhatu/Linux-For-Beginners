
# WINDOWS SECURITY FUNDAMENTALS

## 1. Introduction

Windows security involves protecting a computer, its files, applications, accounts, and network connections against threats.

Windows 11 includes several built-in security features.

## 2. Microsoft Defender Antivirus

Microsoft Defender Antivirus helps detect, block, and remove malware.

Its functions include:
- Real-time protection.
- Virus and threat scanning.
- Protection against malicious applications.
- Security intelligence updates.

Open it through:

Windows Security > Virus & threat protection.

## 3. Windows Firewall

Windows Defender Firewall monitors network connections and controls permitted traffic.

Its functions include:
- Blocking unauthorized connections.
- Controlling application network access.
- Protecting computers on public networks.
- Applying inbound and outbound rules.

Open it through:

Windows Security > Firewall & network protection.

## 4. User Account Control (UAC)

UAC helps prevent unauthorized changes to Windows.

It requests permission when an application needs administrator-level privileges.

Best practice:
- Use a standard account for everyday activities when practical.
- Approve administrator requests only when you understand the action.

## 5. Windows Updates

Updates help fix security vulnerabilities and improve system stability.

Open:

Settings > Windows Update.

Check for updates regularly and install important security updates.

## 6. Windows Security Commands

Open Windows CMD and run:

Display network configuration:

```cmd
ipconfig /all
```

Display active network connections:

```cmd
netstat -ano
```

Display running processes:

```cmd
tasklist
```

Check Windows system files:

```cmd
sfc /scannow
```

Note: Run CMD as administrator for commands that require elevated permissions.

## 7. BitLocker and Device Encryption

BitLocker and Device Encryption help protect data stored on a drive.

They use encryption to make stored information harder to access without the required recovery credentials.

Important:
- Keep recovery keys somewhere secure.
- Never share recovery keys publicly.
- Verify that you can access your recovery information before making major system changes.

Availability depends on the Windows edition and hardware.

## 8. Password and Account Security

Good account security includes:
- Using strong, unique passwords.
- Enabling multifactor authentication where available.
- Locking the computer when leaving it unattended.
- Avoiding unknown login links.
- Protecting administrator accounts.

## 9. Windows Security Logs

Windows Event Viewer records system and security events.

Open it by running:

```cmd
eventvwr.msc
```

Important log categories:
- Application.
- Security.
- Setup.
- System.

Logs can help investigate errors, login activity, and system events.

## 10. Practical Security Checklist

- [ ] Windows updates are enabled.
- [ ] Microsoft Defender protection is active.
- [ ] Firewall protection is enabled.
- [ ] Strong passwords are used.
- [ ] Important files are backed up.
- [ ] Unknown applications are avoided.
- [ ] Administrator privileges are used carefully.
- [ ] Recovery information is stored securely.

## 11. Summary

Windows security protects the operating system, accounts, files, and network connections.

Important features include Microsoft Defender Antivirus, Windows Firewall, User Account Control, Windows Update, and drive encryption.

Understanding these features is an important foundation for cybersecurity professionals.
