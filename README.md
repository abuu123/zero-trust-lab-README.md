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
    GKE --> Storage| Persona         | Role            | Permissions                      | Notes                                        |
| --------------- | --------------- | -------------------------------- | -------------------------------------------- |
| Dev             | developer       | Custom Dev Role                  | Can deploy apps to dev environment only      |
| SecOps          | security team   | Security Admin                   | Monitor audit logs, manage firewall & VPC SC |
| AppService      | service account | Cloud SQL Client, Storage Reader | Used by app workloads only                   |
| ExternalPartner | partner         | Limited IAP access               | Read-only access to app endpoints            |

#Enable IAP for an App

    CloudAudit[Cloud Audit Logs] -.-> SecurityOps[Security Monitoring Team]gcloud iap web enable --project=your-project --resource-type=app-engine

#Create VPC SC Perimeter
gcloud access-context-manager perimeters create zero-trust-perimeter \
    --title="Zero Trust Perimeter" \
    --resources=projects/YOUR_PROJECT_ID

##Assign Least Privilege IAM Roles

gcloud projects add-iam-policy-binding YOUR_PROJECT_ID \
    --member="user:dev@example.com" \
    --role="roles/viewer"

##Enable Workload Identity Federation
gcloud iam workload-identity-pools create my-pool \
    --project=YOUR_PROJECT_ID







