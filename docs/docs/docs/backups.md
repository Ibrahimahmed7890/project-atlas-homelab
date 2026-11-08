# Backups

This page separates what is implemented from what is planned.

## Implemented

| What | Method | Where it is stored |
|---|---|---|
| Immich | Daily script: database dump plus file copy, run by a systemd timer (about 03:00, catches up after downtime) | Same physical host |
| Vaultwarden | Backup archives created manually | Same physical host |
| Pi-hole configuration | Teleporter export | Saved off the Pi |

Original Immich photo files were verified on disk.

## Limitations (honest assessment)

- Most application backups are on the same physical machine as the data they protect. They help with mistakes and application problems, but not with a host or drive failure.
- Proxmox VM-level backups are not yet configured.
- There is no off-site copy.
- A full restore test has not yet been completed for every service.

## Planned (not implemented)

- Proxmox VM backups to a dedicated backup target, with a retention policy and a test restore.
- A second storage system for off-host copies.
- Off-site backup, working towards a 3-2-1 approach.
- Regular restore testing.

RAID is not a backup. Redundancy protects against drive failure, while backups protect against deletion, corruption and loss of the whole system.
