# Basic example configuration for Keycloak with One Login as a federated OIDC provider, Caddy and Oauth2-Proxy as an application gateway

- Caddy reverse proxies traffic & enforces authorisation
- Keycloak authorises and sets roles
- One Login provides the authentication
- Oauth2-Proxy maps claims to headers, handles OIDC for Caddy
- Example application which echoes back header information (`containous/whoami`)

This setup is useful to protect sites that don't have built-in support for OIDC. In the example there is path based security setup and two roles - unauthenticated users cannot access the app at all, authenticated users can access the app but only users who have the `polis-admin` role in Keycloak can access `http://caddy.local/admin`

In principle you can configure another application that supports OIDC to authenticate directly with KeyCloak, this could be protected with a basic reverse_proxy configuration in Caddy.

## Setup

Add the following to the host computer (laptop) `/etc/hosts`

```
127.0.0.1    caddy.local
127.0.0.1    keycloak
```

Run `docker compose up`

Import the Keycloak realm and client into Keycloak

Copy the Keycloak realm key (S256) into One Login and update the Client ID in Keycloak to your own from One Login.

visit http://caddy.local - it should redirect to sign-in

Should be a register link, create two users

In Keycloak verify their emails, add one to the `polis-admins` group

`polis-admin` user should be able to visit http://caddy.local/admin - other user should get a 403 but should be able to visit other paths on the site

## Gotchas

There are example keys in the project so that it will work locally out of the box. **Make sure to [generate your own cookie secret](https://oauth2-proxy.github.io/oauth2-proxy/configuration/overview/#generating-a-cookie-secret) if you want to use this configuration for deployment**

### Caddy

Response matchers (used in `handle_response`) are very limited. Request matchers (used in `handle`) are needed to access e.g. `not`

`forward_auth` can only have response directives
