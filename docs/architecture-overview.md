# Financial Market Aggregator – Architecture Overview

## Project Overview

This project is a containerized, Kubernetes-orchestrated stack that fetches and serves financial market data. It consists of:

- A **Go backend API** for data fetching and persistence.
- A **PostgreSQL** database to store historical and current market data.
- A **Next.js** frontend to visualize market trends, summaries, and symbols.

All services are deployed in a Kubernetes cluster running on the AWS Free Tier using EKS.

## System Architecture

```
[Next.js Frontend (static)] <---> [Go API Backend] <---> [PostgreSQL DB]
                                         ↑
                               [External Market Data APIs]
```

## Backend (Go API)

### Responsibilities
- Fetches current and historical data from external market data providers.
- Stores market data in PostgreSQL.
- Exposes REST endpoints to serve data to the frontend.

### Example Endpoints
- `GET /markets/current`
- `GET /markets/historic?from=2025-01-01&to=2025-04-30`
- `GET /symbols/:symbol/stats`

### Scheduled Jobs
- Hourly or daily cron job to fetch and store updated market data.

## Frontend (Next.js)

### Responsibilities
- Static site generated at build time.
- Fetches and displays market data via the Go API.
- Interactive visualizations, filters, and search.

### Pages/Views
- Market overview dashboard
- Symbol detail pages
- Historic data graphs
- Filters by region, timeframe, or currency

## Kubernetes Deployment

| Component         | Resource Type             | Notes                          |
|------------------|---------------------------|---------------------------------|
| PostgreSQL        | `StatefulSet` + `PVC`     | Backed by AWS EBS               |
| Go API            | `Deployment` + `Service`  | Stateless container             |
| Next.js Frontend  | `Deployment` + `Service`  | Optional: S3 + CloudFront       |
| Market Data Fetch | `CronJob`                 | Scheduled via Kubernetes        |
| Routing           | `Ingress` (ALB Controller)| Handles API + frontend traffic  |
| Configuration     | `ConfigMap`               | App configs like base URLs      |
| Secrets           | `Secret`                  | DB passwords, API keys          |

## Cloud Infrastructure

**Platform**: AWS Free Tier  
**Orchestration**: EKS (Elastic Kubernetes Service)  
**Networking**:
- ALB Ingress Controller for external access
- Internal communication via Kubernetes services

**Storage**:
- PostgreSQL uses Persistent Volume backed by EBS

## Stretch Goals

- Setup CI/CD with GitHub Actions (build → push → deploy)
- Add metrics/logs with Prometheus + Grafana or CloudWatch
- Enable HTTPS via Cert-Manager and Route 53
- Use Helm for reusable deployments
