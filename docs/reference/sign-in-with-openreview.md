# Sign in with OpenReview

Let your users sign in to your application with their OpenReview account. OpenReview acts as a standard **OpenID Connect (OIDC)** identity provider using the **Authorization Code flow with PKCE**, so any spec-compliant OIDC or OAuth 2.0 client library will work.

After a successful sign-in your application receives:

- An **ID token** (a signed JWT) that identifies the user.
- An **access token** you can use to fetch the user's profile claims from the UserInfo endpoint.

{% hint style="info" %}
This guide covers integrations using PKCE (Proof Key for Code Exchange) — the recommended approach for web, mobile, and single-page applications.
{% endhint %}

## Endpoints

All endpoints live under the OpenReview API host. In production:

| Purpose | URL |
| --- | --- |
| Discovery | `https://api2.openreview.net/.well-known/openid-configuration` |
| Authorization | `https://api2.openreview.net/oidc/authorize` |
| Token | `https://api2.openreview.net/oidc/token` |
| UserInfo | `https://api2.openreview.net/oidc/userinfo` |
| JWKS (public keys) | `https://api2.openreview.net/oidc/jwks` |
| Revocation | `https://api2.openreview.net/oidc/revoke` |

The interactive login and consent screens are served from the OpenReview web app at `https://openreview.net`. Your users are redirected there to sign in and approve access.

### Discovery document

Most OIDC libraries configure themselves automatically from the discovery document. Fetch it to see the provider's current capabilities — supported scopes, claims, signing algorithm, and endpoint URLs:

```bash
curl https://api2.openreview.net/.well-known/openid-configuration
```

## Registering your application

Client registration is handled by the OpenReview team. To register, contact OpenReview support and provide:

