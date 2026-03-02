# zero-trust-lab-README.md
# 🔐 Zero Trust Architecture Lab – GCP

Welcome to the **Zero Trust Architecture Lab**. This lab demonstrates the implementation of a Zero Trust security model on **Google Cloud Platform (GCP)** using Identity-Aware Proxy, VPC Service Controls, IAM policies, and fine-grained access controls.

---

## 🎯 Objective

- Design a Zero Trust architecture for enterprise cloud applications.
- Enforce **least privilege access** across users, service accounts, and workloads.
- Implement **context-aware access controls**.
- Validate Zero Trust principles using practical scenarios.

---

## 🏗 Lab Architecture

```mermaid
graph TD
    User[User/Employee] -->|HTTPS + IAP| App[Application Server]
    App --> DB[(Cloud SQL / Firestore)]
    App --> Storage[(Cloud Storage)]
    App --> GKE[GKE Workloads]
    GKE --> DB
    GKE --> Storage
    CloudAudit[Cloud Audit Logs] -.-> SecurityOps[Security Monitoring Team]
