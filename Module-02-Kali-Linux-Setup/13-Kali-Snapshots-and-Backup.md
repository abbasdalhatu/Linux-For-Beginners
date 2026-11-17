---
---

# LESSON 13: KALI SNAPSHOTS AND BACKUP

**Module:** 02 – Kali Linux Setup  
**File:** `13-Kali-Snapshots-and-Backup.md`

## 1. Introduction

During Linux administration and ethical hacking, we frequently make changes to our operating system.

Some changes may cause problems, such as:

- Installing incompatible packages.
- Changing system configurations incorrectly.
- Removing important files.
- Breaking network settings.
- Making mistakes during security experiments.

To protect our environment, we need to understand snapshots and backups.

VMware provides a snapshot feature that allows us to save the state of a virtual machine at a particular moment.

## 2. Understanding a Virtual Machine Snapshot

A snapshot captures the state of a virtual machine at a particular point in time.

Depending on its configuration, it can preserve:

- Virtual disk state.
- VM configuration.
- Memory state, if the snapshot includes the VM's memory.

A snapshot allows us to return to an earlier state if something goes wrong.

### Example

Imagine your Kali VM is working correctly.

You decide to install several packages, but the installation causes problems.

If you created a snapshot beforehand, you may be able to revert to that earlier state.

## 3. Understanding Snapshots vs Backups

These two concepts are related, but they are not the same.

| Snapshot | Backup |
|---|---|
| Captures a VM state | Creates a separate recoverable copy |
| Useful for short-term rollback | Useful for long-term recovery |
| Usually depends on the original VM files | Can be stored independently |
| Can consume additional storage | Also requires storage |
| Not a substitute for backup | Provides protection against data loss |

**Important:** A snapshot stored on the same physical SSD as the original VM may be lost if that SSD fails.

## 4. When Should We Create a Snapshot?

Useful moments include:

1. Immediately after a successful Kali installation.
2. After initial configuration.
3. Before a major system upgrade.
4. Before installing unfamiliar software.
5. Before performing controlled security experiments.
6. Before changing important system configurations.

We will create a clean baseline snapshot now.

## 5. Create Your First VMware Snapshot

### Step 1: Shut Down Kali

Inside Kali, run:

```bash
sudo poweroff
```

Wait until the virtual machine is completely powered off.

### Step 2: Open VMware

1. Open VMware Workstation.
2. Select `Kali-Linux`.
3. Open the Snapshot menu or Snapshot Manager.
4. Select Take Snapshot.

The exact menu location may vary between VMware versions.

### Step 3: Name the Snapshot

Use:

`Kali-Clean-Baseline`

Description:

`Clean Kali installation after initial configuration, networking, and package updates.`

### Step 4: Create the Snapshot

Click Take Snapshot and wait for VMware to finish.

Do not interrupt the process.

## 6. Understanding Snapshot Management

VMware provides a Snapshot Manager for viewing and managing snapshots.

You can use it to:

- View existing snapshots.
- Create additional snapshots.
- Revert to an earlier snapshot.
- Delete snapshots.

Deleting a snapshot is not the same as deleting the entire virtual machine.

VMware may consolidate snapshot data into the VM's current disk state when snapshots are deleted.

Always read the confirmation messages before proceeding.

## 7. Reverting to a Snapshot

Reverting means returning the VM to a previously saved snapshot state.

For example:

1. Create a snapshot.
2. Start Kali.
3. Make a test configuration change.
4. Shut down or power off the VM as appropriate.
5. Open Snapshot Manager.
6. Select the saved snapshot.
7. Choose Revert.

After reverting, changes made after that snapshot may no longer be present in the active VM state.

**Warning:** Do not perform a revert casually. Save important work first because later changes can be lost.

## 8. Creating a Separate Backup

A proper backup should be stored independently of the original VM.

For a powered-off Kali VM, a practical manual backup method is:

1. Shut down Kali completely.
2. Close VMware or ensure the VM is no longer running.
3. Locate the VM's folder in Windows.
4. Copy the entire VM folder.
5. Store the copy on a separate external drive or another suitable storage location.

Do not copy only the main virtual disk if you need a complete VM backup.

The copied folder should contain the files needed by the VM, such as its configuration and virtual disk files.

Ensure that you have enough storage before copying.

## 9. Important VMware File Types

VMware virtual machines may contain several file types.

| Extension | Purpose |
|---|---|
| `.vmx` | Virtual machine configuration |
| `.vmdk` | Virtual disk file |
| `.nvram` | Virtual firmware settings |
| `.vmsd` | Snapshot metadata |
| `.vmsn` | Snapshot state information |

Not every VM will contain all these files.

Do not manually delete VMware files because they may be required by the virtual machine or its snapshots.

## 10. Snapshot Safety Rules

Follow these rules:

1. Create snapshots before major changes.
2. Avoid keeping too many unnecessary snapshots.
3. Monitor available disk space.
4. Do not manually delete snapshot files.
5. Keep independent backups of important data.
6. Shut down the VM before making a manual folder backup.
7. Remember that snapshots are not substitutes for backups.

## 11. Practical Assignment

Complete these tasks:

- [ ] Shut down Kali Linux.
- [ ] Open VMware Workstation.
- [ ] Open Snapshot Manager.
- [ ] Create a snapshot named `Kali-Clean-Baseline`.
- [ ] Add a description.
- [ ] Confirm that the snapshot appears in Snapshot Manager.
- [ ] Inspect the VM folder in Windows without deleting or modifying files.
- [ ] Identify the `.vmx` and `.vmdk` files if visible.
- [ ] Check the available Windows storage space.

Do not revert or delete the snapshot during this exercise.

## 12. Knowledge Check

1. What is a virtual machine snapshot?
2. What is the difference between a snapshot and a backup?
3. Why should we create a snapshot before a major system upgrade?
4. What does reverting to a snapshot mean?
5. What is the purpose of a `.vmx` file?
6. What is the purpose of a `.vmdk` file?
7. Why should we avoid manually deleting snapshot files?
8. Why should a proper backup be stored separately?

## 13. Lesson Summary

In this lesson, we learned how to protect our Kali virtual machine using VMware snapshots and independent backups.

We explored snapshot creation, snapshot management, reverting, VMware file types, and backup safety.

Snapshots will be especially useful when we begin performing controlled ethical hacking experiments.

**Next Lesson:** Lesson 14 – Kali Linux Troubleshooting.