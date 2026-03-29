
---

# 📄 2. Kubernetes README (`k8s/README.md`)

```markdown
# ☸️ Kubernetes Deployment

This folder contains Kubernetes manifests used to deploy the FastAPI application.

---

## 📄 deployment.yaml Overview

This file defines:

- Deployment → runs the application
- Service → exposes the application externally

---

## 🔍 Deployment Breakdown

```yaml
apiVersion: apps/v1
kind: Deployment