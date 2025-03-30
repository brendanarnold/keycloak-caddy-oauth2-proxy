# Basic configuration for Keycloak, Caddy and Oauth2-Proxy as an application gateway

- Caddy reverse proxies traffic & enforces authorisation
- Keycloak authorises and sets roles
- Oauth2-Proxy maps claims to headers, handles OIDC for Caddy

## Keycloak setup

- Keycloak to supply the 'group' claim with user groups
- Users need to be email verified
- Client ID is oauth2-proxy in this case
- Import `realm-export.json` to re-create the realm
- Import `oauth2-proxy-client-export.json` to re-create the client (oauth2-proxy)

## Gotchas

### Caddy

Response matchers (used in `handle_response`) are very limited. Request matchers (used in `handle`) are needed to access e.g. `not`

`forward_auth` can only have response directives
