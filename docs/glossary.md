# Glossary

| Term | Definition |
|---|---|
| Host | Physical machine running the lab. |
| Guest | Virtual server running on the host. |
| Virtual machine (VM) | Guest with its own kernel and emulated hardware. Strong isolation, higher memory cost. |
| System container | Guest sharing the host kernel, with its own processes, filesystem, network and users. Runs a full init system, unlike an application container (Docker). |
| Incus | Manager for system containers and VMs. Fork of LXD. |
| KVM / AMD-V | Hardware-assisted virtualization in the Linux kernel and CPU. |
| Bridge | Virtual layer-2 switch on the host connecting guests. |
| Subnet, CIDR | Address range, e.g. `10.10.10.0/24` = 256 addresses sharing the first 24 bits. |
| NAT | Translation of guest addresses to the host address for outbound traffic. |
| SSH key pair | Private key (kept by the client) and public key (installed on the server) for passwordless authentication. |
| cloud-init | First-boot provisioning: hostname, users, SSH keys, packages. |
| Snapshot | Saved guest state that can be restored. |
