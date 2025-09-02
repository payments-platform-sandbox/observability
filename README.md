# Observability (`observability`)

> Metrics and dashboards for the **payments platform**, built with **Prometheus + Grafana**.  
> Supports **FR5: Ops visibility & recovery** from the System Design Document.  


## 1) Overview
This repository provides the monitoring layer for the payments platform.  
It enables **service metrics collection** (Prometheus) and **visual dashboards** (Grafana).  

**Includes:**
- **Prometheus** — scrapes metrics from services (e.g., `/actuator/prometheus` in Spring Boot).  
- **Grafana** — dashboards for payments, orchestration, and infra health.  


## 2) Why Observability Matters
- Detects failures early (timeouts, retries, reconciliation stalls).  
- Tracks **latency, throughput, error rates** across services.  
- Provides operators with a **single dashboard view** into the system.  


## 3) Repository Layout
```
├── prometheus/
│   ├── alerts.yml              # Alerting rules
│   └── scrape-configs.yml      # Scrape targets for services
│
└── grafana/
├── dashboards/
│   ├── payments.json       # Payments metrics (latency, error rate)
│   ├── orchestration.json  # Workflow execution metrics
│   └── infra.json          # Postgres, Redis metrics
└── datasources.yml         # Prometheus datasource config
```

## 4) Metrics & Dashboards
Each service exposes metrics via `/actuator/prometheus` (Spring Boot).  
Prometheus collects these metrics; Grafana visualizes them.  

**Example Dashboards**
- **Payments** → request rate, latency, error %  
- **Orchestration** → workflow success/failure, retries, compensation count  
- **Infra** → Postgres connections, Redis memory/cache hit rate  


## 5) Alerting (Prometheus)
Sample rule (`prometheus/alerts.yml`):
```yaml
groups:
  - name: payment-alerts
    rules:
      - alert: HighPaymentErrorRate
        expr: rate(payment_requests_failed_total[5m]) > 0.05
        for: 10m
        labels:
          severity: critical
        annotations:
          summary: "High error rate in payment-service"
          description: "More than 5% of requests failed in the last 10 minutes."
```

## 6) Usage
### Run locally with Docker Compose
```
docker compose -f docker-compose.observability.yml up -d
```
### Access
- Grafana: http://localhost:3000
- Prometheus: http://localhost:9090

## 7) Notes
- This repo is **optional**: not required for FR1–FR4, but it completes **FR5 (ops visibility)**.
- Works alongside dev-env for local metrics and dashboards.
- Dashboards and alerts can be extended as new services are added.

## 8) License

MIT
