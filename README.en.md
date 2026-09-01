# [🇷🇴 Română](README.md) | [🇬🇧 English](README.en.md)
# Details on Integrating with the ROeID System

## SSO Trust Model

In order to establish a partnership between the ROeID Platform and e-Government Service Providers, collaboration agreements/protocols must be concluded to ensure trust and the secure transfer of identities and authentication credentials between the ROeID Platform and e-Government systems. Concluding these agreements/protocols allows e-Government Service Providers to make access decisions for their systems based on the trust level defined within the ROeID Platform.

## First Step

From a technical perspective, once the collaboration protocol is signed, the first step is to submit a request to the ADR team for test and production environment credentials, specifying your **redirect_url**.

> **WARNING!**
> `redirect_url` represents the address where the ROeID system redirects the user back to your platform. It is **mandatory** to request this element in advance; it can be updated later upon request.

## Single Sign-On (SSO) Authentication

Single Sign-On (SSO) access establishes a multi-party authentication relationship that abstracts technical differences across entities.

Within this trust federation, the ROeID Platform acts as the Identity Provider (IdP), managing user identities and guaranteeing authentication. e-Government Service Providers (SPs) authorize access to their services based on user attributes received from the Identity Provider (ROeID).

Transactions between federation partners enable:

* Secure exchange of user identification information between partners.
* Association between the user's identity on the IdP side (ROeID) and the user's account in the Service Provider's system.
* Single sign-on functionality across partner websites within the SSO circle of trust.
* Controlled access to Service Provider resources based on Identity Provider assertions.
* Interoperability across heterogeneous environments.

SSO authentication can be implemented using either of the following two secure communication standards / authentication protocols:

* Security Assertion Markup Language (SAML) 2.0 – https://docs.oasis-open.org/security/saml/Post2.0/sstc-saml-tech-overview-2.0.html
* OpenID Connect (OIDC) – https://openid.net

## Security Assertion Markup Language (SAML) Protocol – General Overview

SAML is an XML-based framework developed by OASIS (Organization for the Advancement of Structured Information Standards) for exchanging authentication and authorization data.

SAML transmits user credentials from an IdP (which holds and manages identities) to verify access rights with an SP. Service Providers require authentication before granting access to protected resources. Each user (or group) has attributes that define profile data and specify access permissions.

SAML relies on XML metadata documents and assertions (SAML Tokens) to verify identity and privileges.

Developers use SAML plugins to enable secure SSO login experiences where cryptographically signed assertions determine access rights. SAML assertions can be:

* **Authentication Assertion:** Confirms user authentication, timestamp, and authentication method.
* **Authorization Assertion:** Verifies whether the user is authorized to consume the service or if access was denied due to failed authentication.
* **Attribute Assertion:** Transmits specific user attributes to the Service Provider.

## OpenID Connect Protocol – General Overview

OpenID Connect (OIDC) extends OAuth 2.0 to allow client applications to verify user identity and retrieve profile information via RESTful APIs that exchange JSON Web Tokens (JWT). Identity Providers act as authorization and authentication servers. This approach is widely adopted due to its scalability, cross-platform flexibility, and JSON-native implementation.

**User Authentication:** A user authenticates and receives an access token alongside an ID token from the authorization server. The access token allows the client application to query consenting user claims from the protected `/userinfo` endpoint.

**Similarities:**

* Both are identity protocols enabling Single Sign-On.
* Both are mature, secure, and standardized.
* Users authenticate once at the IdP (ROeID) to access trusted applications.
* The end-user login workflow is practically identical in the browser.

**Differences:**

* OpenID Connect uses JSON/REST payloads instead of XML, simplifying integration.
* OIDC focuses primarily on identity assertions (ID tokens), while SAML supports rich authorization assertion profiles.
* SAML uses XML assertions; OIDC uses JSON Web Tokens (JWT).
* OIDC is optimized for API-driven architectures and mobile applications.

### Use Cases

* **Mobile Applications:** OIDC is strongly recommended due to its lightweight JSON footprint and extensive client library ecosystem.
* **Legacy / Enterprise Portals:** SAML 2.0 is ideal if existing enterprise applications or COTS products already include built-in SAML service provider modules.
* **API Protection:** Exposing and securing backend APIs requires OAuth 2.0 / OIDC.

### Using OIDC and SAML Together

These protocols are not mutually exclusive. Organizations can deploy SAML for internal portal SSO while leveraging OpenID Connect for modern web and mobile applications.

## ROeID Authentication Workflow

Enrolling e-Government services in ROeID provides citizens with a centralized service catalog accessible via SSO.

When accessing a service, existing users are matched via a unique identifier (such as the Romanian national identification number – CNP), while first-time users undergo a streamlined enrollment workflow based on claims provided by ROeID after explicit user consent.

Technical onboarding steps:

