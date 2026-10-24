# Host setup

One-time changes on the host (CachyOS). Each change is lab-specific and reversible.

## Changes

| Change | Purpose | Revert |
|---|---|---|
| Package `incus` | Container and VM manager | `sudo pacman -Rns incus` |
| `root:1000000:1000000000` in `/etc/subuid` and `/etc/subgid` | UID/GID ranges for unprivileged containers | Remove the line from both files |
| `incus.socket` enabled | Start the Incus daemon on first client request | `sudo systemctl disable --now incus.socket` |
| User in group `incus-admin` | Manage Incus without sudo (root-equivalent access) | `sudo gpasswd -d $USER incus-admin` |
| Storage pool `default` (btrfs subvolume under `/var/lib/incus`) | Guest root filesystems | `incus storage delete default` (after deleting all guests) |
| ufw: allow input and routing on `lab-br0` | DHCP/DNS from guests to the host, forwarded traffic to the internet | `sudo ufw delete allow in on lab-br0`, same for both `route` rules |

## Steps

```sh
sudo pacman -S incus
echo "root:1000000:1000000000" | sudo tee -a /etc/subuid /etc/subgid
sudo systemctl enable --now incus.socket
sudo usermod -aG incus-admin $USER
newgrp incus-admin                     # or log out and back in
incus admin init --preseed < infra/incus/preseed.yaml
sudo ufw allow in on lab-br0
sudo ufw route allow in on lab-br0
sudo ufw route allow out on lab-br0
```

The ufw rules must be in place before the first guest boots. Otherwise the guest
gets no DHCP lease, cloud-init fails to install packages (`Temporary failure
resolving 'deb.debian.org'` in `/var/log/cloud-init-output.log`) and SSH is
missing. cloud-init runs once per instance, so recreate the guest after fixing
the firewall.

## Verification

```sh
incus version                          # client and server versions
incus storage list                     # pool "default", driver btrfs
incus launch images:debian/13 smoke
incus exec smoke -- cat /etc/os-release
incus delete --force smoke
```
