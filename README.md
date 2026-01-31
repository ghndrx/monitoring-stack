# Monitoring Stack

![Prometheus](https://img.shields.io/badge/Prometheus-2.47+-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-10+-F46800?style=flat&logo=grafana&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue)

Production-ready monitoring stack configurations for Prometheus, Grafana, Loki, and Alertmanager.

## Components

```
├── prometheus/
│   ├── rules/           # Alert rules
│   └── targets/         # Scrape targets
├── grafana/
│   ├── dashboards/      # JSON dashboards
│   └── datasources/     # Data source configs
├── alertmanager/        # Alert routing
├── loki/                # Log aggregation
└── promtail/            # Log shipping
```

## Dashboards

- 📊 Node Exporter - System metrics
- 🐳 Docker/Kubernetes - Container metrics
- 🌐 NGINX/Traefik - Ingress metrics
- 💾 PostgreSQL/Redis - Database metrics
- ⚡ Custom app dashboards

## Alert Rules

- 🔴 HighCPU, HighMemory, DiskFull
- 🟡 ServiceDown, HighLatency
- 🔵 CertExpiring, BackupFailed

## Quick Start

```bash
docker-compose up -d
# Grafana: http://localhost:3000 (admin/admin)
# Prometheus: http://localhost:9090
```

## License

MIT
