# Stalwart Mail Server Monitoring Stack

A monitoring stack for [Stalwart Mail Server](https://stalw.art) using **Prometheus** for metrics scraping and **Grafana** for visualization.

The project is split into two phases: a local Docker-based test environment, and a bare-metal production deployment on Proxmox.

---

## Architecture
```
Stalwart ──/metrics/prometheus──► Prometheus ──► Grafana
```

---

## Project Structure
```
stalwart-monitoring-stack/
├── Phase-1_Docker/                    # Phase 1 — Docker test environment
│   ├── grafana/img/
│   ├── stalwart/
│   ├── stalwart-config/
│   ├── img/                           # Screenshots
│   └── Docker.md                      # Full Docker setup guide
├── Phase-2_Proxmox/                   # Phase 2 — Bare-metal production
│   ├── img/                           # Screenshots
│   ├── Proxmox.md                     # Full Proxmox deployment guide
│   └── Stalwart-dashbord.json         # Corrected Grafana dashboard
├── prometheus/
│   └── prometheus.yml                 # Prometheus config (used by Docker)
├── docker-compose.yml
└── README.md
```

---

## Project Phases

| Phase | Environment | Doc |
|-------|-------------|-----|
| Phase 1 — Testing | Docker (local) | [Docker.md](Phase-1_Docker/Docker.md) |
| Phase 2 — Production | Bare metal on Proxmox | [Proxmox.md](Phase-2_Proxmox/Proxmox.md) |

---

## Stack

| Service | Version | Port(s) |
|---------|---------|---------|
| Stalwart | `stalwartlabs/stalwart:latest` | 8080, 25, 143, 587 |
| Prometheus | `prom/prometheus:latest` | 9090 |
| Grafana | `grafana/grafana:latest` | 3000 |

---

## Quick Start (Docker)
```bash
git clone git@github.com:Souheib-h/stalwart-monitoring-stack.git
cd stalwart-monitoring-stack
docker compose up -d
```

Then open `http://localhost:3000` for Grafana and `http://localhost:9090` for Prometheus.

→ Full guide: [Phase-1_Docker/Docker.md](Phase-1_Docker/Docker.md)

---

## Production Deployment

→ Full guide: [Phase-2_Proxmox/Proxmox.md](Phase-2_Proxmox/Proxmox.md)

---

## Known Issue — Metric Name Prefix Mismatch

Dashboard `23498` was built for an older Stalwart version that prefixed metrics with `stalwart_`. Modern versions dropped this prefix — all panels show `N/A` out of the box.

**Fix:** Import the corrected dashboard JSON from [`Phase-2_Proxmox/Stalwart-dashbord.json`](Phase-2_Proxmox/Stalwart-dashbord.json).

---

## Next Steps

- [ ] Keycloak IAM integration on the Stalwart VM
- [ ] Grafana alerting on `auth_error` spikes
- [ ] `node_exporter` on both VMs for system-level metrics
- [ ] Basic auth on the Stalwart metrics endpoint