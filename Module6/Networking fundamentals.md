# Linux System Administration & DevOps — Interview Cheat Sheet

Quick-recall reference covering networking, RHEL storage/security, package management, and monitoring/proxy tools.

---

## 1. Deprecated Linux Commands → Replacements

| Old (Deprecated) | New | Purpose |
|---|---|---|
| `ifconfig -a` | `ip a` | Show all interfaces |
| `ifconfig enp6s0 down/up` | `ip link set enp6s0 down/up` | Bring interface down/up |
| `ifconfig enp6s0 192.168.2.24` | `ip addr add 192.168.2.24/24 dev enp6s0` | Assign IP |
| `ifconfig enp6s0 netmask ...` | `ip addr add <ip>/24 dev enp6s0` | Assign IP+mask (CIDR) |
| `ifconfig enp6s0 mtu 9000` | `ip link set enp6s0 mtu 9000` | Set MTU |
| `ifconfig enp6s0:0 ...` (alias) | `ip addr add <ip>/24 dev enp6s0` | Secondary IP (no more aliases, just add another addr) |
| `netstat` | `ss` | Socket stats |
| `netstat -tulpn` | `ss -tulpn` | Listening ports + PID |
| `netstat -neopa` | `ss -neopa` | Extended socket info |
| `netstat -g` | `ip maddr` | Multicast group membership |
| `route` | `ip r` | Show routing table |
| `route add -net ... netmask ...` | `ip route add <net>/24 dev <if>` | Add static route |
| `route add default gw ...` | `ip route add default via <gw>` | Add default gateway |
| `arp -a` | `ip neigh` | Show ARP/neighbor table |
| `arp -v` | `ip -s neigh` | Verbose neighbor stats |
| `arp -s <ip> <mac>` | `ip neigh add <ip> lladdr <mac> dev <if>` | Add static ARP entry |
| `arp -i <if> -d <ip>` | `ip neigh del <ip> dev <if>` | Delete ARP entry |

**Interview tip:** Know this cold — `ifconfig`/`route`/`netstat`/`arp` (net-tools) are all deprecated in favor of `ip` and `ss` (iproute2) on modern RHEL 7/8/9. Be ready to explain *why*: net-tools is unmaintained upstream, iproute2 is more powerful (single tool for addresses, routes, links, neighbors, tunnels).

---

## 2. Core Networking Concepts

- **NIC**: physical/virtual point of connection to a network.
- **IP address**: logical identifier; **subnet mask**: defines network vs. host portion.
- **Default gateway**: router used when destination isn't in local routing table.
- **MAC address**: 48-bit hardware address, format `MM:MM:MM:SS:SS:SS`.
- **Static vs DHCP IP**: static = fixed, manually configured; DHCP = leased/dynamic, can change on reboot.
- **LAN / MAN / WAN**: single site / metro-region / country-to-country scope.

**Key config files (RHEL):**
- `/etc/sysconfig/network-scripts/ifcfg-<if>` — per-interface config (legacy) / NetworkManager keyfiles under `/etc/NetworkManager/system-connections/` (RHEL 8+)
- `/etc/hosts` — static hostname resolution
- `/etc/resolv.conf` — DNS resolvers
- `/etc/nsswitch.conf` — name resolution order

**Core diagnostic commands:**
- `ping` — reachability / latency / packet loss
- `traceroute` — hop-by-hop path to destination
- `tcpdump -i eth0` — packet capture
- `ss -tulpn` — listening ports & owning process
- `nmcli` / `nmtui` — NetworkManager CLI/TUI (modern RHEL way to manage connections)

