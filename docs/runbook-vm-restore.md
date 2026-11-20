# Runbook: Proxmox VM backup and test restore

Identifiers are generalised. No live addresses are shown.

## Purpose
Prove that a VM backup can be restored, and record what a restore actually gives back.

## Prerequisites
- A directory storage that accepts backups, separate from the VM's disks.
- Enough free space: roughly the size of the written data on the backed-up disks.
- A free VM ID for the test restore.

## Backup
1. Add a backup-only directory storage with a retention rule (keep the last 2).
2. Guard it with a mountpoint check, so a missing disk causes a failed job instead of writing to the OS disk.
3. Back up the VM in snapshot mode with ZSTD compression (the guest agent freezes the filesystem briefly).

## Restore test
1. Restore to a new VM ID with the "unique" option, so a new MAC address is generated.
2. Rename the restored VM (for example `restore-test`) so it is never confused with the original.
3. Inspect its config before the first boot: no disk may point at the original VM's disks.
4. Disconnect its virtual NIC before the first boot, to avoid duplicate hostnames, MACs and addresses.
5. Boot it and observe from the console.
6. Stop and destroy the test VM when finished.

## Result of the first test
- The OS disk restored and booted until it waited for the data disk, which was excluded from the backup.
- After the 90-second timeout, the boot stopped in emergency mode.
- Conclusion: an OS-disk-only backup proves the process, but does not restore the services.

## Lessons
- A backup is not proven until it has been restored.
- The restore plan must include the data disks, or a way to re-attach them.
- Isolate a restored VM from the network before its first boot.
- Whether the data disk's mount is required or optional (`nofail`) decides whether a restore boots without it. Choose this deliberately.

## Limitations
- Backups are on the same physical drive as the live data, so they do not protect against drive failure.
- The data disk exclusion is temporary.
