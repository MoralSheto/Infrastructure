# Proxmox: DR Replica VM Migration Guide

This procedure outlines the safe steps for migrating a Disaster Recovery (DR) replica virtual machine from a source server to a target server in Proxmox.

---

## Step 1: Replication Status Pre-Check

Before initiating the migration, verify that no active replication process will conflict with the operation:

1. Go to the **Notes** section of the original source VM.
2. Check the replication variable value: `repActive = on / off`.
3. If it is **off** (meaning replication is not currently running), it is safe to proceed.
4. On the original VM, update the note from `rep=on` to `rep=off`.

---

## Step 2: Migration Command

Run the following dynamic migration command (replace `vmID` with the VM id number and `kvTargetNip` with target host full name):

`qm migrate ${vmID} ${kvTargetNip} --targetstorage${targetStorage} && printf '\a'`

For example:
`qm migrate ${1577} ${kv0228-194-146-38-98} --targetstorage${targetStorage} && printf '\a'`


## Step 3: Fallback Offline Migration Command
If the dynamic command does not work or fails, use the direct offline migration command (example shown with specific IDs):

`qm migrate 1577 kv0228-194-146-38-98 --targetstorage kv0228_storage1`

## Troubleshooting: Non-Migratable Snapshots Error
If the migration fails with an error stating non-migratable snapshots, follow these steps to bypass the block:

1.Back up the VM configuration file (example using VMID 4488):
`cp /etc/pve/qemu-server/4488.conf /root/4488.conf.bak`

2.Strip snapshot entries from the configuration file:
`sed -i '/^\[/,$d' /etc/pve/qemu-server/4488.conf`

3.Run the migration command:
`qm migrate 1577 kv0228-194-146-38-98 --targetstorage kv0228_storage1`


