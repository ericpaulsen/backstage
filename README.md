# Spotify Backstage on AKS

My deployment of [backstage.io](https://backstage.io/) on a Kubernetes cluster.

## Suppporting infrastructure

- AKS cluster
- PostgreSQL database
- Azure Application Gateway Ingress Controller

## PostgreSQL

This is a standard PostgreSQL deployment, with a Persistent Volume for storage, a Secret
with authentication credentials, and a Service for Backstage <-> PostgreSQL networking.

## Application Gateway

The Azure Application Gateway is installed via Helm, using the `helm-config.yaml` as the values file.
It watches for any `Ingress` resources created in the cluster and provisions a load balancer in order
to serve traffic to Backstage. The App Gateway `IngressClass` is set on the Backstage Ingress resource
to wire things up:

```yaml
ingressClassName: azure-application-gateway
```

## Backstage configuration

Backstage is compiled into the `ericpaulsen/backstage:latest` image, with the configuration living in `/app/app-config.yaml`.
[Here's the Backstage documentation on building the application into a Docker image](https://backstage.io/docs/deployment/docker).
