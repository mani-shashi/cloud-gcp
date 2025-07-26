# React App on GKE with Terraform

Project to deploy a React app onto Google Kubernetes Engine with Terraform

## Pre-requisites

- Make sure you have installed [Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli), and [Google Cloud SDK](https://cloud.google.com/sdk/docs/install)
- Also install the `kubectl` CLI tool

```bash
gcloud components install kubectl
terraform -help
```

## Configuration

- Populate `terraform.tfvars`:

```bash
region  = "europe-west2"
zone    = "europe-west2-b"
project = <GCP_PROJECT_ID>
creds   = <PATH_TO_GCP_CREDENTIALS_JSON>
```

## Build/Push the Docker image

```bash
docker build -t gcr.io/<your_project_id>/react-gke-app:v1 .
gcloud auth configure-docker
docker push gcr.io/<your_project_id>/react-gke-app:v1
```

## Create GCP resources

```bash
cd deploy
terraform init
terraform apply
terraform destroy
```

## Deployment

```bash
gcloud container clusters get-credentials react-gke-cluster --region=europe-west2
cd k8s
kubectl apply -f namespace.yml
kubectl apply -f .
kubectl delete -f .
```

- Visit app at `app_ip` e.g. `http://ip-address/`