**Legacy remote-access tools (know these exist, and why they're avoided):**
- `telnet`, `ftp`, `rlogin`, `rsh`, `rcp` — all send credentials/data in **cleartext**; replaced by `ssh`, `sftp`, `scp` respectively. `.rhosts`-based trust (rsh/rlogin/rcp) is a classic security anti-pattern — expect this as an interview question.

---

## 3. NIC Bonding (Link Aggregation)

- Load driver: `modprobe bonding`; check: `modinfo bonding`
- Bond interface file: `/etc/sysconfig/network-scripts/ifcfg-bond0`
  ```
  DEVICE=bond0
  TYPE=Bond
  BONDING_MASTER=yes
  BOOTPROTO=none
  ONBOOT=yes
  IPADDR=192.168.1.80
  NETMASK=255.255.255.0
  GATEWAY=192.168.1.1
  BONDING_OPTS="mode=5 miimon=100"
  ```
- Slave interfaces (`ifcfg-enp0s3`, `ifcfg-enp0s8`): `MASTER=bond0`, `SLAVE=yes`, `BOOTPROTO=none`.
- Verify: `cat /proc/net/bonding/bond0`
- Restart networking: `systemctl restart network` (or `NetworkManager` on newer RHEL).

**Bonding modes (memorize the numbers + fault tolerance/load balancing):**

| Mode | Name | Fault Tolerance | Load Balancing |
|---|---|---|---|
| 0 | Round Robin | No | Yes |
| 1 | Active-Backup | Yes | No |
| 2 | XOR | Yes | Yes |
| 3 | Broadcast | Yes | No |
| 4 | 802.3ad (LACP/Dynamic Link Aggregation) | Yes | Yes (needs switch support) |
| 5 | TLB (Transmit Load Balancing) | Yes | Yes |
| 6 | ALB (Adaptive Load Balancing) | Yes | Yes (no special switch config needed) |

**Common interview question:** "Which bonding mode did you use in production and why?" — Mode 4 (LACP/802.3ad) is the most common enterprise answer since it needs switch-side configuration and gives true aggregation + failover; mode 1 (active-backup) is simplest and switch-agnostic.

---

## 4. Package Management — YUM/DNF & RPM

### RPM (low-level, single package, no dependency resolution)
| Command | Action |
|---|---|
| `rpm -ivh pkg.rpm` | Install |
| `rpm -Uvh pkg.rpm` | Upgrade |
| `rpm -ev pkg` | Erase/remove |
| `rpm -ev --nodeps pkg` | Remove ignoring dependencies |
| `rpm -qa` | List all installed packages |
| `rpm -qi pkg` | Package info |
| `rpm -qf /path/to/file` | Find which package owns a file |
| `rpm -qc pkg` | List config files of a package |
| `rpm -qR pkg` | List dependencies |

### YUM/DNF (high-level, repo-aware, resolves dependencies)
| Task | Command |
|---|---|
| Install | `yum install <pkg>` |
| Remove + deps | `yum remove <pkg>` |
| Update all | `yum update` |
| Security updates only | `yum update --security` |
| Search | `yum search <term>` |
| Package info | `yum info <pkg>` |
| List repos | `yum repolist` |
| Clean cache | `yum clean all` |
| Local/offline install | `yum localinstall pkg.rpm` |
| History (audit/undo) | `yum history list` / `yum history undo <id>` |
| Downgrade | `yum downgrade <pkg>` |
| Group install | `yum groupinstall "Web Server"` |
| Find which repo | `find-repos-of-install` (yum-utils) |
| Reverse dependency query | `repoquery --requires --resolve <pkg>` |

**Building a local repo** (offline/air-gapped environments):
1. Mount media / stage RPMs into a directory (e.g., `/tmp/localrepo`)
2. `createrepo -v /tmp/localrepo/`
3. Create `/etc/yum.repos.d/localrepo.repo` with `baseurl=file:///tmp/localrepo`
4. `yum clean all && yum repolist`

**Interview angle:** RPM = "dpkg"-equivalent (no dependency resolution); YUM/DNF = "apt"-equivalent (dependency resolution, repo management). DNF replaces YUM as of RHEL 8+ (same command syntax, newer backend).

---

## 5. SELinux

- Mandatory Access Control layer (kernel-integrated via LSM), developed jointly by NSA/Red Hat.
- Config file: `/etc/sysconfig/selinux` (symlinked to `/etc/selinux/config`)
- **Modes:**
  - `enforcing` — policy is enforced, violations blocked & logged
  - `permissive` — violations logged only, not blocked (debugging)
  - `disabled` — SELinux fully off
- Check status: `sestatus`, `getenforce`
- Change mode at runtime: `setenforce 0|1` (permissive/enforcing — doesn't persist reboot)
- File/process context: `ls -Z`, `ps -Z`
- Useful commands: `chcon` (temp context change), `semanage` (persistent policy mgmt), `restorecon`/`fixfiles` (reset to default context), `getsebool`/`setsebool` (toggle booleans), `audit2allow` (build custom policy from denials)
- Logs: `/var/log/audit/audit.log`, `ausearch -m avc`

**Common interview question:** "App works in permissive but fails in enforcing — how do you troubleshoot?" → check `/var/log/audit/audit.log` for AVC denials, use `audit2allow -a` to generate a suggested policy module, or fix with proper `semanage fcontext`/`restorecon` rather than just disabling SELinux (disabling is a red flag answer in interviews — shows you don't understand least-privilege).

---

## 6. OS Hardening / Security Checklist

Standard hardening checklist — good as a structured interview answer for "how do you secure a Linux server":

1. **User accounts**: naming convention, UID ranges, `chage -l <user>` for password aging, edit `/etc/login.defs` for defaults, disable password reuse via `/etc/pam.d/system-auth`.
2. **Least footprint**: remove unwanted packages, don't install more than needed.
3. **Disable unused services**: `systemctl -a` to audit; disable legacy services like telnet, ftp, NFS if unneeded.
4. **Check listening ports**: `ss -tulpn` (formerly `netstat -tunlp`).
5. **Harden SSH**: disable direct root login (`PermitRootLogin no`), change default port, key-based auth over passwords.
6. **Firewall**: `firewalld` (modern) or `iptables` (legacy) — `firewall-cmd`, `firewall-config` (GUI).
7. **SELinux**: keep enforcing wherever possible (see above).
8. **Non-standard ports** for exposed services where practical.
9. **Patching**: keep OS current (`yum update --security`).

---

## 7. RHEL Storage Administration (LVM & Filesystems)

- **Partitioning**: `fdisk`/`parted` for MBR/GPT partitions.
- **LVM stack**: Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV)
  - `pvcreate /dev/sdb1` → `vgcreate vgdata /dev/sdb1` → `lvcreate -L 10G -n lvdata vgdata`
  - Extend: `lvextend -L +5G /dev/vgdata/lvdata` then `xfs_growfs` (or `resize2fs` for ext4)
  - Reduce (ext4 only, not xfs): `lvreduce`
  - Snapshots: `lvcreate -s -L 1G -n snap1 /dev/vgdata/lvdata`
- **Filesystems**: `xfs` (default on RHEL 7+), `ext4`. Create with `mkfs.xfs` / `mkfs.ext4`.
- **Mounting**: `/etc/fstab` for persistent mounts; UUID preferred over device name (`blkid` to find UUID).
- **Swap**: `mkswap`, `swapon`, entry in `/etc/fstab`.
- Multipath / iSCSI concepts also live in this guide — be ready to explain PV/VG/LV relationship and why LVM is preferred over raw partitions (flexibility to resize/snapshot without downtime).

---

## 8. RHEL 8 Web Console (Cockpit)

- Browser-based system management UI, service name `cockpit`.
- Enable: `systemctl enable --now cockpit.socket`
- Default port: **9090** (`https://<host>:9090`)
- Lets you manage: storage (LVM), networking, users, services, logs (journal), software updates, containers, performance metrics — all without CLI.
- Useful to mention in interviews as the "modern GUI companion" to CLI administration, especially for less CLI-comfortable teams or quick visual diagnostics.

---

## 9. NTP (Time Sync)

- Service: `ntpd` (legacy) — many modern RHEL 8 systems now default to `chronyd`, know both.
- Config: `/etc/ntp.conf` — add `server <ntp-host>`
- `systemctl enable --now ntpd`
- Manual time sync: `ntpdate -v <hostname>`
- Check peers: `ntpq -p`
- Logs: `/var/log/messages`
- **Why it matters**: Kerberos auth (AD/LDAP integration), log correlation across servers, cluster consistency all depend on accurate time sync — good context to mention.

---

## 10. SCP / File Transfer

```bash
# Remote → local
scp user@host:file.txt /local/dir/

# Local → remote
scp file.txt user@host:/remote/dir/

# Directory copy (recursive)
scp -r user@host:/remote/dir/ /local/dir/

# Remote → remote
scp user@hostA:/path/file.txt user@hostB:/path/
```
Encrypted (SSH-based) replacement for legacy `rcp`/`ftp`.

---

## 11. Squid Proxy Server

Squid Proxy Server is an open-source proxy and web caching server used to improve network performance, reduce bandwidth usage, and control internet access. It acts as an intermediary between client systems and the internet. When a user requests a webpage, Squid first checks its local cache. If the content is available and valid, it serves it directly from the cache, reducing response time. If not, it fetches the content from the internet, delivers it to the client, and stores a copy for future requests. Squid also supports access control through ACLs, user authentication, URL filtering, logging, and bandwidth management. It is commonly used in enterprise networks, schools, and organizations to optimize internet usage and enforce security policies

```bash
dnf install squid* -y
systemctl enable --now squid
```
- Config: `/etc/squid/squid.conf`
  - `acl localnet src 192.168.100.0/24`
  - `http_access allow localnet`
  - Block sites: `acl blocksites url_regex "/etc/squid/blocksites"` + `http_access deny blocksites`
- Firewall: `firewall-cmd --permanent --add-port=3128/tcp && firewall-cmd --reload`
- Default port: **3128**
- Restart after config change: `systemctl restart squid`

---

## 12. Nagios (Monitoring)

- Dependencies: `httpd php gcc glibc gd gd-devel make net-snmp`
- Dedicated user/group: `nagios` / `nagcmd`; apache added to `nagcmd` group for web access.
- Build from source: `./configure --with-command-group=nagcmd`, `make all`, `make install`, `make install-init`, `make install-config`, `make install-webconf`
- Plugins installed separately (`nagios-plugins`).
- Host/service definitions: `/usr/local/nagios/etc/objects/hosts.cfg`
  - `define host {...}` / `define service {...}` blocks — know the basic structure (host_name, check_command, check_period, notification_interval).
- Web UI auth: `htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin`
- Default web path: `http://<host>/nagios`
- **Interview framing**: Nagios = classic host/service-level monitoring with alerting via checks + notification escalation — good to contrast with modern stack (Prometheus + Grafana + Alertmanager) since that's your current DXC toolset. Be ready to explain the shift from agent/plugin polling (Nagios) to pull-based metrics scraping (Prometheus) and dashboards (Grafana).

---

## Suggested Quick "Explain This" Prep List

Use these as 60-90 second interview answers:
1. Why `ip`/`ss` replaced `ifconfig`/`netstat`.
2. LVM resize workflow (extend LV → grow filesystem) live, no downtime.
3. SELinux enforcing vs permissive, and troubleshooting an AVC denial.
4. YUM vs RPM — dependency resolution vs raw package ops.
5. Bonding mode 4 (LACP) vs mode 1 (active-backup) — when to use each.
6. OS hardening checklist end-to-end (accounts → services → ports → SSH → firewall → SELinux → patching).
7. Nagios vs Prometheus/Grafana — legacy push/poll monitoring vs modern pull-based metrics + dashboards (ties directly to your current stack).
8. SCP/SSH vs FTP/Telnet/RSH — why cleartext protocols are deprecated.

---

*Compiled from your uploaded reference material (networking guides, RHEL 7/8 storage & security guides, SELinux guide, YUM/RPM cheat sheets, NTP, SCP, NIC bonding, Squid, Nagios notes, and the deprecated-commands chart).*
