# Project Atlas: Home Infrastructure & Networking Lab

Project Atlas is a personal homelab I built to develop practical infrastructure, networking and Linux skills. It runs Proxmox VE on a Dell OptiPlex hosting an Ubuntu Docker VM (Immich, Vaultwarden, Uptime Kuma), with Pi-hole on a Raspberry Pi providing DNS and DHCP and Tailscale for remote administration.

This is homelab experience, not production work. The repository documents what is implemented, the design decisions behind it, and problems I diagnosed along the way. Planned work is kept in a separate Roadmap section.

> IP addresses, host identifiers and other infrastructure details shown in this documentation have been sanitised from the live environment.

## Documentation

- [Architecture](docs/architecture.md)
- [DNS and DHCP](docs/dns-dhcp.md)
- [Storage](docs/storage.md)
- [Backups](docs/backups.md)
- [Troubleshooting](docs/troubleshooting.md)
- [Changelog](CHANGELOG.md)

## Roadmap (planned, not implemented)

Reverse proxy, VLAN segmentation, OPNsense, off-host backups, monitoring expansion, Ansible.
