zero-trust-architecture-gcp/
│
├── README.md
├── architecture-diagram.png
│
├── docs/
│   ├── zero-trust-principles.md
│   ├── threat-model.md
│   ├── blast-radius-analysis.md
│
├── scripts/
│   ├── enable-iap.sh
│   ├── create-vpc-sc.sh
│   ├── iam-roles-setup.sh
│   ├── workload-identity-setup.sh
│
├── exercises/
│   ├── exercise-1-iap-access-control.md
│   ├── exercise-2-vpc-perimeter-test.md
│   ├── exercise-3-breach-simulation.md
│
└── terraform/
    ├── main.tf
    ├── iam.tf
    ├── variables.tf

# 🔐 Zero Trust Principles Applied in GCP

This lab applies the following Zero Trust principles:

## 1. Verify Explicitly
All access requests are authenticated and authorized using:
- Identity-Aware Proxy (IAP)
- IAM role validation
- Context-aware access policies

## 2. Least Privilege Access
Permissions are:
- Resource-scoped
- Custom-role defined
- Time-restricted where possible

## 3. Assume Breach
Security monitoring assumes compromise:
- IAM audit logging enabled
- Service account impersonation tracked
- VPC SC perimeter violations logged

## 4. Identity is the New Perimeter
Access is granted based on:
- User identity
- Role binding
- Context (IP/device/location when applicable)# 🚨 Threat Model – Zero Trust GCP Environment

## Assets
- Cloud SQL database
- Cloud Storage buckets
- GKE workloads
- Compute Engine instances

## Threat Actors
- Compromised developer credentials
- Malicious insider
- External attacker with leaked service account key

## Attack Vectors
- Privilege escalation via IAM
- Service account impersonation
- Lateral movement across VPC
- Data exfiltration outside perimeter

## Mitigations Implemented
- IAM Conditions
- VPC Service Controls
- Workload Identity Federation# 🧱 Blast Radius Reduction Analysis

## Without Zero Trust
- Broad Editor roles
- Public service endpoints
- Shared service accounts
- No access segmentation

Impact:
Compromised identity could access entire project.

## With Zero Trust Controls
- Custom least-privilege roles
- Service account isolation
- IAP enforcement
- VPC Service Controls perimeter

Impact:
Compromised identity limited to specific scoped resource.
- Audit log monitoring

- #!/bin/bash

PROJECT_ID=$1

echo "Enabling IAP..."

gcloud services enable iap.googleapis.com --project=$PROJECT_ID

echo "IAP Enabled"
#!/bin/bash

PROJECT_NUMBER=$1

gcloud access-context-manager perimeters create zero-trust-perimeter \
    --title="Zero Trust Perimeter" \
    --resources=projects/$PROJECT_NUMBER

echo "VPC Service Control Perimeter Created"# 🚨 Exercise 3 – Breach Simulation

## Scenario

A developer account is compromised.

Attacker attempts to:
- Access Cloud Storage outside perimeter
- Modify IAM bindings
- Impersonate service account

## Expected Result

- VPC SC blocks exfiltration
- IAM logs record SetIamPolicy attempt
- Monitoring alerts triggered

## Validation

Run:
provider "google" {
  project = var.project_id
  region  = "us-central1"
}

resource "google_project_iam_binding" "dev_viewer" {
  project = var.project_id
  role    = "roles/viewer"

  members = [
    "user:dev@example.com",
  ]
}
gcloud logging read "protoPayload.methodName=SetIamPolicy"






- 

- 




    
