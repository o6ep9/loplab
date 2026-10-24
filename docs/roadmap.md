# Roadmap

Each stage is tracked as a GitHub issue with the same structure:
problem, goal, acceptance criteria, failure injection.

## Stage 0. Foundation

- **Problem:** no isolated environment to build and break infrastructure safely.
- **Goal:** Incus with Debian guests on an internal bridge network, SSH key access.
- **Acceptance criteria:**
  - every guest is reachable via `ssh <name>` without a password;
  - guests reach each other and the internet;
  - guests are created and destroyed with one command.
- **Failure injection:** delete a guest and recreate it with the same command.

## Stage 1. Configuration management

- **Problem:** server configuration is undocumented and cannot be reproduced.
- **Goal:** Ansible inventory and roles `common` (packages, users, SSH hardening), `nginx`, `node_exporter`.
- **Acceptance criteria:**
  - a fresh guest reaches the target state with a single `ansible-playbook` run;
  - a second run reports `changed=0` (idempotency).
- **Failure injection:** break nginx config and remove a package manually; destroy a guest entirely; restore both with the playbook.

## Stage 2. Monitoring and alerting

- **Problem:** outages are not detected until users report them.
- **Goal:** Prometheus scraping node_exporter on all guests, Grafana dashboard, Alertmanager with Telegram notifications.
- **Acceptance criteria:**
  - dashboard shows all hosts;
  - alert rules for host down, high CPU, low disk space;
  - Telegram receives both firing and resolved notifications.
- **Failure injection:** stop a guest, generate CPU load, fill a disk.
- **Screenshots:** dashboard, Telegram alert.

## Stage 3. High availability and networking

- **Problem:** a single web server is a single point of failure.
- **Goal:** two nginx backends behind HAProxy; two HAProxy nodes sharing a virtual IP via keepalived; WireGuard tunnel, nftables rules, traffic analysis with tcpdump.
- **Acceptance criteria:**
  - service stays available when any one backend or load balancer is down;
  - failover is visible in Grafana.
- **Failure injection:** stop a backend under load; stop the active load balancer.
- **Screenshots:** failover on the Grafana graph.

## Stage 4. Data

PostgreSQL primary/replica, backups (pgBackRest or pg_dump), restore test, Redis Sentinel.

## Stage 5. CI/CD and containers

GitLab CE and runner, pipeline deploying via Ansible, k3s, Helm.

## Stage 6. Infrastructure as code

Terraform provider for Incus, Proxmox in nested virtualization.

## Stage 7. Logging and observability

OpenSearch/ELK, Kibana, Python automation, OpenTelemetry, SLO.
