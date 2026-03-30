# Canopy

**Canopy** is an open-source platform for building FAIR-aligned scientific data hubs that support study discovery, data sharing, and metadata validation. It is derived from the NIH RADx Data Hub, a cloud-based platform originally developed for the NIH Rapid Acceleration of Diagnostics (RADx) program during the COVID-19 pandemic. RADx Data Hub is available at https://radxdatahub.nih.gov/ and [on GitHub](https://github.com/radxdatahub). Canopy redesigns the core components of the RADx Data Hub by removing domain-specific assumptions while retaining essential functionality.

---

## 🚀 Getting Started

**Deploying Canopy to AWS**  
Start here → 📘 [Deployment Guide](https://github.com/canopy-datahub/datahub-docs/blob/feature/aws/DEPLOYMENT_GUIDE.md)

**Exploring the codebase?**  
Start here → 🗂️ [Main Repository](https://github.com/canopy-datahub/dataHub) — links to every service, tool, and guide

---

## 🏗️ Architecture

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

## 📦 Repository Map

### Backend Services (Spring Boot)
| Repository | Description |
|---|---|
| [datahub-service-entity](https://github.com/canopy-datahub/datahub-service-entity) | Direct retrieval of database entities |
| [datahub-service-search](https://github.com/canopy-datahub/datahub-service-search) | Search across studies and variables |
| [datahub-service-user](https://github.com/canopy-datahub/datahub-service-user) | User info, profiles, and support requests |
| [datahub-service-submission](https://github.com/canopy-datahub/datahub-service-submission) | Data and study ingestion workflows |
| [datahub-service-report](https://github.com/canopy-datahub/datahub-service-report) | Metrics dashboard and reporting |
| [datahub-service-download](https://github.com/canopy-datahub/datahub-service-download) | Controlled dataset file downloads |
| [datahub-service-email](https://github.com/canopy-datahub/datahub-service-email) | Lambda-based email notifications via AWS SES |
| [datahub-lib-keycloak-auth](https://github.com/canopy-datahub/datahub-lib-keycloak-auth) | Shared Keycloak authentication library |
| [datahub-project](https://github.com/canopy-datahub/datahub-project) | Maven parent POM for all Java services |

### Frontend
| Repository | Description |
|---|---|
| [datahub-ui-main](https://github.com/canopy-datahub/datahub-ui-main) | Next.js / React web application |

### Infrastructure & Deployment
| Repository | Description |
|---|---|
| [datahub-cloud-replication](https://github.com/canopy-datahub/datahub-cloud-replication) | AWS CloudFormation templates
| [datahub-development](https://github.com/canopy-datahub/datahub-development) | PostgreSQL schema scripts, seed data, OpenSearch Lambda, Keycloak Docker Compose |
| [datahub-docs](https://github.com/canopy-datahub/datahub-docs) | Deployment guide, limitations, and operator documentation |
| [datahub-deployment-scripts](https://github.com/canopy-datahub/datahub-deployment-scripts) | Automation scripts supporting deployment and operations |

### Developer Tooling
| Repository | Description |
|---|---|
| [datahub-cli](https://github.com/canopy-datahub/datahub-cli) | CLI for local development and server management |
| [datahub-utility-scripts](https://github.com/canopy-datahub/datahub-utility-scripts) | Automation helpers and publication utilities |