1. The Service Provider and ADR agree on the authentication protocol (SAML 2.0 or OpenID Connect).
2. Both parties establish:
* Specific authentication parameters and endpoints.
* User claims/attributes to be released upon user consent (using CNP as the primary unique matching key).
* Provisioning policies: whether accounts are created automatically upon first login or via custom onboarding forms.
* User notification policies regarding account provisioning.



### Onboarding Procedure via OpenID Connect (OIDC)

Onboarding via OIDC requires:

* Defining the user claims requested from ROeID.
* Specifying secure communication endpoints and exchanging encryption/signing keys.

On the ROeID Platform side, configuration involves:
a) Attribute mapping under the Authorization Provider claims settings (e.g., `telefon` $\leftrightarrow$ `mobile`, `prenume` $\leftrightarrow$ `givenName`).

b) Assigning the default scope containing the supported ROeID claims.

c) Registering a new OpenID Client with:

* `ServiceProviderName`: Service provider name.
* `Client ID`: Auto-generated identifier.
* `Client Secret`: Auto-generated secret.
* `Logo URL`: Link to the service provider's logo.
* `Authorization Provider`: IDP.
* `Scopes`: `openid` (default) + custom defined scopes.
* `Redirect URIs`: Authorized callback URL whitelist.
* `Encryption Keys`: Default ROeID platform certificates or client-provided public keys.

Metadata provided to the Service Provider:

* Client Name, Client ID, Client Secret, Scope Names
* Discovery Endpoints (Issuer, Authorization, Token, UserInfo, Introspection, Revocation, JWKS, Provider Metadata).

On the Service Provider side:

