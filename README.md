# Monitoring Stack

![Prometheus](https://img.shields.io/badge/Prometheus-2.50+-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-10+-F46800?style=flat&logo=grafana&logoColor=white)
![Alertmanager](https://img.shields.io/badge/Alertmanager-0.27+-E6522C?style=flat&logo=prometheus&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-blue)

Production-ready monitoring stack configurations for Prometheus, Grafana, Loki, and Alertmanager.

## 🗂️ Structure

```
├── prometheus/
│   ├── prometheus.yml         # Main Prometheus config
│   ├── rules/                 # Alert rules
│   │   └── node-exporter.yml  # Host monitoring alerts (30+ rules)
│   └── targets/               # File-based service discovery
│       └── node-exporter.yml  # Node exporter targets
├── alertmanager/
│   └── alertmanager.yml       # Alert routing & receivers
├── grafana/
│   ├── dashboards/            # JSON dashboards
│   └── datasources/           # Data source configs
├── loki/                      # Log aggregation
└── promtail/                  # Log shipping
```

## 📊 Alert Rules

### Node Exporter (Host Monitoring)

| Category | Alerts |
|----------|--------|
| **Availability** | NodeDown |
| **CPU** | HighCpuLoad, CriticalCpuLoad, CpuStealNoisyNeighbor |
| **Memory** | HighMemoryUsage, CriticalMemoryUsage, MemoryUnderPressure, OomKillDetected |
| **Disk Space** | DiskSpaceWarning, DiskSpaceCritical, DiskWillFillIn24Hours |
| **Disk I/O** | DiskReadLatency, DiskWriteLatency, DiskIOSaturation |
| **Network** | NetworkReceiveErrors, NetworkTransmitErrors, NetworkInterfaceSaturated |
| **System** | ClockSkew, ClockNotSynchronising, RequiresReboot, SystemdServiceCrashed |
| **Resources** | LowEntropy, FileDescriptorsExhausted, ConntrackLimit |

### Alert Severities

- 🔴 **Critical** - Page immediately, potential data loss or outage
- 🟡 **Warning** - Investigate within hours, degradation detected
- 🔵 **Info** - Low priority, informational only

## 🔔 Alertmanager Features

- **Intelligent routing** - Critical → PagerDuty, Warning → Slack, Info → low-priority channel
- **Inhibition rules** - Critical alerts suppress matching warnings
- **Grouped notifications** - Reduces alert fatigue
- **Multiple receivers** - Slack, PagerDuty, Email, Webhooks pre-configured

## 🚀 Quick Start

### Docker Compose

```bash
docker-compose up -d

# Access points:
# Prometheus: http://localhost:9090
# Alertmanager: http://localhost:9093
# Grafana: http://localhost:3000 (admin/admin)
```

### Kubernetes / Helm

```bash
# Using kube-prometheus-stack
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring prometheus-community/kube-prometheus-stack \
  --set alertmanager.config="$(cat alertmanager/alertmanager.yml)"
```

### Copy Rules Only

```bash
# If you have existing Prometheus, just copy the rules
cp prometheus/rules/*.yml /etc/prometheus/rules/
# Reload: curl -X POST http://localhost:9090/-/reload
```

## 📁 File-Based Service Discovery

Add hosts without restarting Prometheus:

```yaml
# prometheus/targets/node-exporter.yml
- targets:
    - 'web-server-01:9100'
    - 'web-server-02:9100'
  labels:
    role: 'web'
    datacenter: 'us-east-1'
```

Prometheus watches this file and auto-reloads targets.

## 🔧 Configuration

### Environment Variables (Alertmanager)

```bash
# .env or secrets
SLACK_WEBHOOK_URL=https://hooks.slack.com/services/...
PAGERDUTY_ROUTING_KEY=your-routing-key
SMTP_PASSWORD=your-smtp-password
```

### Customizing Thresholds

Edit `prometheus/rules/node-exporter.yml`:

```yaml
# Change CPU threshold from 80% to 75%
- alert: HostHighCpuLoad
  expr: 100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 75
```

## 📚 References

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Alertmanager Configuration](https://prometheus.io/docs/alerting/latest/configuration/)
- [Awesome Prometheus Alerts](https://samber.github.io/awesome-prometheus-alerts/)
- [Node Exporter](https://github.com/prometheus/node_exporter)

## 📝 License

MIT
