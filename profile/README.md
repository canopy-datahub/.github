# Canopy

**Canopy** is an open-source platform for FAIR-aligned scientific data hubs, supporting data sharing, harmonization, discovery, and reuse across research studies. Canopy is derived from the NIH RADx Data Hub (https://radxdatahub.nih.gov/), a cloud-based platform originally developed for the NIH Rapid Acceleration of Diagnostics (RADx) program. RADx Data Hub is available [on GitHub](https://github.com/radxdatahub). Rather than presenting a one-size-fits-all data hub, Canopy enables customization of RADx Data Hub technology for the needs of specific scientific domains.

> **Live demo:** A demonstration instance of Canopy is publicly available at **[canopy.stanford.edu](https://canopy.stanford.edu/)**. All studies, datasets, and files on that site are synthetic and intended for demonstration purposes only.


---

## Getting Started

**Deploying Canopy to AWS**  
Start here → [Deployment Guide](https://github.com/canopy-datahub/canopy-docs/blob/main/DEPLOYMENT_GUIDE.md)

**Exploring the codebase?**  
Start here → [Repositories](https://github.com/orgs/canopy-datahub/repositories) — links to every service, tool, and guide

**Want to contribute?**  
Start here → [Contributing Guide](https://github.com/canopy-datahub/.github/blob/main/CONTRIBUTING.md)

---

## Architecture

Canopy runs on **AWS** as a microservices platform:

- **7 Spring Boot microservices** on ECS Fargate, behind an Application Load Balancer
- **Next.js / React frontend** with server-side rendering
- **PostgreSQL (RDS)** for relational data persistence
- **OpenSearch** for full-text and faceted search
- **AWS Lambda** for asynchronous email processing and search reindexing
- **S3** for dataset file storage
- **Keycloak** for authentication and authorization
- **CloudFormation** (IaC) for repeatable, auditable AWS deployments

---

## Repository Map

### Backend Services (Spring Boot)
| Repository | Description |
|---|---|
| [canopy-service-entity](https://github.com/canopy-datahub/canopy-service-entity) | Direct retrieval of database entities |
| [canopy-service-search](https://github.com/canopy-datahub/canopy-service-search) | Search across studies and variables |
| [canopy-service-user](https://github.com/canopy-datahub/canopy-service-user) | User info, profiles, and support requests |
| [canopy-service-submission](https://github.com/canopy-datahub/canopy-service-submission) | Data and study ingestion workflows |
| [canopy-service-report](https://github.com/canopy-datahub/canopy-service-report) | Metrics dashboard and reporting |
| [canopy-service-download](https://github.com/canopy-datahub/canopy-service-download) | Controlled dataset file downloads |
| [canopy-service-email](https://github.com/canopy-datahub/canopy-service-email) | Lambda-based email notifications via AWS SES |
| [canopy-project](https://github.com/canopy-datahub/canopy-project) | Maven parent POM for all Java services |

### Frontend
| Repository | Description |
|---|---|
| [canopy-ui-main](https://github.com/canopy-datahub/canopy-ui-main) | Next.js / React web application |

### Infrastructure & Deployment
| Repository | Description |
|---|---|
| [canopy-cloud-replication](https://github.com/canopy-datahub/canopy-cloud-replication) | AWS CloudFormation templates
| [canopy-development](https://github.com/canopy-datahub/canopy-development) | PostgreSQL schema scripts, seed data, OpenSearch Lambda, Keycloak Docker Compose |
| [canopy-docs](https://github.com/canopy-datahub/canopy-docs) | Deployment guide, limitations, and operator documentation |

### Developer Tooling
| Repository | Description |
|---|---|
| [canopy-cli](https://github.com/canopy-datahub/canopy-cli) | CLI for local development and server management |
