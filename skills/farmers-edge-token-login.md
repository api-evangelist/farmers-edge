---
name: Authenticate a FarmCommand product user (token login)
description: Exchange product-user credentials for a FarmCommand API token, then reset a password if needed.
api: openapi/farmers-edge-farmcommand-openapi-original.json
operations:
  - token-login_create
  - labcommand_token-login_create
  - gridcalc_token-login_create
  - recengine_token-login_create
  - hefty_token-login_create
  - client_password_reset_create
  - client_password_reset_change_create
---

# Authenticate a FarmCommand product user (token login)

FarmCommand exposes a token-login endpoint per product; each returns an API token on success.

## Steps
1. **Log in** for the relevant product to obtain a token:
   - FarmCommand — `POST /token-login/` (`token-login_create`)
   - LabCommand — `POST /labcommand/token-login` (`labcommand_token-login_create`)
   - GridCalc — `POST /gridcalc/token-login/` (`gridcalc_token-login_create`)
   - RecEngine — `POST /recengine/token-login/` (`recengine_token-login_create`)
   - Hefty — `POST /hefty/token-login/` (`hefty_token-login_create`)
   A `401 Invalid Authentication` means the credentials were rejected.
2. **Use the token** on subsequent calls as the `token` query parameter.
3. **Reset a forgotten password** — request a reset email with
   `POST /client/password/reset/` (`client_password_reset_create`), then submit the emailed token with
   `POST /client/password/reset/change/` (`client_password_reset_change_create`).

## Notes
Rate limiting is signaled with `429 Too Many Requests`; retry after a delay. See
`conventions/farmers-edge-conventions.yml`.
