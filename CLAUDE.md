# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working in this repository.

## What this repo is

A two-phase deployment of a Super Mario game (Docker image `sevenajay/mario:latest`) on AWS EKS:

1. **Provision the cluster** — Terraform in `EKS-Terraform/` creates an EKS cluster (`EKS_CLOUD`) and a node group (`Node-cloud`, t2.medium, 1–2 nodes) in `us-east-1`, plus the required IAM roles.
2. **Run the game** — Kubernetes manifests at the repo root (`deployment.yaml`, `service.yaml`) deploy the container (2 replicas, port 80) behind a `LoadBalancer` service.

## Key files

- `EKS-Terraform/main.tf` — cluster + node group + IAM role resources. Uses the default VPC and public subnets in `us-east-1a/b/c/d/f`.
- `EKS-Terraform/provider.tf` — AWS provider, `us-east-1`.
- `EKS-Terraform/backend.tf` — S3 backend, bucket `mario-sp-game`, key `EKS/terraform.tfstate`.
- `deployment.yaml` / `service.yaml` — the Mario Deployment and LoadBalancer Service.
- `script.sh` — bootstrap installer (Terraform, kubectl, AWS CLI) for the Ubuntu instance used for the deployment.
- `Images/`, `Video/` — screenshots and a demo video of the deployment (not code).

## Common commands

Run from an Ubuntu instance with the IAM role from `script.sh`:

```bash
# Install tooling
./script.sh

# Terraform (from EKS-Terraform/)
cd EKS-Terraform
terraform init
terraform validate
terraform plan
terraform apply -auto-approve

# Configure kubectl
aws eks update-kubeconfig --region us-east-1 --name EKS_CLOUD

# Deploy the game
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# Get the game URL
kubectl get service mario-service -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'

# Tear down
kubectl delete -f deployment.yaml -f service.yaml
terraform destroy -auto-approve
```

## Notes

- The S3 backend bucket name (`mario-sp-game`) in `backend.tf` is a placeholder — it must exist (or be created) before `terraform init`, or init will fail.
- The game image is pulled from Docker Hub (`sevenajay/mario:latest`), so nodes need internet access / ECR pull-through for that registry.
- `script.sh` uses `sudo` and installs system packages — it is meant to be run on a fresh Ubuntu EC2 instance, not as part of a build pipeline.