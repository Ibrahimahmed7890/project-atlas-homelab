# DNS and DHCP: migrating DHCP from the ISP router to Pi-hole

Addresses are sanitised and do not match the live network.

## Summary

DHCP was moved from the Virgin Media Hub 3 to Pi-hole running on a Raspberry Pi 4. Pi-hole already provided DNS, and the Hub did not expose enough control over the DNS servers handed out to clients. Running both services on Pi-hole lets every client use Pi-hole for filtering, query logging and local name resolution.

## Current state

| Role | Device | Address |
|---|---|---|
| Router / gateway | Virgin Media Hub 3 | 192.168.10.1 |
| DNS + DHCP | Raspberry Pi 4 (Pi-hole) | 192.168.10.53 |
| Hypervisor | Proxmox VE | 192.168.10.10 |
| Docker VM | Ubuntu Server | 192.168.10.20 |

- DHCP pool: 192.168.10.100 to 192.168.10.199. Infrastructure addresses sit outside the pool.
- Clients receive gateway 192.168.10.1 and DNS 192.168.10.53.
- Proxmox and the Pi use static addresses. The Docker VM uses DHCP with a reservation.
- Local DNS records use the `home.arpa` domain.
- Upstream DNS: Cloudflare, with DNSSEC validation.

## Change record

**What changed:** DHCP service moved from the Hub to Pi-hole.

**Why:** the Hub could not hand out a custom DNS server, so clients bypassed Pi-hole.

**Risk:** if the Pi failed, clients could not get new leases or resolve names, and I could be locked out of the network.

**Preparation (before the change):**
- Gave the Pi a static address configured on the Pi itself, so it does not depend on DHCP.
- Prepared an emergency laptop configuration (static IP in the LAN range, gateway set to the Hub, public DNS) to reach the Hub's admin page and re-enable its DHCP if needed.
- Confirmed the Teleporter backup of the Pi-hole configuration was saved off the Pi.

**Implementation:**
1. Configured the pool, gateway and DNS settings in Pi-hole.
2. Disabled DHCP on the Hub.
3. Enabled DHCP on Pi-hole.
4. Added a reservation so the Docker VM keeps its address.

**Validation:** from a Windows client, confirmed that it received a new lease with the DHCP server and DNS server both set to the Pi-hole address, the default gateway remained the Hub, internet access worked, and local names resolved.

**Rollback:** disable DHCP on Pi-hole and re-enable it on the Hub using the emergency laptop configuration. Existing leases remain valid until they expire, so disabling a DHCP server does not remove clients' addresses immediately.

## DNS record types used

| Type | Maps | Example use |
|---|---|---|
| A | hostname to IPv4 | `proxmox.home.arpa` to the Proxmox address |
| AAAA | hostname to IPv6 | not currently used |
| PTR | IP to hostname | reverse lookups |
| CNAME | alias to another name | aliases for services |

An A record and its PTR record are separate pieces of data, so one does not create the other.

I used `home.arpa` because it is reserved for home networks. `.local` is avoided because it is used by mDNS.

## Lessons learned

- Plan the recovery method before the change, not after.
- Static addressing and DHCP reservations serve different purposes: static is set on the device, a reservation is set on the DHCP server.
- Test from a real client, not just from the server's own dashboard.

## Limitations

- Pi-hole is a single point of failure for DNS and DHCP.
- The Pi currently connects over Wi-Fi. Moving it to Ethernet is planned (not yet implemented).
