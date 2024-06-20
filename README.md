# K8S Rollout

This repo contains a action to rollout restart k8s deployments in a cluster in:
- DigitalOcean
- Scaleway

## Input

```yaml
inputs:
  vendor:
    description: The vendor to communicate with (digitalocean, scaleway)
    required: true
  cluster-id:
    description: Target cluster ID
    required: true
  rollout-deployments:
    description: List of deployments to rollout (comma or space separated)
    required: false
  rollout-labels:
    description: List of labels to rollout (comma or space separated)
    required: false
  rollout-namespace:
    description: Rollout namespace filter
    required: true

  # Vendor: DigitalOcean
  digitalocean-api-token:
    description: DigitalOcean API token
    required: false

  # Vendor: Scaleway
  scaleway-organization-id:
    description: Scaleway organization ID
    required: false
  scaleway-project-id:
    description: Scaleway project ID
    required: false
  scaleway-region:
    description: Scaleway deployment region (such as nl-ams)
    required: false
  scaleway-access-key:
    description: Scaleway API key ID
    required: false
  scaleway-secret-key:
    description: Scaleway API key secret
    required: false
```

## Usage

```yaml
name: Pull Request
on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  rollout-to-scw:
    steps:
      - uses: wisemen-digital/devops-ga-k8s-rollout@main
        with:
          vendor: scaleway
          cluster-id: ${{ vars.K8S_CLUSTER_ID }}
          rollout-deployments: my-project-api,my-project-web
          rollout-namespace: my-project-development
          scaleway-organization-id: ${{ vars.SCALEWAY_ORGANIZATION_ID }}
          scaleway-project-id: ${{ vars.SCALEWAY_PROJECT_ID }}
          scaleway-region: ${{ vars.SCALEWAY_REGION }}
          scaleway-cluster-id: ${{ vars.K8S_CLUSTER_ID }}
          scaleway-access-key: ${{ secrets.SCALEWAY_ACCESS_KEY }}
          scaleway-secret-key: ${{ secrets.SCALEWAY_SECRET_KEY }}

  rollout-to-do:
    steps:
      - uses: wisemen-digital/devops-ga-k8s-rollout@main
        with:
          vendor: digitalocean
          cluster-id: ${{ vars.K8S_CLUSTER_ID }}
          rollout-labels: app=my-project
          rollout-namespace: my-project-development
          digitalocean-api-token: ${{ secrets.DIGITALOCEAN_API_TOKEN }}
```
