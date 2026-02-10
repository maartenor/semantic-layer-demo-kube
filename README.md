# Semantic Layer Demo - Kubernetes Deployment

Kubernetes manifests for deploying the [semantic-layer-business-layer](https://github.com/maartenor/semantic-layer-business-layer) application. This is a mock Collibra-like Data Governance REST API and web UI built with Flask, serving a three-layer data model (Business, Logical, Technical) with interactive visualizations.

The deployment uses kustomize with a shared base and separate dev/prod overlays. No container image is pre-built; an init container clones the Git repository and installs Python dependencies at pod startup.


> ⚠️ **Warning: Visualization and Assets pages may appear empty on initial load.**
>
> The relationship model and asset data can be lost by the server on first render. To fix this, either switch between the tabs at the top of the page or press the **Reload Data** button.



## Dev vs Prod overlay differences

| | Dev | Prod |
|---|---|---|
| Namespace | `semlaydemo-dev` (created) | `webserver` (existing) |
| Host | `demo.maartenor.org` | `semantic-layer-demo.maartenor.org` |
| TLS secret | `demo-maartenor-org-tls` | `semantic-layer-demo-tls` |
| Replicas | 1 | 2 |
| CPU request | 50m | 100m |
| Memory limit | 256Mi | 512Mi |
| IngressRoute priority | 105 | 105 |

## Project structure

```
base/                  # Shared deployment, service, ingressroute
overlays/
  dev/                 # Dev overlay (creates own namespace + middleware)
  prod/                # Prod overlay (uses existing webserver namespace)
```
