---
title: "Make workflow able to use authentication request on run"
date: 2025-10-07
---
Starting RHDH 1.7.3, the orchestrator plugin let you specify in the worfklow's data input schema file the required login for the workflow to work against external services.


# Prerequisites
* Keycloak or another OIDC provider that supports OAuth 2.0 Token Exchange
* A workflow calling an OpenAPI client generated from an OpenAPI specification file using an `oauth2` security scheme

# Configure data input schemas property
To enable RHDH Orchestrator plugin to dynamically prompt for authentication you need to set the `authSetup` property and add each authentication provider needed under `authTokenDescriptors`:
```
"authSetup": {
    "type": "string",
    "ui:widget": "AuthRequester",
    "ui:props": {
        "authTokenDescriptors": [
            {
            "provider": "oidc",
            "customProviderApiId": "internal.auth.oidc",
            "tokenType": "oauth"
            }
        ]
    }
}
```
In the above example, we are making sure that upon execution the RHDH will request the user to login in the OIDC provider configured in RHDH.


In the workflow, you will then be able to use the token by using the header `X-Authorization-Oidc`. See [Token Propagation](../token-propagation) and [Token Exchange](../token-exchange) pages for example in using such header.

You can specify other providers such as github or gitlab, the header's name is formatted as follow: `X-Authorization-<provider>`.