- **`client_id`** — a unique identifier for your application. You choose this value (for example, `my-conference-portal`); it is included in every request.
- **Application name** — the human-readable name shown to users on the consent screen.
- **Redirect URIs** — the exact callback URLs in your application that OpenReview is allowed to redirect back to after sign-in. Redirect URIs are matched exactly, so list every URL you will use (for example, both your production and staging callbacks).
- **Scopes** — the scopes your application needs (see [Scopes and claims](#scopes-and-claims)).

Once your application is reviewed and approved, you can begin the sign-in flow.

{% hint style="warning" %}
Requests using a `client_id` that has not been approved are rejected. If you receive a "Client is not approved" error, your registration is still pending.
{% endhint %}

## How the flow works

1. Your application generates a **PKCE code verifier** and its **code challenge**, plus a random **`state`** value, and redirects the user's browser to the **authorization endpoint**.
2. OpenReview signs the user in (if they are not already) and asks them to consent to sharing the requested data with your application.
3. OpenReview redirects the browser back to your **redirect URI** with a short-lived authorization **`code`** and your original **`state`**.
4. Your application exchanges the `code` (plus the PKCE **code verifier**) at the **token endpoint** for an **ID token** and an **access token**.
5. Your application validates the ID token and, optionally, calls the **UserInfo endpoint** with the access token to retrieve the user's claims.

```
Your App                 User's Browser              OpenReview
   |                           |                          |
   |-- redirect to /authorize->|-- login + consent ------>|
   |                           |<-- redirect w/ code -----|
   |<-- callback w/ code ------|                          |
   |                                                      |
   |-- POST /token (code + code_verifier) --------------->|
   |<-- id_token + access_token --------------------------|
   |                                                      |
   |-- GET /userinfo (Bearer access_token) -------------->|
   |<-- user claims --------------------------------------|
```

## Scopes and claims

Request scopes as a space-separated list. `openid` is always required.

| Scope | Claims returned | Description |
| --- | --- | --- |
| `openid` | `sub` | Required. `sub` is the user's stable OpenReview profile ID (for example, `~Jane_Doe1`). |
| `profile` | `name` | The user's full name. |
| `email` | `email`, `email_verified` | The user's preferred email address and whether it is verified. |

{% hint style="info" %}
Use **`sub`** as the primary key for the user in your system. It is stable and unique to the OpenReview profile. Names and emails can change.
{% endhint %}

## Walkthrough

### 1. Create the PKCE values and redirect to the authorization endpoint

Generate a `code_verifier` (a high-entropy random string) and derive the `code_challenge` as the base64url-encoded SHA-256 of the verifier:

```bash
# code_verifier: random 32 bytes, base64url-encoded
code_verifier=$(openssl rand -base64 32 | tr '+/' '-_' | tr -d '=')

# code_challenge: SHA-256 of the verifier, base64url-encoded
code_challenge=$(printf '%s' "$code_verifier" \
  | openssl dgst -binary -sha256 \
  | openssl base64 | tr '+/' '-_' | tr -d '=')
```

Store the `code_verifier` and a random `state` in the user's session, then redirect the browser to:

```
https://api2.openreview.net/oidc/authorize
  ?client_id=my-conference-portal
  &redirect_uri=https://myapp.example.com/callback
  &response_type=code
  &scope=openid%20profile%20email
  &state=RANDOM_STATE
  &code_challenge=CODE_CHALLENGE
  &code_challenge_method=S256
```

| Parameter | Required | Notes |
| --- | --- | --- |
| `client_id` | Yes | Your registered client ID. |
| `redirect_uri` | Yes | Must exactly match a registered redirect URI. |
| `response_type` | Yes | Always `code`. |
| `scope` | Yes | Space-separated; must include `openid`. |
| `state` | Recommended | Opaque value echoed back to you; use it to prevent CSRF. |
| `nonce` | Recommended | Random value bound into the ID token to prevent replay. |
| `code_challenge` | Yes | Base64url SHA-256 of your code verifier. |
| `code_challenge_method` | Yes | Always `S256`. |

The user signs in and consents on OpenReview, then is redirected back to your `redirect_uri`:

```
https://myapp.example.com/callback?code=AUTH_CODE&state=RANDOM_STATE
```

Confirm the returned `state` matches the value you stored before continuing.

### 2. Exchange the code for tokens

```bash
curl -X POST https://api2.openreview.net/oidc/token \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "grant_type=authorization_code" \
  -d "code=AUTH_CODE" \
  -d "redirect_uri=https://myapp.example.com/callback" \
  -d "client_id=my-conference-portal" \
  -d "code_verifier=CODE_VERIFIER"
```

Response:

```json
{
  "access_token": "…",
  "id_token": "…",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

The `id_token` is an RS256-signed JWT. Validate it before trusting its contents (see [Validating the ID token](#validating-the-id-token)). Its claims include `sub` and, depending on the requested scopes, `name`, `email`, and `email_verified`, plus the `nonce` you sent.

### 3. Fetch user claims from UserInfo (optional)

If you need the claims outside the ID token, call the UserInfo endpoint with the access token:

```bash
curl https://api2.openreview.net/oidc/userinfo \
  -H "Authorization: Bearer ACCESS_TOKEN"
```

```json
{
  "sub": "~Jane_Doe1",
  "name": "Jane Doe",
  "email": "jane@example.edu",
  "email_verified": true
}
```

{% hint style="info" %}
Because OpenReview is a standard OIDC provider, most OIDC and OAuth 2.0 client libraries can handle PKCE, discovery, and token validation for you — point the library at the discovery URL above and configure it as a generic OIDC provider.
{% endhint %}

## Validating the ID token

If you validate the ID token yourself instead of relying on a library, check that:

- The signature verifies against a key from the JWKS endpoint (`https://api2.openreview.net/oidc/jwks`). Tokens are signed with **RS256**.
- `iss` equals `https://api2.openreview.net`.
- `aud` equals your `client_id`.
- The token is not expired (`exp`).
- `nonce` matches the value you sent in the authorization request.

## Token lifetime

- Access tokens and ID tokens are valid for **1 hour** (`expires_in: 3600`).
- **Refresh tokens are not issued.** Only the `authorization_code` grant is supported. When a token expires, send the user through the authorization flow again. If they still have an active OpenReview session and have already granted consent, this re-authentication is seamless and requires no additional interaction.

### Revoking an access token

To proactively invalidate an access token (for example, on user sign-out):

```bash
curl -X POST https://api2.openreview.net/oidc/revoke \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "token=ACCESS_TOKEN"
```

Per [RFC 7009](https://www.rfc-editor.org/rfc/rfc7009), this endpoint always returns `200 OK`, whether or not the token existed.

## Security best practices

- Always send a random **`state`** and verify it on the callback to protect against CSRF.
- Always send a **`nonce`** and check it in the ID token to protect against replay.
- Generate a fresh **`code_verifier`** per authorization request and never reuse it.
- Register the **narrowest set of redirect URIs** you need; they are matched exactly.
- Request only the **scopes your application actually uses**.

## Troubleshooting

| Error | Likely cause |
| --- | --- |
| `Client is not approved` | Your `client_id` registration is still pending, or was rejected. |
| `Invalid client_id` | The `client_id` is unknown. Check for typos. |
| `Invalid redirect_uri` | The `redirect_uri` does not exactly match a registered value. |
| `Scope must include openid` | The `scope` parameter is missing `openid`. |
| `Unsupported scope: …` | You requested a scope not granted to your client. |
| `code_challenge is required for public clients` | PKCE parameters were missing from the authorization request. |
| `invalid_grant` (Invalid or expired authorization code) | The code was already used, expired (10-minute lifetime), or the `redirect_uri`/`client_id` did not match the original request. |
| `invalid_grant` (Invalid code_verifier) | The `code_verifier` does not match the `code_challenge` sent during authorization. |
