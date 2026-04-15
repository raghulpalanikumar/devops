"# DevOps Monitoring & Alerting Stack

A Docker-based monitoring and alerting solution using Prometheus, Grafana, Node Exporter, and AlertManager. This stack provides comprehensive system monitoring, visualization, and alert notifications via email.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Services](#services)
- [Access Points](#access-points)
- [Configuration](#configuration)
- [Architecture](#architecture)
- [Alert Rules](#alert-rules)
- [Usage](#usage)
- [Troubleshooting](#troubleshooting)

## 📖 Overview

This project sets up a complete monitoring ecosystem that:
- Collects system metrics from your infrastructure
- Stores and queries metrics using Prometheus
- Visualizes data through Grafana dashboards
- Triggers alerts based on predefined rules
- Sends notifications via email

## 🔧 Prerequisites

Before starting, ensure you have:
- **Docker** (v20.10 or higher)
- **Docker Compose** (v1.29 or higher)
- A valid Gmail account (for email alerts)
- At least 2GB of RAM allocated to Docker

## 🚀 Quick Start

1. **Clone or navigate to the project directory:**
   ```bash
   cd d:\Devops
   ```

2. **Update AlertManager configuration:**
   - Edit `alertmanager.yml` and update Gmail credentials with an app-specific password

3. **Start all services:**
   ```bash
   docker-compose up -d
   ```

4. **Verify services are running:**
   ```bash
   docker-compose ps
   ```

5. **Access the services:**
   - Prometheus: http://localhost:9090
   - Grafana: http://localhost:3000
   - AlertManager: http://localhost:9093

## 🔧 Services

### Prometheus
- **Port:** 9090
- **Purpose:** Metrics collection and storage
- **Configuration:** `prometheus.yml`
- **Scrape Interval:** 5 seconds
- **Features:**
  - Collects metrics from Node Exporter
  - Evaluates alert rules
  - Routes alerts to AlertManager

### Grafana
- **Port:** 3000
- **Purpose:** Metrics visualization and dashboards
- **Default Credentials:** 
  - Username: `admin`
  - Password: `admin`
- **Features:**
  - Create custom dashboards
  - Visualize Prometheus metrics
  - Alert management UI

### Node Exporter
- **Port:** 9100
- **Purpose:** Exports system and hardware metrics
- **Metrics Exposed:**
  - CPU usage
  - Memory usage
  - Disk I/O
  - Network statistics
  - Process information

### AlertManager
- **Port:** 9093
- **Purpose:** Handles and routes alerts
- **Configuration:** `alertmanager.yml`
- **Notification Method:** Email

## 📍 Access Points

| Service | URL | Port |
|---------|-----|------|
| Prometheus | http://localhost:9090 | 9090 |
| Grafana | http://localhost:3000 | 3000 |
| Node Exporter Metrics | http://localhost:9100/metrics | 9100 |
| AlertManager | http://localhost:9093 | 9093 |

## ⚙️ Configuration

### Prometheus Configuration (`prometheus.yml`)
```yaml
Global Settings:
- Scrape interval: 5 seconds
- Target: Node Exporter on port 9100

Alert Configuration:
- Rule file: alert.rules.yml
- AlertManager target: localhost:9093
```

### Alert Rules (`alert.rules.yml`)
Triggered alerts:
- **HighCPUUsage:** Triggers when CPU usage > 5% for 1 second
- **HighMemoryUsage:** Triggers when Memory usage > 5% for 1 second

### AlertManager Configuration (`alertmanager.yml`)
Email notification settings:
- **SMTP Server:** smtp.gmail.com:587
- **From Email:** raghulpg2006@gmail.com
- **To Email:** raghulpg2006@gmail.com
- **TLS:** Enabled

**Email Template:**
- Uses custom HTML template (`email_template.html`) for professional alert formatting
- Features:
  - Color-coded alert status (🔴 Red for active, ✅ Green for resolved)
  - Severity badges with visual indicators
  - Clean, organized layout with instance details
  - Direct links to AlertManager for quick access
  - Professional styling with improved readability
  - Grouped alerts for better organization

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Monitoring Stack                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐ │
│  │  Node Exporter   │  │   Prometheus     │  │    Grafana   │ │
│  │   (9100)         │  │    (9090)        │  │   (3000)     │ │
│  └────────┬─────────┘  └────────┬─────────┘  └──────┬───────┘ │
│           │                     │                    │          │
│           │    Metrics          │                    │          │
│           └─────────────────────┤                    │          │
│                                 │ Alert Rules        │          │
│                            ┌────▼─────────────┐     │          │
│                            │  Alert Manager   │     │          │
│                            │    (9093)        │     │          │
│                            └────┬─────────────┘     │          │
│                                 │                    │          │
│                        ┌────────▼────────┐           │          │
│                        │   Email Alert   │           │          │
│                        │  (Gmail SMTP)   │           │          │
│                        └─────────────────┘           │          │
│                                                      │          │
│                          Connected to Grafana ───────┘          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

## 🚨 Alert Rules

The system monitors two critical metrics:

### 1. HighCPUUsage
- **Condition:** CPU usage > 5%
- **Duration:** 1 second
- **Severity:** Critical
- **Action:** Email notification

### 2. HighMemoryUsage
- **Condition:** Memory usage > 5%
- **Duration:** 1 second
- **Severity:** Critical
- **Action:** Email notification

*Note: Alert thresholds can be adjusted in `alert.rules.yml`*

## 📊 Usage

### View Metrics in Prometheus
1. Open http://localhost:9090
2. Go to "Graph" tab
3. Example queries:
   - `node_cpu_seconds_total` - CPU metrics
   - `node_memory_MemTotal_bytes` - Memory metrics

### Create Grafana Dashboard
1. Open http://localhost:3000
2. Login with default credentials
3. Add Prometheus as data source
4. Create visualization panels
5. Save as dashboard

### Check Alert Status
1. Open http://localhost:9093
2. View active alerts
3. Check alert history

## 🛠️ Commands

```bash
# Start services
docker-compose up -d

# Stop services
docker-compose down

# View logs for all services
docker-compose logs -f

# View logs for specific service
docker-compose logs -f prometheus
docker-compose logs -f grafana
docker-compose logs -f node_exporter
docker-compose logs -f alertmanager

# Restart services
docker-compose restart

# Check service status
docker-compose ps

# Remove containers and volumes
docker-compose down -v
```

## 📁 Project Files

- `docker-compose.yml` - Service definitions and configurations
- `prometheus.yml` - Prometheus configuration
- `alert.rules.yml` - Alert rules and thresholds
- `alertmanager.yml` - AlertManager configuration and notification settings
- `email_template.html` - Custom HTML email template for professional alert formatting
- `README.md` - This file

## ⚠️ Troubleshooting

### Services not starting
```bash
# Check logs
docker-compose logs

# Verify Docker daemon is running
docker ps
```

### Metrics not appearing in Prometheus
- Ensure Node Exporter is running: http://localhost:9100/metrics
- Check Prometheus target status in UI: http://localhost:9090/targets

### Alerts not sending emails
- Verify Gmail credentials in `alertmanager.yml`
- Ensure "Less secure app access" is enabled (or use app-specific password)
- Check AlertManager logs: `docker-compose logs alertmanager`

### Connection refused errors
- Ensure services are running: `docker-compose ps`
- Check if ports are already in use
- Try recreating containers: `docker-compose down && docker-compose up -d`

## 📝 Notes

- Alert thresholds (5%) are set very low for testing. Adjust in `alert.rules.yml` for production
- Gmail credentials are hardcoded. For production, use secrets management
- Default Grafana password should be changed on first login
- Regular backups recommended for Prometheus data

## 📞 Support

For issues or questions, check:
- Individual service logs: `docker-compose logs <service-name>`
- Service health pages at their respective ports
- Official documentation:
  - [Prometheus](https://prometheus.io/docs/)
  - [Grafana](https://grafana.com/docs/)
  - [AlertManager](https://prometheus.io/docs/alerting/latest/alertmanager/)" 
