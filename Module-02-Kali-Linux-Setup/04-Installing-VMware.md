---
---

# Lesson 04: Installing VMware Workstation

## 1. Introduction

VMware Workstation is desktop virtualization software that allows users to create and run virtual machines on a Windows or Linux computer.

It enables users to operate multiple operating systems on one physical computer.

For this course, VMware will allow us to run Kali Linux inside Windows 11.

Our configuration:

- Host operating system: Windows 11 Pro.
- Virtualization software: VMware Workstation Pro.
- Guest operating system: Kali Linux.
- Future laboratory target: Metasploitable.

## 2. What Is VMware Workstation Pro?

VMware Workstation Pro is a Type 2 hypervisor.

It runs as an application on an existing operating system and provides virtual hardware to guest operating systems.

Its features include:

- Creating virtual machines.
- Allocating CPU and RAM.
- Creating virtual hard disks.
- Configuring virtual networks.
- Managing snapshots.
- Running multiple operating systems.

## 3. VMware Workstation Requirements

Before installation, check that your computer meets the requirements.

Recommended starting requirements for this course:

| Component | Recommendation |
|---|---|
| Host OS | Windows 11 64-bit |
| Processor | Intel or AMD 64-bit CPU |
| RAM | 16 GB recommended for our planned setup |
| Storage | Sufficient free SSD space |
| Virtualization | Intel VT-x or AMD-V enabled |
| Internet | Required for downloading software and updates |

Actual requirements depend on the guest operating systems and the number of virtual machines running simultaneously.

## 4. Downloading VMware Workstation

Always obtain VMware from the official Broadcom website.

Official website:

https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion

Broadcom support and downloads:

https://support.broadcom.com/

Steps:

1. Open your browser.
2. Visit the official VMware Workstation product page.
3. Locate the Workstation Pro download.
4. Sign in or create a Broadcom account if required by the download process.
5. Select the Windows installer.
6. Download the installer.
7. Wait for the download to complete.

Avoid downloading VMware installers from unofficial websites.

## 5. Verifying the Installer

Before installing the software, verify that the downloaded file came from the expected official source.

You can inspect the digital signature:

1. Open File Explorer.
2. Locate the downloaded VMware installer.
3. Right-click the file.
4. Select Properties.
5. Open the Digital Signatures tab, if available.
6. Inspect the signer and signature status.

You can also calculate the file's SHA-256 hash using PowerShell:

```powershell
Get-FileHash "C:\Path\To\VMware-Installer.exe" -Algorithm SHA256
```

Replace the example path with the actual installer path.

Compare the result against a checksum published by the official distributor, if one is provided.

A hash by itself does not prove authenticity; it must be compared with a trusted reference.

## 6. Installing VMware Workstation

After verifying the installer:

1. Double-click the downloaded installer.
2. Approve the Windows permission prompt if expected.
3. Follow the installation wizard.
4. Read and accept the license terms.
5. Keep the recommended installation settings unless you have a specific reason to change them.
6. Complete the installation.
7. Restart Windows if the installer requests it.

Do not disable Windows security features just to make the installation work.

## 7. Launching VMware Workstation

After installation:

1. Open the Windows Start menu.
2. Search for VMware Workstation.
3. Launch the application.
4. Review the interface.

You should see a virtual machine library or home screen where you can create and manage virtual machines.

## 8. Understanding the VMware Interface

Important interface components include:

### A. Library

Displays the virtual machines you have created.

### B. Home Screen

Provides access to options for creating or opening virtual machines.

### C. Virtual Machine Settings

Allows you to configure:
- CPU.
- Memory.
- Hard disk.
- Network adapter.
- Display.
- USB controller.

### D. Console

Displays the virtual machine's operating system.

### E. Snapshot Controls

Allows you to manage snapshots when the feature is available.

## 9. Understanding VMware Virtual Network Components

VMware may install virtual network adapters on Windows.

Common virtual networking components include:

- VMnet0: Commonly associated with bridged networking.
- VMnet1: Commonly associated with host-only networking.
- VMnet8: Commonly associated with NAT networking.

Exact configurations can vary.

For our future laboratory:
- NAT will be useful for Kali updates.
- Host-only will be useful for an isolated Kali and Metasploitable lab.

We will configure the networks in a later lesson.

## 10. Understanding VMware and Windows Security

Virtualization software needs access to hardware virtualization features.

Windows may also use virtualization-based features such as Hyper-V and Virtual Machine Platform.

Some VMware versions can coexist with Windows Hyper-V-based features using Windows' hypervisor platform, although performance and compatibility can vary.

If VMware displays a virtualization error:
- Read the exact error message.
- Check whether CPU virtualization is enabled.
- Check the VMware version.
- Review the relevant official VMware troubleshooting documentation.

Do not randomly disable Windows security or virtualization features.

## 11. Practical Lab: Install and Inspect VMware

### Task 1: Download VMware

Download VMware Workstation Pro from its official website.

### Task 2: Verify the installer

Inspect the digital signature and, where possible, calculate its SHA-256 hash.

### Task 3: Install VMware

Follow the installation wizard.

### Task 4: Launch VMware

Open VMware Workstation from the Windows Start menu.

### Task 5: Inspect the interface

Identify:
- The virtual machine library.
- The home screen.
- The virtual machine creation option.
- The application settings.

### Task 6: Confirm installation

Close VMware and reopen it to verify that it launches successfully.

Record:
- VMware version.
- Installation status.
- Any error messages.
- Whether VMware opens successfully.

Do not create the Kali virtual machine yet. That is the subject of a later lesson.

## 12. Exam-Ready Questions and Answers

**Q1. What is VMware Workstation?**

VMware Workstation is desktop virtualization software that allows users to create and operate virtual machines on a physical computer.

**Q2. What type of hypervisor is VMware Workstation?**

It is a Type 2 hosted hypervisor.

**Q3. What is the purpose of VMware Workstation in this course?**

It allows us to run Kali Linux as a guest operating system inside Windows 11.

**Q4. Mention four VMware features.**

Virtual machine creation, hardware allocation, virtual networking, and snapshot management.

**Q5. Why should VMware be downloaded from its official website?**

To reduce the risk of downloading modified, malicious, or outdated installers.

**Q6. What is the purpose of a VMware virtual network adapter?**

It allows virtual machines to communicate with the host, other virtual machines, or external networks according to the configured network mode.

**Q7. What is VMnet8 commonly used for?**

VMnet8 is commonly associated with VMware NAT networking.

**Q8. What is VMnet1 commonly used for?**

VMnet1 is commonly associated with VMware host-only networking.

## 13. Knowledge Check

Answer without looking at your notes.

1. What is VMware Workstation?
2. What type of hypervisor is VMware Workstation?
3. What is its purpose in our cybersecurity course?
4. Where should you download VMware?
5. What is a digital signature?
6. What is a SHA-256 hash?
7. What is the purpose of the VMware library?
8. What does VMnet8 commonly represent?
9. What does VMnet1 commonly represent?
10. Why should you avoid disabling Windows security features unnecessarily?

## 14. Lesson Summary

In this lesson, we studied:

- VMware Workstation.
- Hypervisor requirements.
- Downloading VMware safely.
- Digital signatures and SHA-256 verification.
- Installing VMware.
- Understanding the VMware interface.
- VMware virtual network components.
- Windows virtualization compatibility.
- Basic VMware installation procedures.

**Key takeaway:** VMware Workstation provides the virtual environment that will allow us to run Kali Linux safely alongside Windows 11.