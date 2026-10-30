# OpenSearch Dashboards service for Kubernetes on Wodby

Run OpenSearch Dashboards as a reusable Kubernetes application service with Wodby.

This repository defines the Wodby service manifests and operational
configuration for OpenSearch Dashboards.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [OpenSearch application stack](https://github.com/wodby/stack-opensearch)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `opensearch-dashboards` |
| Type | Application service |
| Versions | `3.8` by default |
| Workloads | `main` (Deployment, primary) |
| Containers | `opensearch-dashboards` using `opensearchproject/opensearch-dashboards` |
| Endpoints | `opensearch-dashboards`: HTTP 5601 (main) |
| Service links | OpenSearch (`opensearch`), required |
| Application build | Not buildable from application source |
| Helm | chart `oci://registry-1.docker.io/wodby/stateless`; version `0.4.0` |

## Use this service

Use this service through [OpenSearch application stack](https://github.com/wodby/stack-opensearch), or reference it from a
custom Wodby stack.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

## Authentication

The required OpenSearch link supplies the `kibanaserver` credentials. The
linked OpenSearch service sets that account's password each time it starts.
Sign in to the web interface as `admin` using the linked OpenSearch service's
generated `password` token.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Keep service, workload, container, endpoint, link, volume, config, and
derivative names stable unless dependent stacks and app-level overrides are
updated at the same time.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
