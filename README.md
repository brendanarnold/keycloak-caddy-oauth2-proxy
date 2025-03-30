# Basic configuration for Keycloak, Caddy and Oauth2-Proxy as an application gateway

- Caddy reverse proxies traffic & enforces authorisation
- Keycloak authorises and sets roles
- Oauth2-Proxy maps claims to headers, handles OIDC for Caddy

## Setup

Add `127.0.0.1    caddy.local` to `/etc/hosts`

Run `docker compose up`

Import the Keycloak realm and client into Keycloak

visit http://caddy.local - it should redirect to sign-in

Should be a register link, create two users

In Keycloak verify their emails, add one to the `polis-admins` group

`polis-admin` user should be able to visit http://caddy.local/admin - other user should get a 403 but should be able to visit other paths on the site

## Gotchas

### Caddy

Response matchers (used in `handle_response`) are very limited. Request matchers (used in `handle`) are needed to access e.g. `not`

`forward_auth` can only have response directives