1. Integrate an OIDC-certified client library (see https://openid.net/developers/certified/).
2. Map the incoming SSO session to the internal application session.
3. (Optional) Automate account provisioning from `/userinfo` claims.
4. Add the official "Connect with ROeID" button to the login interface.

### Onboarding Procedure via SAML 2.0

Configuring SAML 2.0 within the ROeID IDP federation requires:

1. Creating a Partnership Entity in ROeID.
2. Defining `Entity ID`, `Entity Name`, `Assertion Consumer Service (ACS) URL`, and optional `Single Logout (SLO) URL`.
3. Exchanging X.509 signing and encryption certificates.
4. Setting up attribute release policies and assertion encryption (`Require Signed AuthN Request`, `Encrypt NameID`, `Encrypt Assertion`).
5. Integrating a standard SAML 2.0 SP toolkit (e.g., https://www.samltool.com/toolkits.php).

## Publishing Services on roeid.ro and the ROeID Mobile App

To list an e-service in the ROeID catalog, submit the following metadata to ADR:

1. **Short Name:** Maximum 12 characters (e.g., `ghiseul`).
2. **Icon:** 300x300 PNG with a **transparent background** (for light/dark theme compatibility).

3. **Short Description:** Up to 60 characters (e.g., *Pay taxes online to enrolled public institutions*).
4. **Long Description:** 100–200 words displayed on the detail screen.
5. **Initiation URL:** Direct URL redirecting to the ROeID login entry point (e.g., `[https://www.ghiseul.ro/ghiseul/public/login-pscid](https://www.ghiseul.ro/ghiseul/public/login-pscid)`).
6. **Redirect URI:** Target callback address after successful authentication.

## Supported Claims and Data Format

ROeID provides verified citizen identity attributes upon consent. Discovery endpoints (e.g., `[https://sso.roeid.ro/affwebservices/CASSO/oidc/demo/.well-known/openid-configuration](https://sso.roeid.ro/affwebservices/CASSO/oidc/demo/.well-known/openid-configuration)`) expose supported claims:

```json
"claims_supported": [
  "scara",
  "cnp",
  "judet",
  "nr",
  "prenume",
  "nume complet",
  "nume",
  "bloc",
  "apartament",
  "telefon",
  "etaj",
  "adresa",
  "strada",
  "sector",
  "localitate",
  "email"
]

```

## ROeID Visual Identity

### Color Palette

*  `#000000` Black
*  `#09338a` Button hover
*  `#2e5ae6` Royal blue
*  `#8298db` Cornflower blue
*  `#f4f4f4` Light gray
*  `#f2b62c` Goldenrod
*  `#f1f3f8` Ghost white
*  `#d1d9e8` Lavender

Integrate the official button style using the demo assets at https://api.roeid.ro/public/demo.html. Use the primary logo for light backgrounds and the secondary logo for dark themes. The button text must state exactly `Conectare cu ROeID`.

Official typography: **Sora** ([Google Fonts](https://fonts.googleapis.com/css2?family=Sora:wght@100;200;300;400;500;600;700;800&display=swap)).

Official SVG vector logos:

* [Primary Logo (Dark/Blue)](https://github.com/roeid-ro/integrare/blob/main/html/roeid_logo_primary.svg)
* [Secondary Logo (Negative/White)](https://github.com/roeid-ro/integrare/blob/main/html/roeid_logo_secondary.svg)

## Mandatory Notice Alongside the ROeID Button

Partner websites must prominently display the following notice adjacent to the login button:

```text
Vă recomandăm să folosiți aplicația ROeID pentru conectarea la sistemul ______. 
ROeID este o aplicație pe telefonul mobil pusă la dispoziție de către Autoritatea pentru Digitalizarea României, 
aplicație notificată la Comisia Europeană ca modalitatea oficială de identificare electronică in România.

```

Hyperlink requirements:

* "ROeID" $\rightarrow$ [https://www.roeid.ro/](https://www.roeid.ro/)
* "Autoritatea pentru Digitalizarea Romaniei" $\rightarrow$ [https://adr.gov.ro/](https://adr.gov.ro/)
* "Comisia Europeana" $\rightarrow$ [https://ec.europa.eu/digital-building-blocks/sites/display/EIDCOMMUNITY/Romania+-+ROeID](https://ec.europa.eu/digital-building-blocks/sites/display/EIDCOMMUNITY/Romania+-+ROeID)

## ROeID Test Environment

A pre-production environment is provided for integration testing. Download the Android test authenticator APK here:

[Google Drive Test App Folder](https://drive.google.com/drive/folders/1wnWUCj61-x-LSnSpgq2LZflMHQQy0xPz?usp=sharing)

> **Note:** Production builds from Google Play and Apple App Store cannot authenticate against the test environment. In the test environment, verification is automatically approved if the submitted selfie matches the test ID card.

Simulate end-to-end integration via the demo Relying Party: `[http://demo.beta.roeid.ro/sts](http://demo.beta.roeid.ro/sts)`.

## Step-by-Step Browser Authentication Trace

Traced from the Ghișeul.ro test integration:

```http
1. GET https://www.ghiseul.ro/testare/ghiseul
2. GET https://www.ghiseul.ro/testare/ghiseul/public/login-pscid 
3. GET https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/ghiseul/authorize?response_type=code&client_id=ghiseul&scope=openid+profile+ghiseul&state=1698308536-441d47&redirect_uri=https%3A%2F%2Fwww.ghiseul.ro%2Ftestare%2Fghiseul%2Fpublic%2Flogin-pscid%2Findex
4. GET https://sso.beta.roeid.ro/affwebservices/secure/secureredirect?response_type=code&client_id=ghiseul&scope=openid+profile+ghiseul&state=1698308536-441d47&redirect_uri=https%3A%2F%2Fwww.ghiseul.ro%2Ftestare%2Fghiseul%2Fpublic%2Flogin-pscid%2Findex&SMPORTALURL=...
5. GET https://sso.beta.roeid.ro/siteminderagent/forms/shim.fcc?...
   # User credentials entry form displayed
6. POST https://sso.beta.roeid.ro/arcotafm/MasterController.jsp?profile=ldapandpush&tokenID=...
   # 2FA Push notification prompt displayed
7. GET https://sso.beta.roeid.ro/arcotafm/getOTTFromPushSMToken.jsp?txnId=...
   # Polling until mobile 2FA approval is received
8. POST https://sso.beta.roeid.ro/arcotafm/MasterController.jsp?profile=ldapandpush&tokenID=...
9. GET https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/userconsent
   # User consent form rendered
10. POST https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/userconsent
11. GET https://sso.beta.roeid.ro/affwebservices/CASSO/oidc/ghiseul/authorize?SMASSERTIONREF=QUERY&response_type=code&...
12. GET https://www.ghiseul.ro/testare/ghiseul/public/login-pscid/index?code=...&state=1698308536-441d47
   # Final redirect back to the Service Provider callback with authorization code

```

## Edge Cases to Handle

Flag and persist ROeID-originated accounts in your database. Accessing ROeID-verified records through unverified bypass channels represents a security vulnerability.

* **New Users:** Provision account from `/userinfo` claims and prompt for additional application-specific fields if required.
* **Existing Users:** Match records using `cnp` or verified `email`.
* **Multiple Accounts per CNP:** Match using verified email addresses.
* **Natural vs. Legal Person Profiles:** Render an account selection screen immediately post-login (e.g., PCUe implementation: [https://edirect.e-guvernare.ro/](https://edirect.e-guvernare.ro/)).

## Single Logout (SLO)

It is recommended to invalidate only the local application session on standard logout.

To terminate the global ROeID session across all federated services, redirect the user to:

```text
https://sso.beta.roeid.ro/logout?target=https://dev.mydemosystem.ro

```

> **WARNING:** Global logout terminates active sessions across all federated public sector portals. Label this button explicitly as **Deconectare din ROeID** (Disconnect from ROeID) and reserve it primarily for shared or kiosk environments.

## Developer Tips & Tricks

When configuring local environments (e.g., `dev.mydemosystem.ro`), use [Caddy](https://caddyserver.com/docs/quick-starts/reverse-proxy) to terminate trusted local SSL certificates:

```bash
caddy reverse-proxy --from dev.mydemosystem.ro:443 --to localhost:3000

```

Map the domain in `/etc/hosts` or `C:\Windows\System32\Drivers\etc\hosts`:

```text
127.0.0.1 dev.mydemosystem.ro

```

### Java Application Server (WAS / Keycloak Filter)

Extracting the principal and custom claims via `keycloak-servlet-filter-adapter`:

```java
String email = keycloakPrincipal.getKeycloakSecurityContext().getIdToken().getPreferredUsername();
String cnp = (String) keycloakPrincipal.getKeycloakSecurityContext().getIdToken().getOtherClaims().get("cnp");

```

### Node.js / Express Example

```javascript
const express = require('express');
const { auth, requiresAuth } = require('express-openid-connect');
const router = express.Router();

const openIdConfig = {
  authRequired: false,
  routes: {
    login: false,
    callback: '/sso'
  },
  authorizationParams: {
    response_type: 'code',
    scope: 'openid profile',
    response_mode: 'query'
  },
  baseURL: process.env.OPENID_BASEURL,
  clientID: process.env.OPENID_CLIENTID,
  issuerBaseURL: process.env.OPENID_ISSUER,
  secret: process.env.OPENID_SECRET,
  clientSecret: process.env.OPENID_CLIENTSECRET
};

router.use(auth(openIdConfig));

router.use((req, res, next) => {
  res.locals.user = req.oidc.user;
  next();
});

router.get('/login', (req, res) => {
  res.oidc.login({
    returnTo: '/dashboard',
    authorizationParams: {
      redirect_uri: process.env.OPENID_REDIRECT,
      response_type: 'code',
      scope: 'openid bapi'
    }
  });
});

router.post('/sso', express.urlencoded({ extended: false }), (req, res) => {
  res.oidc.callback({
    returnTo: '/dashboard',
    redirectUri: process.env.OPENID_REDIRECT
  });
});

router.get('/protectedRoute', requiresAuth(), (req, res) => {
  res.json({
    status: 'authenticated',
    user: req.oidc.user
  });
});

```

### Ory Kratos Configuration (Self-Hosted)

Add ROeID as a generic OIDC provider in `kratos.yml`:

```yaml
selfservice:
  methods:
    oidc:
      config:
        providers:
          - id: roeid
            provider: generic
            client_id: <CLIENT_ID>
            client_secret: <CLIENT_SECRET>
            issuer_url: <ISSUER_URL>
            mapper_url: file:///etc/kratos/oidc.roeid.jsonnet
            scope:
              - openid
              - profile
      enabled: true

```

Jsonnet attribute mapping definition (`oidc.roeid.jsonnet`):

```jsonnet
local claims = std.extVar('claims');

{
  identity: {
    traits: {
      name: {
        [if "prenume" in claims.raw_claims then "first" else null]: claims.raw_claims.prenume,
        [if "nume" in claims.raw_claims then "last" else null]: claims.raw_claims.nume,
      },
      email: claims.email,
      [if "cnp" in claims.raw_claims then "cnp" else null]: claims.raw_claims.cnp,
      [if "telefon" in claims.raw_claims then "telefon" else null]: claims.raw_claims.telefon,
      verified: claims.verified,
      [if "datanasterii" in claims.raw_claims then "data_nasterii" else null]: claims.raw_claims.datanasterii,
      adresa: {
        [if "strada" in claims.raw_claims then "strada" else null]: claims.raw_claims.strada,
        [if "bloc" in claims.raw_claims then "bloc" else null]: claims.raw_claims.bloc,
        [if "nr" in claims.raw_claims then "nr" else null]: claims.raw_claims.nr,
        [if "etaj" in claims.raw_claims then "etaj" else null]: claims.raw_claims.etaj,
        [if "apartament" in claims.raw_claims then "apt" else null]: claims.raw_claims.apartament,
        [if "localitate" in claims.raw_claims then "localitate" else null]: claims.raw_claims.localitate,
        [if "judet" in claims.raw_claims then "judet" else null]: claims.raw_claims.judet
      },
      act: {
        [if "tipact" in claims.raw_claims then "tipact" else null]: claims.raw_claims.tipact,
        [if "serie" in claims.raw_claims then "serie" else null]: claims.raw_claims.serie,
        [if "autoritateaemitenta" in claims.raw_claims then "autoritatea_emitenta" else null]: claims.raw_claims.autoritateaemitenta,
        [if "valabilitatedocument" in claims.raw_claims then "valabilitate_document" else null]: claims.raw_claims.valabilitatedocument
      }
    }
  }
}

```
