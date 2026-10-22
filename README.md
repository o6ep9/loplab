# loplab

Home DevOps lab running on a single laptop: Debian guests in Incus,
configured with Ansible, monitored with Prometheus and Grafana, alerting
to Telegram, with load balancing and failover.

Each stage has a goal, acceptance criteria and a failure drill.
See [docs/roadmap.md](docs/roadmap.md).

## Status

| Stage | Scope | Status |
|---|---|---|
| 0 | Foundation: Incus, guests, network, SSH | in progress |
| 1 | Configuration management: Ansible | planned |
| 2 | Monitoring and alerting | planned |
| 3 | High availability: HAProxy, keepalived | planned |
| 4 | Data: PostgreSQL replication, backups, Redis Sentinel | planned |
| 5 | CI/CD and containers: GitLab CI, k3s, Helm | planned |
| 6 | Infrastructure as code: Terraform, Proxmox | planned |
| 7 | Logging and observability: OpenSearch, OpenTelemetry, SLO | planned |

## Topology

```
host (CachyOS, Incus)
└── bridge lab-br0  10.10.10.0/24, gateway 10.10.10.1, NAT
    ├── 10.10.10.10  VIP (keepalived)
    ├── 10.10.10.11  lb1   HAProxy, keepalived
    ├── 10.10.10.12  lb2   HAProxy, keepalived
    ├── 10.10.10.21  web1  nginx
    ├── 10.10.10.22  web2  nginx
    └── 10.10.10.31  mon   Prometheus, Alertmanager, Grafana
```

## Stack

| Layer | Tool |
|---|---|
| Host | ThinkPad T14s Gen 3, Ryzen 7 PRO 6850U (8C/16T), 13 GiB RAM, CachyOS |
| Virtualization | Incus: Debian 13 system containers, VMs where a separate kernel is required |
| Configuration | Ansible |
| Monitoring | Prometheus, node_exporter, Alertmanager, Grafana |

## Repository layout

```
infra/incus/   network, profile, guest provisioning
ansible/       inventory, roles, playbooks
monitoring/    alert rules, Grafana dashboards
scripts/       checks and failure drills
docs/          roadmap, runbooks, screenshots
```

Directories are added as content appears.

## Usage

TBD.

## Verification

TBD.
