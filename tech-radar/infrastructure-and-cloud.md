# Tech Radar: Infrastructure & Cloud

**Version:** 1.0  
**Status:** Active  
**Owner:** Engineering Leadership / Platform Team  
**Last Updated:** 2026-07-31

---

## Cloud Provider

| Technology | Ring | Notes |
|---|---|---|
| **[Primary Cloud Provider]** | ✅ Approved | All production infrastructure runs here. Replace this placeholder with your actual provider (AWS / GCP / Azure). |
| **Multi-cloud** | ⚠️ Restricted | Permitted only for specific DR or compliance scenarios. Requires ADR. Avoid multi-cloud by default — operational complexity is high. |

---

## Container & Orchestration

| Technology | Ring | Notes |
|---|---|---|
| **Docker** | ✅ Approved | Required for all services. All services must ship as a container image. |
| **Kubernetes** | ✅ Approved | Standard deployment target for all environments above local. |
| **Docker Compose** | ✅ Approved | Local development only. Not for staging or production. |
| **Nomad** | 🚫 Banned | Kubernetes is the standard; do not introduce Nomad. |
| **ECS / Fargate** | ⚠️ Restricted | Permitted only in cloud-native migrations where Kubernetes is not yet available. Requires ADR. |

---

## Infrastructure as Code (IaC)

| Technology | Ring | Notes |
|---|---|---|
| **Terraform** | ✅ Approved | Standard IaC tool. All infrastructure must be defined as code. No manual console changes in staging or production. |
| **Helm** | ✅ Approved | Standard for Kubernetes application packaging and deployment. |
| **Pulumi** | 🔵 Trial | Permitted for teams comfortable with general-purpose languages. Evaluate against Terraform ergonomics. |
| **AWS CloudFormation** | ⚠️ Restricted | Permitted only in AWS-specific contexts where Terraform is insufficient. |
| **Manual / ClickOps** | 🚫 Banned | No manual infrastructure changes in staging or production. All changes must be IaC and go through the deployment pipeline. |

---

## CI/CD

| Technology | Ring | Notes |
|---|---|---|
| **GitHub Actions** | ✅ Approved | Standard CI/CD platform. |
| **ArgoCD** | ✅ Approved | GitOps-based continuous deployment to Kubernetes. |
| **Jenkins** | 🚫 Banned | Replaced by GitHub Actions. Migrate any remaining Jenkins pipelines. |

---

## Networking & API Gateway

| Technology | Ring | Notes |
|---|---|---|
| **API Gateway (managed)** | ✅ Approved | See the service catalog for the specific product in use. |
| **Nginx** | ✅ Approved | Permitted as an ingress controller and reverse proxy. |
| **Traefik** | 🔵 Trial | Permitted as a Kubernetes ingress alternative to Nginx. |
| **HAProxy** | ⚠️ Restricted | Legacy only. No new HAProxy deployments. |

---

## Observability

| Technology | Ring | Notes |
|---|---|---|
| **Prometheus** | ✅ Approved | Standard metrics collection. All services must expose `/metrics` in Prometheus format. |
| **Grafana** | ✅ Approved | Standard metrics visualization and alerting dashboards. |
| **OpenTelemetry (OTel)** | ✅ Approved | Standard instrumentation library. Use OTel SDKs for distributed tracing and metrics. |
| **Jaeger / Tempo** | ✅ Approved | Distributed tracing backend. |
| **ELK / EFK stack** | 🔵 Trial | Approved for log aggregation. Elasticsearch, Fluentd/Logstash, Kibana. |
| **Datadog** | 🔵 Trial | Permitted as a managed observability alternative. Requires cost review for production use. |
| **New Relic** | ⚠️ Restricted | Permitted in existing integrations only. No new New Relic usage. |

---

## Secrets Management

| Technology | Ring | Notes |
|---|---|---|
| **HashiCorp Vault** | ✅ Approved | Standard secrets manager. All production secrets must be stored here. |
| **GitHub Actions Secrets** | ✅ Approved | Permitted for CI/CD pipeline secrets only. |
| **AWS Secrets Manager** | 🔵 Trial | Permitted in cloud-native deployments. Use if Vault is not available in the environment. |
| **Hardcoded secrets** | 🚫 Banned | Absolute prohibition. See [`standards/security-standards.md`](../standards/security-standards.md). |
| **`.env` files in production** | 🚫 Banned | `.env` is for local development only. Never deploy `.env` files. |

---

## Related Documents

- [`tech-radar/README.md`](./README.md)
- [`tech-radar/banned-and-restricted.md`](./banned-and-restricted.md)
- [`context/environment-topology.md`](../context/environment-topology.md)
- [`standards/security-standards.md`](../standards/security-standards.md)
- [`standards/observability-standards.md`](../standards/observability-standards.md)
