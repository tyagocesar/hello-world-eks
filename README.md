# hello-world-eks

Technical challenge: containerize an application and deploy it to Kubernetes on AWS EKS using Helm and GitHub Actions.

## Architecture

```
  push to main          workflow_dispatch
 ──────────────►   ◄────────────────────────
       │                       │
       ▼                       ▼
  ┌─────────┐          ┌──────────────┐
  │  CI     │          │    CD        │
  │ Build + │          │ helm upgrade │
  │  Push   │          │  → EKS       │
  └─────────┘          └──────────────┘
       │                       ▲
       │  Docker Hub image      │
       └───────────────────────┘
```

## Stack

| Layer | Tool |
|---|---|
| App | Python 3.11 / Flask |
| Container | Docker |
| Orchestration | Kubernetes (EKS) |
| IaC | Terraform |
| Package manager | Helm |
| CI/CD | GitHub Actions |

## Project Structure

```
.
├── app.py                        # Flask app
├── Dockerfile
├── requirements.txt
├── helm/                         # Helm chart (Deployment, Service, Ingress, HPA)
│   ├── Chart.yaml
│   ├── values.yaml
│   └── templates/
└── .github/workflows/
    ├── ci.yml                    # Build + push image to Docker Hub on push to main
    └── cd.yml                    # Deploy via Helm to EKS on workflow_dispatch
```

## API

```
GET /    Returns "Hello, World from Kubernetes!"
```

## Prerequisites

**GitHub Secrets:**
| Secret | Description |
|---|---|
| `DOCKER_HUB_USERNAME` | Docker Hub username |
| `DOCKER_HUB_TOKEN` | Docker Hub access token |
| `AWS_ACCESS_KEY_ID` | AWS access key |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key |
| `REPO` | Docker Hub repo (e.g. `youruser`) |

**GitHub Variables:**
| Variable | Description |
|---|---|
| `AWS_REGION` | AWS region (e.g. `us-east-1`) |
| `CLUSTER_NAME` | EKS cluster name |

## Local Development

```bash
pip install -r requirements.txt
python app.py
curl http://localhost:5000
```

## Manual Deploy

```bash
helm upgrade --install desafio ./helm \
  --namespace staging --create-namespace \
  --set image.repository=<DOCKER_HUB_USER>/desafio \
  --set image.tag=latest
```

## Infrastructure

The `infra/iacm_eks` directory contains a Terraform module to provision the EKS cluster used in this challenge. See its README for usage.
