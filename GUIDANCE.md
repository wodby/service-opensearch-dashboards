# OpenSearch Dashboards on Wodby

What Wodby sets up for this service. It runs the official OpenSearch Dashboards image, configured through environment variables and one config file set by the manifest.

## Linked OpenSearch

The `opensearch` link is required. It sets:

| Variable | Meaning |
| --- | --- |
| `OPENSEARCH_HOSTS` | the linked OpenSearch service over HTTPS |
| `OPENSEARCH_USERNAME` | the `kibanaserver` server account |
| `OPENSEARCH_PASSWORD` | the password Wodby generated for that account on the OpenSearch service |

The OpenSearch service sets the `kibanaserver` password itself each time it starts. Nothing has to be configured by hand. Certificates are not verified because OpenSearch serves the image's self-signed demo certificate.

The major and minor version of this service must match the linked OpenSearch service.

## Signing in

- The web interface is on port `5601`, the service's main HTTP endpoint, and has a public HTTPS address.
- People sign in with OpenSearch users. The built-in administrator is `admin`; its password is the `password` token of the linked OpenSearch service. Create other users under **Security** in Dashboards.
- The address accepts sign-ins from anywhere. Do not turn on Wodby Basic Auth for this service: Dashboards reads the `Authorization` header for its own login. To restrict who can reach the address, use App Access.

## What is generated

`cookie_password` is generated once and encrypts session cookies. Replacing it signs users out.

## Changing configuration

The config file `opensearch_dashboards.yml` is a service config you can override. Keep `opensearch_security.cookie.secure: true`, because the public address always uses HTTPS. Environment variables set by the link take precedence over the same settings in the file.

## Data

Dashboards has no volume. Dashboards, index patterns and other saved objects are stored in the linked OpenSearch, on its volume.

## Check the result

- `GET http://<app service name>:5601/app/login` answers `200` when the server is ready.
- "OpenSearch Dashboards server is not ready yet" means OpenSearch is unreachable or the `kibanaserver` password is not set yet: check the OpenSearch service first.
