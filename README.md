# Project Atlas: Home Infrastructure & Networking Lab

Project Atlas is my personal homelab I built to develop practical infrastructure, networking and Linux skills. It runs Proxmox VE on a Dell OptiPlex hosting an Ubuntu Docker VM (Immich, Vaultwarden, Uptime Kuma), with Pi-hole on a Raspberry Pi providing DNS and DHCP and Tailscale for remote administration.

This is homelab experience, not production work. The repository documents what is implemented, the design decisions behind it, and problems I diagnosed along the way. All my future planned work is kept in a separate Roadmap section.

> IP addresses, host identifiers and other infrastructure details shown in this documentation have been sanitised from the live environment.

## Architecture

## DNS and DHCP

See [docs/dns-dhcp.md](docs/dns-dhcp.md).

See [docs/architecture.md](docs/architecture.md).

## Roadmap (planned, not implemented)

Looking into implementing Reverse proxy, VLAN segmentation, OPNsense, off-host backups, monitoring expansion, Ansible.
