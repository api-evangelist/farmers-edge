---
name: Onboard a FarmCommand self-serve client
description: Register, validate, and verify a new self-serve FarmCommand client account.
api: openapi/farmers-edge-farmcommand-openapi-original.json
operations:
  - client_validate-username_list
  - client_selfserveclient_create
  - client_selfserveclient_verify_create
  - client_selfserveclient_resend-verification-email_create
---

# Onboard a FarmCommand self-serve client

Use the public FarmCommand API (host `admin.farmcommand.com`) to sign a new customer up.

## Auth
Most public onboarding calls are unauthenticated create endpoints; authenticated calls send the
FarmCommand API token as the `token` **query parameter** (scheme `FarmCommand Token`). See
`authentication/farmers-edge-authentication.yml`.

## Steps
1. **Check the username is free** — `GET /client/validate-username` (`client_validate-username_list`).
   Only proceed if the username is reported available.
2. **Create the client** — `POST /client/selfserveclient/` (`client_selfserveclient_create`) with the
   `SelfServeClient` body (username, email, first/last name, company, address, product/service offering).
   A `201` returns the created self-serve client; a `400` means the payload failed validation — fix and retry.
3. **Verify the email** — the client receives a token by email; submit it to
   `POST /client/selfserveclient/verify` (`client_selfserveclient_verify_create`).
4. **Resend if needed** — if the verification email is lost, call
   `POST /client/selfserveclient/resend-verification-email`
   (`client_selfserveclient_resend-verification-email_create`).

## Errors
`400` validation, `404` client not found, `429` too many requests (back off). Errors are DRF-style
JSON, not RFC 9457 — see `errors/farmers-edge-problem-types.yml`.
