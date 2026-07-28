# Dev Databases on GKE (Postgres, Redis, Cosmos Emulator)

This Helm chart deploys **Postgres**, **Redis**, and the **Azure Cosmos DB Emulator** into a GKE cluster for development, behind a single **NGINX Ingress Controller** and **Let’s Encrypt TLS**.

## Components

- **Namespace:** `dev-databases`
- **Postgres:** Stateful DB with PVC and credentials from Kubernetes Secret
- **Redis:** In-memory cache, single replica
- **Cosmos Emulator:** Local Cosmos DB-compatible endpoint with PVC
- **Ingress:** NGINX-based, one public IP, hostnames for services
- **TLS:** Managed by cert-manager + Let’s Encrypt (optional but recommended)

## Security Model

- **No secrets in Git.**
- **No passwords in Helm values or templates.**
- All sensitive values (Postgres user/password/db, Cosmos key) are stored in a Kubernetes Secret named `db-secrets`, created manually or via CI/CD.

### Create secrets manually

Run this once (or via pipeline):

```bash
kubectl create namespace dev-databases --dry-run=client -o yaml | kubectl apply -f -

kubectl create secret generic db-secrets \
  --namespace dev-databases \
  --from-literal=POSTGRES_USER=devuser \
  --from-literal=POSTGRES_PASSWORD=devpassword \
  --from-literal=POSTGRES_DB=devdb \
  --from-literal=COSMOS_KEY="C2FBB...=="
```
