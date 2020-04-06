---
title: Split API
language_tabs:
  - shell: Shell
  - ruby: Ruby
  - javascript--node: NodeJS
  - python: Python
  - java: Java
  - php: PHP
  - go: Go
toc_footers: []
includes: []
search: true
highlight_theme: darkula
headingLevel: 2

---

<h1 id="Split-API">Split API v1.0</h1>

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Split allows you to make, get and manage payments using nothing but bank accounts.

It is important to understand that there are 2 main ways Split can be used for maximum flexibility:

1. Between Split accounts.
2. Between a Split account and anyone.

Due to the above, certain endpoints and techniques will differ slightly depending on who you are interacting with. You can find more on this in the [Making payments](/#making-payments) and [Getting paid](/#getting-paid) guides.

<div class="middle-header">Conventions</div>

* Authentication is performed using OAuth2. See the [Get started](/#get-started) and [Authentication & Authorisation](/#authentication-and-authorisation) guides for more.
* All communication is via `https` and supports **only** `TLSv1.2`.
* Production API: `https://api.split.cash/`.
* Production UI: `https://go.split.cash/`.
* Sandbox API: `https://api.sandbox.split.cash/`.
* Sandbox UI: `https://go.sandbox.split.cash/`.
* Data is sent and received as JSON.
* Clients should include the `Accepts: application/json` header in their requests.
* Currencies are represented by 3 characters as defined in [ISO 4217](http://www.xe.com/iso4217.php).
* Dates & times are returned in UTC using [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format with second accuracy. With requests, when no TZ is supplied, the configured TZ of the authenticated user is used.
* Amounts are always in cents with no decimals unless otherwise stated.
* Split provides static public IP addresses for all outbound traffic, including webhooks.
    * Sandbox IP: `13.237.142.60`
    * Production IPs: `52.64.11.67` and `13.238.78.114`

<div class="middle-header">System Status</div>

Check the platform status, or subscribe to receive notifications at [status.split.cash](https://status.split.cash/). If you would like to check platform status programmatically, please refer to [status.split.cash/api](https://status.split.cash/api).

# Guides

## Try it out
The best way to familiarise yourself with our API is by interacting with it.

We've preloaded a collection of all our endpoints for you to use in Postman. Before you start, load up our API collection:

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/54edf5935ad8a2bcd15b#?env%5BSplit%20Payments%20Public%20Sandbox%5D=W3sia2V5Ijoic2l0ZV9ob3N0IiwidmFsdWUiOiJodHRwczovL2dvLnNhbmRib3guc3BsaXQuY2FzaCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYXBpX2hvc3QiLCJ2YWx1ZSI6Imh0dHBzOi8vYXBpLnNhbmRib3guc3BsaXQuY2FzaCIsImVuYWJsZWQiOnRydWV9LHsia2V5Ijoib2F1dGgyX2FwcGxpY2F0aW9uX2lkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im9hdXRoMl9zZWNyZXQiLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWV9LHsia2V5Ijoic2NvcGUiLCJ2YWx1ZSI6InB1YmxpYyBhZ3JlZW1lbnRzIGJhbmtfYWNjb3VudHMgYmFua19jb25uZWN0aW9ucyBjb250YWN0cyBwYXltZW50cyBwYXltZW50X3JlcXVlc3RzIHJlZnVuZF9yZXF1ZXN0cyB0cmFuc2FjdGlvbnMgcmVmdW5kcyBvcGVuX2FncmVlbWVudHMgb2ZmbGluZV9hY2Nlc3MiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Imlzbzg2MDFfbm93IiwidmFsdWUiOiIyMDE4LTExLTI4VDIwOjU1OjI0WiIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYWNjZXNzX3Rva2VuIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InJlZnJlc2hfdG9rZW4iLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWV9XQ==)

Okay, lets get things setup!

1. **Create a Split account**

    If you haven't already, you'll want to create a sandbox Split account at [https://go.sandbox.split.cash](https://go.sandbox.split.cash)

2. **Register your application with Split**

    Sign in and create an OAuth2 application: [https://go.sandbox.split.cash/oauth/applications](https://go.sandbox.split.cash/oauth/applications).

    [![Split OAuth2 app create](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_oauth2_app_create.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_oauth2_app_create.png)

3. **Generate personal access tokens**

    The quickest way to access your Split account via the API is using
    personal access tokens. Click on your newly created application from your [application
list](https://go.sandbox.split.cash/oauth/applications) and click on **+ Personal Access Token**.

    [![Split locate personal OAuth2 tokens](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_tokens_empty.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_tokens_empty.png)

    _(You'll have the option to give the token a title)_

    [![Split personal OAuth2 tokens](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_token.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_token.png)

    <aside class="notice">Please note that personal access tokens do not expire.</aside>

4. **Use personal access token in Postman**

    You can use this `access_token` to authorise any requests to the
    Split API in Postman by choosing the **Bearer Token** option under
    the **Authorization** tab.

    [![Postman use personal OAuth2 tokens](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_use_personal_access_token.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_use_personal_access_token.png)

5. **Make an API request!**

    You are now ready to interact with your Split account via the
    API! Go ahead and send a request using Postman.

    [![Postman use personal OAuth2 tokens](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_request_response.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_request_response.png)

## Get started
This guide will help you setup an OAuth2 app in order to get authenticated & authorised to communicate with the Split API.

**Before you start:**

* We use the term **user** below but the user can be a third party or the same user that owns the OAuth2 application.
* As noted below, some access tokens expire every 2 hours. To get a new access token use the [refresh grant strategy](/#authentication-and-authorisation) to swap a refresh token for a new access token.

1. **Create a Split account**

    If you haven't already, you'll want to create a sandbox Split account at [https://go.sandbox.split.cash](https://go.sandbox.split.cash).

2. **Choose authentication method**

    All requests to the Split API require an `access_token` for authentication. There are two options for obtaining these tokens, the correct option will depend on your use case:

    **Personal access token** If you only need to access your own Split account via the API, then using personal access tokens are the most straight-forward way. Refer to [Personal access token](/#personal-access-token) to setup. These tokens do not expire so no refreshing is required.

    **OAuth grant flow** When you require your application to act on behalf of other Split accounts you'll need to implement the OAuth grant flow process. Refer to [OAuth grant flow guide](/#oauth-grant-flow) to setup. There is also an [OAuth grant flow tutorial](/#oauth-grant-flow-tutorial). These access tokens expire every 2 hours, unless the `offline_access` scope is used in which case the tokens will not expire.

## Personal access token
If you're looking to only access your own account via the API, you can generate a personal access token from the UI. These tokens do not expire, but can be deleted.

* To do this, sign in to your Split account and [create an application](https://go.sandbox.split.cash/oauth/applications) if you haven't already. Click on your application from your [application list](https://go.sandbox.split.cash/oauth/applications) and click on **Personal access**.

    [![Split locate personal OAuth2 tokens](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_tokens_empty.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_tokens_empty.png)

    _(You'll have the option to give the token a title)_

    [![Split personal OAuth2 tokens](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_token.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_personal_access_token.png)

* Now that you have an `access_token` you can interact with your Split account via the API.

    To do so, you must simply append the access token to the header of any API request: `Authorization: Bearer {access_token}`

## OAuth grant flow
1. **Register your application with Split**

    Once you've got your account up and running, sign in and create an OAuth2 profile for your application: [https://go.sandbox.split.cash/oauth/applications](https://go.sandbox.split.cash/oauth/applications)

    | Parameter | Description |
    |-----------|-------------|
    | **Name**  | The name of your application. When using the the *Authorisation Grant Flow*, users will see this name as the application requesting access to their account. |
    | **Redirect URI** | Set this to your application's endpoint charged with receiving the authorisation code. |

2. **Obtain an authorisation code**

    Construct the initial URL the user will need to visit in order to grant your application permission to act on his/her behalf. The constructed URL describes the level of permission ([`scope`](/#scopes)), the application requesting permission (`client_id`) and where the user gets redirected once they've granted permission (`redirect_uri`).

    The URL should be formatted to look like this:
    `https://go.sandbox.split.cash/oauth/authorize?response_type=code&client_id={client_id}&redirect_uri={redirect_uri}&scope={scope}`

    | Parameter | Description |
    |-----------|-------------|
    | `response_type` | Always set to `code` |
    | `client_id` | This is your `Application ID` as generated when you registered your application with Split |
    | `redirect_uri` | URL where the user will get redirected along with the newly generated authorisation code |
    | `scope` | The [scope](/#scopes) of permission you're requesting |

3. **Exchange the authorisation code for an access token**

    When the user visits the above-mentioned URL, they will be presented with a Split login screen and then an authorisation screen:

    [![Authorise OAuth2 app](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/oauth2_app_authorise.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/oauth2_app_authorise.png)

    After the user has authorised your application, they will be returned to your application at the URL specified in `redirect_uri` along with the `code` query parameter as the authorisation code.

    Finally, the authorisation code can then be exchanged for an access token and refresh token pair by POSTing to: `https://go.sandbox.split.cash/oauth/token`

    **Note** The authorisation code is a ONE-TIME use code. It will not work again if you try to POST it a second time.

    | Parameter | Description |
    |-----------|-------------|
    | `grant_type` | Set to `authorization_code` |
    | `client_id` | This is your `Application ID` as generated when you registered your application with Split |
    | `client_secret` | This is your `Secret` as generated when you registered your application with Split |
    | `code` | The authorisation code returned with the user (ONE-TIME use) |
    | `redirect_uri` | Same URL used in step 3 |

4. **Wrap-up**

    Now that you have an access token and refresh token, you can interact with the Split API as the user related to the access token.
    To do so, you must simply append the access token to the header of any API request: `Authorization: Bearer {access_token}`

## OAuth grant flow tutorial
The OAuth grant flow process is demonstrated using Postman in the steps below.

Before you start, load up our API collection:

[![Run in Postman](https://run.pstmn.io/button.svg)](https://app.getpostman.com/run-collection/54edf5935ad8a2bcd15b#?env%5BSplit%20Payments%20Public%20Sandbox%5D=W3sia2V5Ijoic2l0ZV9ob3N0IiwidmFsdWUiOiJodHRwczovL2dvLnNhbmRib3guc3BsaXQuY2FzaCIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYXBpX2hvc3QiLCJ2YWx1ZSI6Imh0dHBzOi8vYXBpLnNhbmRib3guc3BsaXQuY2FzaCIsImVuYWJsZWQiOnRydWV9LHsia2V5Ijoib2F1dGgyX2FwcGxpY2F0aW9uX2lkIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Im9hdXRoMl9zZWNyZXQiLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWV9LHsia2V5Ijoic2NvcGUiLCJ2YWx1ZSI6InB1YmxpYyBhZ3JlZW1lbnRzIGJhbmtfYWNjb3VudHMgYmFua19jb25uZWN0aW9ucyBjb250YWN0cyBwYXltZW50cyBwYXltZW50X3JlcXVlc3RzIHJlZnVuZF9yZXF1ZXN0cyB0cmFuc2FjdGlvbnMgcmVmdW5kcyBvcGVuX2FncmVlbWVudHMgb2ZmbGluZV9hY2Nlc3MiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6Imlzbzg2MDFfbm93IiwidmFsdWUiOiIyMDE4LTExLTI4VDIwOjU1OjI0WiIsImVuYWJsZWQiOnRydWV9LHsia2V5IjoiYWNjZXNzX3Rva2VuIiwidmFsdWUiOiIiLCJlbmFibGVkIjp0cnVlfSx7ImtleSI6InJlZnJlc2hfdG9rZW4iLCJ2YWx1ZSI6IiIsImVuYWJsZWQiOnRydWV9XQ==)

**A screencast of this process is also available: [https://vimeo.com/246203244](https://vimeo.com/246203244).**

1. **Create a Split account**

    If you haven't already, you'll want to create a sandbox Split account at [https://go.sandbox.split.cash](https://go.sandbox.split.cash)

2. **Register your application with Split**

    Sign in and create an OAuth2 application: [https://go.sandbox.split.cash/oauth/applications](https://go.sandbox.split.cash/oauth/applications).

    Use the special Postman callback URL: `https://www.getpostman.com/oauth2/callback`

    [![Split OAuth2 app setup](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_oauth2_app_setup.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_oauth2_app_setup.png)

3. **In Postman, setup your environment variables**

    We've included the **Split Payments Public Sandbox** environment to get you started. Select it in the top right corner of the window then click the <img class="inline-1" alt="Postman Quick-Look icon" src="https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_quick_look_icon.png" /> icon and click **edit**.

    [![Edit Postman environment](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_edit_environment.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_edit_environment.png)

    Using the details from the OAuth2 app you created earlier, fill in the **oauth2_application_id** & **oauth2_secret** fields.

    [![Fill in environment values](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_environment_values.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_environment_values.png)

4. **Setup the authorization**

    Click on the **Authorization** tab and select **OAuth 2.0**

    [![Postman Authorization tab](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_authorization_tab.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_authorization_tab.png)

    Click the **Get New Access Token** button

    [![Postman get new access token](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_get_new_access_token.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_get_new_access_token.png)

    Fill in the OAuth2 form as below:

    [![Postman OAuth2](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_oauth2_form.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_oauth2_form.png)

5. **Get authorised**

    Click **Request Token** and wait a few seconds and a browser window will popup

    Sign in with your Split account (or any other Split account you want to authorise).

    [![Signin Split to authorise via OAuth2](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_oauth2_signin.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/split_oauth2_signin.png)

    Click **Authorise** to allow the app to access the signed in account. Once complete, Postman will automatically exchange the authorisation code it received from Split for the `access_token/refresh_token` pair. It will then store the `access_token/refresh_token` for you to use in subsequent API requests. The `access_token` effectively allows you to send requests via the API as the user who provided you authorisation.

    [![Authorise OAuth2 app](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/oauth2_app_authorise.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/oauth2_app_authorise.png)

6. **You're now ready to use the API**

    Select an endpoint from the Split collection from the left hand side menu. Before you send an API request ensure you select your access token and Postman will automatically add it to the request header.

    [![Postman use token](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_use_token.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/postman_use_token.png)

<aside class="notice">Remember to select the access token everytime you try a new endpoint. Have fun!</aside>

## Authentication and Authorisation

Split uses OAuth2 over https to manage authentication and authorisation.

OAuth2 is a protocol that lets external applications request permission from another Split user to send requests on their behalf without getting their password.
This is preferred over Basic Authentication because access tokens can be limited by scope and can be revoked by the user at any time.

New to OAuth2? DigitalOcean has a fantastic 5 minute [introduction to OAuth2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2#grant-type-authorization-code).

We currently support the **authorisation code** and **refresh token** grants.

### Authorisation Code Grant
This type of grant allows your application to act on behalf of a user. If you've ever used a website or application with your
Google, Twitter or Facebook account, this is the grant being used.

See the [Get Started guide](/#get-started) for step by step details on how to use this grant.

### Refresh Token Grant

> Code sample

```
curl -F "grant_type=refresh_token" \
     -F "client_id={{oauth2_application_id}}" \
     -F "client_secret={{oauth2_application_secret }}" \
     -F "refresh_token={{refresh_token}}" \
     -X POST https://go.sandbox.split.cash/oauth/token
```

> Example response

```json
{
    "access_token": "ad0b5847cb7d254f1e2ff1910275fe9dcb95345c9d54502d156fe35a37b93e80",
    "token_type": "bearer",
    "expires_in": 7200,
    "refresh_token": "cc38f78a5b8abe8ee81cdf25b1ca74c3fa10c3da2309de5ac37fde00cbcf2815",
    "scope": "public"
}
```

When using the authorisation code grant above, Split will return a `refresh token` along with the access token. Access tokens are short lived and last 2 hours but refresh tokens do not expire.

When the access token expires, instead of sending the user back through the authorisation flow you can use the refresh token to retrieve a new access token with the same permissions as the old one.

<aside class="notice">
  The <code>refresh_token</code> gets regenerated and sent alongside the new <code>access_token</code>. In other words, <code>refresh_token</code>s are single use so you'll
want to store the newly generated <code>refresh_token</code> everytime you use it to get a new <code>access_token</code>
</aside>
## Making payments
In order to payout funds, you'll be looking to use the [Payments](/#Split-API-Payments) endpoint. Whether you're paying out another Split account holder or anyone, the process is the same:

1. Add the recipient to your Contacts: [Split Contact](/#add-a-split-contact) or [Anyone Contact](/#add-an-anyone-contact)
2. [Make the Payment](/#make-a-payment) to your Contact.

Common use cases:

* Automated payout disbursement (Referal programs, net/commission payouts, etc...)

## Getting paid
There are 2 ways to get paid:

### POSTing a [Payment Request](/#Split-API-Payment-Requests)

Provides the ability to send a Payment Request (get paid) to any Contact that is either:

* A Split Contact (The contact has a Split account); **or**
* An Anyone Contact (The contact does not have a Split account) with an accepted Agreement in place.

**For a Split Contact**:

* They will receive a request that they must approve via the Split UI or API in order for the funds to flow from their bank account to yours.

* To automate the Payment Request approval, process you must first [enter into an Agreement](/#Split-API-Agreements) with the Split Contact. Once the Agreement is accepted, any future Payment Request will be automatically approved and processed per the Agreement terms.

**For an Anyone Contact**:

* To send a Payment Request using the API to an Anyone Contact, you must first have an accepted Agreement with them . To do so, you can send them an [Open Agreement link](https://help.split.cash/agreements/open-agreement) or [Unassigned Agreement link](http://help.split.cash/agreements/unassigned-agreement) for them to [elect & verify their bank account](https://help.split.cash/bank-accounts/instant-account-verification-iav) and accept the Agreement.

Common use cases:

* Subscriptions
* On-account balance payments
* Bill smoothing
* Repayment plans

Example flow embedding an [Open Agreement link](https://help.split.cash/agreements/open-agreement) using an iFrame in order to automate future Payment Request approvals:

[![Hosted Open Agreement](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/host_oa.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/host_oa.png)

### Sharing an [Open Payment Request](http://help.split.cash/payment-requests/open-payment-requests)

Provides the ability to get paid once-off by a anyone whether they are a Split account holder or not.

Usage notes:

* Utilise a [customisable hosted Payment Request form](http://help.split.cash/payment-requests/open-payment-requests) that takes care of everything from confirming the payer's bank account access to the transfer of funds.
* The Open Payment Request link can be shared and sit as a secure form either outside your app or embeded within via iframe with the ability to whitelabel.
* The URL for the form contains all the customisation parameters enabling you to generate the form on the fly.

Common use cases:

* Online or offline purchases (eCommerce, fundraiser, etc...)
* Invoice payment
* Group funding (paying for a restaurant bill)

Example flow embedding the [Open Payment Request](https://help.split.cash/payment-requests/open-payment-requests) link using an iFrame:

[![Hosted Open Payment Request](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/hosted_opr.png)](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/hosted_opr.png)

## Idempotent requests

> Example response

```json
{
  "errors": [
    {
      "title": "Duplicate idempotency key",
      "detail": "A resource has already been created with this idempotency key",
      "links": {
        "about": "https://docs.split.cash/"
      },
      "meta": {
        "resource_ref": "PB.1a4"
      }
    }
  ]
}
```

The Split API supports idempotency for safely retrying requests without accidentally performing the same operation twice.
For example, if a [Payment](#Split-API-Payments) is `POST`ed and a there is a network connection error, you can retry the Payment with the same idempotency key to guarantee that only a single Payment is created.

To perform an idempotent request, provide an additional `Idempotency-Key: <key>` header to the request.
You can pass any value as the key but we suggest that you use [V4 UUIDs](https://www.uuidtools.com/generate/v4) or another appropriately random string.

Keys expire after 24 hours. If there is a subsequent request with the same idempotency key within the 24 hour period, we will return a `409 Conflict`.

* The `meta.resource_ref` value is the reference of the resource that was previously created with the conflicting idempotency key.
* The `Idempotency-Key` header is optional but recommended.
* Only the `POST` action for the Payments, Payment Requests and Refunds endpoints support the use of the `Idempotency-Key`.
* Endpoints that use the `GET` or `DELETE` actions are idempotent by nature.

## Error responses

> Example detailed error response

```json
{
  "errors": [
    {
      "title": "A Specific Error",
      "detail": "Details about the error",
      "links": {
        "about": "https://docs.split.cash/..."
      }
    }
  ]
}
```
> Example resource error response

```json
{
  "errors": "A sentence explaining error/s encounted"
}
```
The Split API returns two different types of error responses depending on the context.

**Detailed error responses** are returned for:

* Authentication
* Request types
* Idempotency

All other errors relating to Split specific resources(e.g. Contacts) will return the **Resource error response** style.

## Speeding up onboarding
Consider the following scenario:

<blockquote class="main-quote">Split is integrated in your application to handle payments.<br>A customer would like to use Split but does not yet have Split account.<br>You already have some information about this customer.</blockquote>

Given the above, in a standard implementation where a customer enables/uses Split within your application, these are the steps they would follow:

1. Click on some sort of button within your app to use Split.
2. They get redirected to the Split sign in page (possibly via a popup or modal).
3. Since they don't yet have a Split account, they would click on sign up.
4. They would fill in all their signup details and submit.
5. They would be presented with the [authorisation page](https://raw.githubusercontent.com/splitpayments/public_assets/master/images/oauth2_app_authorise.png).
6. They would click the "Authorise" button and be redirected to your app.

Whilst not too bad, we can do better!

In order to speed up the process, we allow query string params to be appended to the [authorisation URL](/#get-started). For instance, if we already have some information about the customer and know they probably don't have a Split account, we can embed this information in the authorisation URL.

**Supported query string parameters**

| Parameter | Description |
|-----------|--------|
| `landing`   | Accepted value: `sign_up`. What page the user should see first if not already signed in. Default is the sign in page. <br><br>Deprecated values: `business_sign_up`, `personal_sign_up`.|
| `nickname` | Only letters, numbers, dashes and underscores are permitted. This will be used to identify the account in Split. |
| `name` | Business account only. Business name. |
| `abn` | Business account only. Business ABN. |
| `phone` | Business account only. Business phone number. |
| `street_address` | |
| `suburb` | |
| `state` | See the sign up page for accepted values |
| `postcode` | |
| `first_name` | |
| `last_name` | |
| `mobile_phone` | |
| `email` | |

All values should be [URL encoded](https://en.wikipedia.org/wiki/Query_string#URL_encoding).

As an example, the following authorisation URL would display the **personal sign up** & prefill the first name field with **George**:
`https://go.sandbox.split.cash/oauth/authorize?response_type=code&client_id=xxx&redirect_uri=xxx&scope=xxx&landing=sign_up&first_name=George`

You can also pass the values directly to the sign up page outside of the OAuth2 authorisation process. Click on the following link to see the values preloaded: [https://go.sandbox.split.cash/business/sign_up?name=GeorgeCo&nickname=georgeco&first_name=George](https://go.sandbox.split.cash/business/sign_up?name=GeorgeCo&nickname=georgceco&first_name=George).

# Sandbox Testing Details
Try out your happy paths and not-so happy paths, the sandbox is a great place to get started without transferring actual funds. All transactions are simulated and no communication with financial institutions is performed.

The sandbox works on a 1 minute cycle to better illustrate how transactions are received and the lifecyle they go through. In other words, every minute, we simulate communicating with financial institutions and update statuses and events accordingly.

All 6 digits BSBs are valid in the sandbox with the exception of `100000` which is a place keeper for an invalid BSB. In production, only real BSBs are accepted.
## Transaction failures
To simulate [transaction failures](#failure-reasons) create a Payment or Payment Request with a specific amount listed in the table.

| Transaction failure reason | Debit | Credit |
|----------------------------|-------|--------|
| `invalid_bsb_number`       |  $0.01  |  $0.51   |
| `account_closed`           |  $0.02  |  $0.52   |
| `customer_deceased`        |  $0.03  |  $0.53   |
| `incorrect_account_number` |  $0.04  |  $0.54   |
| `refer_to_split`           |  $0.05  |  $0.55   |
| `user_voided`              |  $0.06  |  $0.56   |
| `admin_voided`             |  $0.07  |  $0.57   |
| `refer_to_customer`        |  $0.10  |          |
| `insufficient_funds`       |  $0.11  |          |
| `payment_stopped`          |  $0.12  |          |

### Example scenarios

  1. Pay a contact with an invalid account number:
    * Initiate a Payment for <code>$0.54</code>.
    * Split will mimic a successful debit from your bank account.
    * Split will mimic a failure to credit the contact's bank account.
    * Split will automatically create a <code>payout_reversal</code> credit transaction back to your bank account.
  2. Pay a contact whilst having insufficient funds:
    * Initiate a Payment for <code>$0.11</code>.
    * Split will mimic a failure to debit your bank account.
    * Split will mark the debit as `returned` due to `insufficient_funds`.
    * Split will void the scheduled credit to the contact's bank account.
  3. Request payment from a contact with a closed bank account:
    * Initiate a Payment Request for <code>$0.02</code>.
    * Split will mimic a failure to debit the contact's bank account.

## Special accounts
There are some accounts with a special behaviour. You can add them to your Contacts by nickname or bank details.

| Account nickname | Branch code (BSB) | Account number | Description |
|---|---|---|---|
| accept_agreements | `900000` | `99999` | Always accepts agreements |
| decline_agreements | `900001` | `99999` | Always declines agreements |

### (Deprecated) Payment credit failure bank accounts
You can send Payments to the following reserved bank accounts to trigger specific failures on the credit side.

| Failure type             | Branch code (BSB) | Account number |
|--------------------------|-------------------|----------------|
| `account_closed`           | `300000`          | `99999`        |
| `customer_deceased`        | `400000`          | `99999`        |
| `incorrect_account_number` | `500000`          | `99999`        |
| `refer_to_split`           | `700000`          | `99999`        |

### (Deprecated) Payment Request failure bank accounts
You can send Payment Requests to the following reserved bank accounts to trigger specific failures.

| Failure type             | Branch code (BSB) | Account number |
|--------------------------|-------------------|----------------|
| `payment_stopped`          | `200000`          | `99999`        |
| `account_closed`           | `300000`          | `99999`        |
| `customer_deceased`        | `400000`          | `99999`        |
| `incorrect_account_number` | `500000`          | `99999`        |
| `refer_to_customer`        | `600000`          | `99999`        |
| `refer_to_split`           | `700000`          | `99999`        |
| `insufficient_funds`       | `800000`          | `99999`        |

By default, all Agreements sent to Contacts with a failure bank account will be automatically accepted.

## Instant account verification accounts
When using any of our hosted solutions ([Payment Requests](https://help.split.cash/payment-requests/open-payment-requests), [Open Agreements](https://help.split.cash/agreements/open-agreement) or [Unassigned Agreements](http://help.split.cash/agreements/unassigned-agreement)) you may want to test the [Instant Account Verification (IAV)](http://help.split.cash/bank-accounts/instant-account-verification-iav) process where we accept online banking credentials to validate bank account access. To do so, you can use the following credentials:

| Login | Password |
|-------|----------|
| `12345678` | `TestMyMoney` |

<aside class="notice">The credentials will work with any of the available financial institutions.</aside>
# Configuration

## Scopes
Scopes define the level of access granted via the OAuth2 authorisation process. As a best practice, only use the scopes your application will require.

| Scope | Description |
|--------|------------|
| `public` | View user's public information |
| `agreements` | Manage user's Agreements |
| `bank_accounts` | Manage user's Bank Accounts |
| `bank_connections` | Manage user's Bank Connections |
| `contacts` | Manage user's Contacts |
| `open_agreements` | Manage user's Open Agreements |
| `payments` | Manage user's Payments |
| `payment_requests` | Manage user's Payment Requests |
| `refunds` | Manage user's Refunds |
| `transactions` | Access user's Transactions |
| `offline_access` | Create non-expiring access tokens for user |

  <aside class="notice">Please use `offline_access` with discretion, as you'll have no direct way to invalidate the token. Please contact Split Payments immediately if any token may have potentially been compromised.</aside>

## Pagination

> Example response header

```
Link: <http://api.sandbox.split.cash/contacts?page=5>; rel="last", <http://api.sandbox.split.cash/contacts?page=2>; rel="next"
Per-Page: 25
Total: 5
```

All collections are paginated to 25 items by default and the pagination information may be found in the response header. You can customise the pagination by appending `?per_page=x` and/or `?page=x` to the endpoint URL.

## Remitter

> Example request

```json
{
  "...": "...",
  "metadata": {
   "remitter": "CustomRem"
   }
}
```

You can elect to assign a remitter name on a per-request basis when submitting Payments & Payment Requests. Simply append the `remitter` key and a value within the `metadata` key.

* **For Payments**, the party being credited will see the designated remitter name along the entry on their bank statement.
* **For Payment Requests**, the party being debited will see the designated remitter name along the entry on their bank statement.

<aside class="notice">The remitter name MUST be between 3 and 16 characters.</aside>

## Aggregation

Split will automatically aggregate debits that are:

- From the same bank account; and
- Initiated by the same Split account.

Likewise for credits:

- To the same bank account; and
- Initiated by the same Split account.

Should you prefer aggregation to be disabled, please contact [support@splitpayments.com.au](mailto:support@splitpayments.com.au). Note that additional charges may apply.

## Webhooks

> Example response

```json
{
  "event": {
    "type": "object.action",
    "at": "yyyy-mm-ddThh:mm:ssZ",
    "who": {
      "account_id": "x",
      "bank_account_id": "x"
    }
  },
  "data": [
    {
      // The data section will follow the same type of structure
      // as the event type it is representing. For example, if the
      // event type was "debit.scheduled", the data representation
      // structure would be that of a debit object. Too see what a
      // debit object structure looks like, see the Transactions
      // section.
    }
  ]
}
```
Please refer to our help centre [article on webhooks](http://help.split.cash/en/articles/3303626-webhooks) for more information and an overview of what you can achieve with webhooks.

We support two main categories of webhooks:

1. **Owner**: These webhooks are managed by the owner of the Split account and only report on events owned by the Split account.
2. **App**: These webhooks are managed by the Split OAuth2 application owner and will report on events relating to any authorised Split account (limited by scope).

All events posted to the designated URL fit the same structure.
<aside class="notice">The sandbox environment allow both HTTP and HTTPS webhook URLs. The live environment however will only POST to HTTPS URLs.
</aside>

### Our Delivery Promises
1. We only consider a webhook event delivery as failed if we don't receive any http response code (2xx, 4xx, 5xx, etc.)
2. We will auto-retry failed deliveries every 5 minutes for 1 hour.
3. Delivery order for webhook events is not guaranteed.
4. We guarantee at least 1 delivery attempt.

### Request ID

> Example header

```
Split-Request-ID: 07f4e8c1-846b-5ec0-8a25-24c3bc5582b5
```

Split provides a `Split-Request-ID` header in the form of a `UUID` which uniquely identifies a webhook event. If the webhook event is retried/retransmitted by Split, the UUID will remain the same. This allows you to check if a webhook event has been previously handled/processed.

### Checking Webhook Signatures

> Example header

```
Split-Signature: 1514772000.93eee90206280b25e82b38001e23961cba4c007f4d925ba71ecc2d9804978635
```

Split signs the webhook events it sends to your endpoints. We do so by including a signature in each event’s `Split-Signature` header. This allows you to validate that the events were indeed sent by Split.

Before you can verify signatures, you need to retrieve your endpoint’s secret from your Webhooks settings. Each endpoint has its own unique secret; if you use multiple endpoints, you must obtain a secret for each one.

The `Split-Signature` header contains a timestamp and one or more signatures. All separated by `.` (dot).

> Example code

```sh
# Shell example is not available
```

```go
package main
import (
    "crypto/hmac"
    "crypto/sha256"
    "strings"
    "fmt"
    "encoding/hex"
)
func main() {
    secret := "1234"
    message := "full payload of the request"
    splitSignature := "1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f"

    data := strings.Split(splitSignature, ".")
    timestamp, givenSignature := data[0], data[1]

    signedPayload := timestamp + "." + message

    hash := hmac.New(sha256.New, []byte(secret))
    hash.Write([]byte(signedPayload))
    expectedSignature := hex.EncodeToString(hash.Sum(nil))

    fmt.Println(expectedSignature)
    // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
    fmt.Println(givenSignature)
    // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
}
```

```python
import hashlib
import hmac

split_signature = '1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f'
secret = bytes('1234').encode('utf-8')
message = bytes('full payload of the request').encode('utf-8')

data = split_signature.split('.')
timestamp = data[0]
given_signature = data[1]

signed_payload = timestamp + '.' + message

expected_signature = hmac.new(secret, signed_payload, digestmod=hashlib.sha256).hexdigest()

print(expected_signature)
# > f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f

print(given_signature)
# > f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
```

```ruby
require 'openssl'

split_signature = '1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f'
secret = '1234'
message = 'full payload of the request'

timestamp, given_signature, *other = split_signature.split('.')
signed_payload = timestamp + '.' + message
expected_signature = OpenSSL::HMAC.hexdigest('sha256', secret, signed_payload)

puts(expected_signature)
# => f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
puts(given_signature)
# => f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
```

```javascript--node
var crypto = require('crypto')
var message = 'full payload of the request'
var secret = '1234'
var splitSignature = '1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f'

var data = splitSignature.split('.')
var timestamp = data[0]
var givenSignature = data[1]

var signedPayload = timestamp + '.' + message

var expectedSignature = crypto.createHmac('sha256', secret).update(signedPayload).digest('hex')

console.log(expectedSignature)
// f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
console.log(givenSignature)
// f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f

```

```php
$split_signature = '1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f';
$secret = '1234';
$message = 'full payload of the request';

list($timestamp, $given_signature, $other) = explode('.', $split_signature);
$signed_payload = $timestamp . "." . $message;
$expected_signature = hash_hmac('sha256', $signed_payload, $secret, false);

echo $expected_signature; // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
echo "\n";
echo $given_signature; // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f

```

```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;

class Main {
  public static void main(String[] args) {
    try {
      String splitSignature = "1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f";
      String secret = "1234";
      String message = "full payload of the request";

      String[] data = splitSignature.split("\\.");
      String timestamp = data[0];
      String givenSignature = data[1];

      String signedPayload = timestamp + "." + message;

      Mac sha256_HMAC = Mac.getInstance("HmacSHA256");
      SecretKeySpec secret_key = new SecretKeySpec(secret.getBytes(), "HmacSHA256");
      sha256_HMAC.init(secret_key);

      String expectedSignature = javax.xml.bind.DatatypeConverter.printHexBinary(sha256_HMAC.doFinal(signedPayload.getBytes())).toLowerCase();

      System.out.println(expectedSignature);
      // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f

      System.out.println(givenSignature);
      // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
    }
    catch (Exception e){
      System.out.println("Error");
    }
  }
}

```
<!-- This example is commented out since the docs do not include C#
```csharp
using System; using System.Security.Cryptography;
class MainClass {
  public static void Main (string[] args) {
    var splitSignature = "1514772000.f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f";
    var secret = "1234";
    var message = "full payload of the request";

    var header = splitSignature.Split('.');
    var timeStamp = header[0];
    var givenSignature = header[1];

    var signedPayload = timeStamp + "." + message;

    var encoding = new System.Text.ASCIIEncoding();
    var hmacsha256 = new HMACSHA256(encoding.GetBytes(secret));

    byte[] hashmessage = hmacsha256.ComputeHash(encoding.GetBytes(signedPayload));
    var expectedSignature = BitConverter.ToString(hashmessage).Replace("-", "").ToLower();

    Console.WriteLine(expectedSignature);
    // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f

    Console.WriteLine(givenSignature);
    // f04cb05adb985b29d84616fbf3868e8e58403ff819cdc47ad8fc47e6acbce29f
  }
}

```
-->

**Step 1. Extract the timestamp and signatures from the header**

Split the header, using the `.` (dot) character as the separator, to get a list of elements.

| Element | Description |
|---------|-------------|
| `timestamp` | [Unix time](https://en.wikipedia.org/wiki/Unix_time) in seconds when the signature was created |
| `signature` | Request signature |
| `other`     | Placeholder for future parameters (currently not used) |

**Step 2: Prepare the signed_payload string**

You achieve this by concatenating:

- The timestamp from the header (as a string)
- The character `.` (dot)
- The actual JSON payload (request body)

**Step 3: Determine the expected signature**

Compute an HMAC with the SHA256 hash function. Use the endpoint’s signing secret as the key, and use the `signed_payload` string as the message.

**Step 4: Compare signatures**

Compare the signature in the header to the expected signature. If a signature matches, compute the difference between the current timestamp and the received timestamp, then decide if the difference is within your tolerance.

To protect against timing attacks, use a constant-time string comparison to compare the expected signature to each of the received signatures.

<h1 id="Split-API-Agreements">Agreements</h1>

An Agreement is an arrangement between two parties that allows them to agree on terms for which future Payment Requests will be auto-approved.

Split Agreements are managed on a per Contact basis and are unidirectional. In other words, if both parties wish for auto-approved Payment Requests, they must each propose an Agreement to the other.

If a Payment Request is sent for an amount that exceeds the terms of the agreement, it will need to be manually approved by the recipient.
Please refer to the [What is an Agreement](http://help.split.cash/articles/3094575-what-is-an-agreement) article in our knowledge base for an overview.
<div class="middle-header">Direction</div>

Agreements are therefore broken up by direction:

1. **Incoming:** Agreement received from another Split account
2. **Outgoing:** Agreement sent to another Split account

##Lifecycle

An Agreement can have the following statuses:

| Status | Description |
|-------|-------------|
| `proposed` | Waiting for the Agreement to be accepted or declined. |
| `accepted` | The Agreement have been accepted and is active. |
| `cancelled` | The Agreement has been cancelled (The initiator or authoriser can cancel an Agreement). |
| `declined` | The Agreement has been declined. |
| `expended` | The Agreement has been expended (Only for [single_use Unassigned Agreements](/#Split-API-Unassigned-Agreements)). |

## Propose an Agreement

<a id="opIdProposeAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/agreements \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"authoriser_contact_id":"8df89c16-330f-462b-8891-808d7bdceb7f","terms":{"per_payout":{"min_amount":null,"max_amount":10000},"per_frequency":{"days":7,"max_amount":1000000}}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"authoriser_contact_id\":\"8df89c16-330f-462b-8891-808d7bdceb7f\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  authoriser_contact_id: '8df89c16-330f-462b-8891-808d7bdceb7f',
  terms: {
    per_payout: { min_amount: null, max_amount: 10000 },
    per_frequency: { days: 7, max_amount: 1000000 }
  }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"authoriser_contact_id\":\"8df89c16-330f-462b-8891-808d7bdceb7f\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/agreements", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/agreements")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"authoriser_contact_id\":\"8df89c16-330f-462b-8891-808d7bdceb7f\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"authoriser_contact_id":"8df89c16-330f-462b-8891-808d7bdceb7f","terms":{"per_payout":{"min_amount":null,"max_amount":10000},"per_frequency":{"days":7,"max_amount":1000000}}}');

$request->setRequestUrl('https://api.sandbox.split.cash/agreements');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements"

	payload := strings.NewReader("{\"authoriser_contact_id\":\"8df89c16-330f-462b-8891-808d7bdceb7f\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /agreements`

Propose an Agreement to another Split Contact

<aside class="notice">You can set any of the term metrics to <code>null</code> if you wish them to not have a limit.</aside>

<aside class="notice">[31/07/2018] Please note that we've deprecated <code>authoriser_id</code> in favour of <code>authoriser_contact_id</code>. We will continue to support the use of <code>authoriser_id (Contact.data.account.id)</code> but encourage you to use <code>authoriser_contact_id (Contact.data.id)</code> to identify the authoriser of the Agreement.</aside>

> Body parameter

```json
{
  "authoriser_contact_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
  "terms": {
    "per_payout": {
      "min_amount": null,
      "max_amount": 10000
    },
    "per_frequency": {
      "days": 7,
      "max_amount": 1000000
    }
  }
}
```

<h3 id="Propose-an-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ProposeAgreementRequest](#schemaproposeagreementrequest)|true|No description|
|» authoriser_contact_id|body|string|true|The Authoriser's contact ID (`Contact.data.id`)|
|» terms|body|[Terms](#schematerms)|true|Terms|
|»» per_payout|body|[PerPayout](#schemaperpayout)|true|No description|
|»»» min_amount|body|number|false|Minimum amount in cents a PR can be in order to be auto-approved|
|»»» max_amount|body|number|false|Maximum amount in cents a PR can be in order to be auto-approved|
|»» per_frequency|body|[PerFrequency](#schemaperfrequency)|true|No description|
|»»» days|body|number|false|Amount of days to apply against the frequency|
|»»» max_amount|body|number|false|Maximum amount in cents the total of all PRs can be for the duration of the frequency|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "contact_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "proposed",
    "status_reason": null,
    "responded_at": null,
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

<h3 id="Propose an Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Created|[ProposeAgreementResponse](#schemaproposeagreementresponse)|

## Approve an Agreement

<a id="opIdApproveAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/agreements/A.2/accept \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements/A.2/accept")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements/A.2/accept",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/agreements/A.2/accept", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/agreements/A.2/accept")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/agreements/A.2/accept');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements/A.2/accept"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /agreements/{agreement_ref}/accept`

Approve an incoming Agreement

<h3 id="Approve-an-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|agreement_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "accepted",
    "status_reason": null,
    "responded_at": "2017-03-20T02:13:11Z",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": 1
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

<h3 id="Approve an Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ApproveAgreementResponse](#schemaapproveagreementresponse)|

## Decline an Agreement

<a id="opIdDeclineAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/agreements/A.2/decline \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements/A.2/decline")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements/A.2/decline",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/agreements/A.2/decline", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/agreements/A.2/decline")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/agreements/A.2/decline');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements/A.2/decline"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /agreements/{agreement_ref}/decline`

Decline an incoming Agreement

<h3 id="Decline-an-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|agreement_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "declined",
    "status_reason": null,
    "responded_at": "2017-03-20T02:13:11Z",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": 1
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

<h3 id="Decline an Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[DeclineAgreementResponse](#schemadeclineagreementresponse)|

## Get an Agreement

<a id="opIdGetAgreement"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/agreements/A.2 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements/A.2")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements/A.2",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/agreements/A.2", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/agreements/A.2")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/agreements/A.2');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements/A.2"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /agreements/{agreement_ref}`

Get a single Agreement by its reference

<h3 id="Get-an-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|agreement_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "approved",
    "status_reason": null,
    "responded_at": "2017-03-20T02:13:11Z",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": 1
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

<h3 id="Get an Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetAgreementResponse](#schemagetagreementresponse)|

## Cancel an Agreement

<a id="opIdCancelAgreement"></a>

> Code samples

```shell
curl --request DELETE \
  --url https://api.sandbox.split.cash/agreements/A.2 \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements/A.2")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "DELETE",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements/A.2",
  "headers": {
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = { 'authorization': "Bearer {access-token}" }

conn.request("DELETE", "/agreements/A.2", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.delete("https://api.sandbox.split.cash/agreements/A.2")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/agreements/A.2');
$request->setRequestMethod('DELETE');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements/A.2"

	req, _ := http.NewRequest("DELETE", url, nil)

	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`DELETE /agreements/{agreement_ref}`

An Agreement can be cancelled by the initiator at any time whilst the authoriser (Agreement recipient) can only cancel a previously accepted Agreement.

<h3 id="Cancel-an-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|agreement_ref|path|string|true|Single value, exact match|

<h3 id="Cancel an Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|

## List incoming Agreements

<a id="opIdListIncomingAgreements"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/agreements/incoming \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements/incoming")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements/incoming",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/agreements/incoming", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/agreements/incoming")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/agreements/incoming');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements/incoming"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /agreements/incoming`

By default, all incoming Agreements will be returned. You can apply filters to your query to customise the returned Agreements.

<h3 id="List-incoming-Agreements-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|
|contact_id|query|string|false|Contact ID (`Contact.data.id`), single value, exact match|
|initiator_id|query|string|false|Initiator ID (`Contact.data.account.id`), single value, exact match|
|status|query|array[string]|false|Multiple values, exact match|

#### Enumerated Values

|Parameter|Value|
|---|---|
|status|proposed|
|status|accepted|
|status|declined|
|status|cancelled|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "A.2",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
      "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
      "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": 1
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      }
    },
    {
      "ref": "A.1",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "56df206a-aaff-471a-b075-11882bc8906a",
      "contact_id": "a80ac411-c8fb-45c0-9557-607c54649907",
      "bank_account_id": "fa80ac411-c8fb-45c0-9557-607c54649907",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-16T22:51:48Z",
      "terms": {
        "per_payout": {
          "max_amount": 5000,
          "min_amount": 0
        },
        "per_frequency": {
          "days": "1",
          "max_amount": 10000
        }
      }
    }
  ]
}
```

<h3 id="List incoming Agreements-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListIncomingAgreementsResponse](#schemalistincomingagreementsresponse)|

## List outgoing Agreements

<a id="opIdListOutgoingAgreements"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/agreements/outgoing \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/agreements/outgoing")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/agreements/outgoing",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/agreements/outgoing", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/agreements/outgoing")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/agreements/outgoing');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/agreements/outgoing"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /agreements/outgoing`

By default, all outgoing Agreements will be returned. You can apply filters to your query to customise the returned Agreements.

<h3 id="List-outgoing-Agreements-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|
|authoriser_id|query|string|false|Authoriser ID (`Contact.data.account.id`), single value, exact match|
|contact_id|query|string|false|Contact ID (`Contact.data.id`), single value, exact match|
|status|query|array[string]|false|Exact match|

#### Enumerated Values

|Parameter|Value|
|---|---|
|status|proposed|
|status|accepted|
|status|declined|
|status|cancelled|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "A.4",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
      "contact_id": "a80ac411-c8fb-45c0-9557-607c54649907",
      "bank_account_id": "fa80ac411-c8fb-45c0-9557-607c54649907",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": 1
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      }
    },
    {
      "ref": "A.3",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "56df206a-aaff-471a-b075-11882bc8906a",
      "contact_id": "a80ac411-c8fb-45c0-9557-607c54649907",
      "bank_account_id": "fa80ac411-c8fb-45c0-9557-607c54649907",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-16T22:51:48Z",
      "terms": {
        "per_payout": {
          "max_amount": 5000,
          "min_amount": 0
        },
        "per_frequency": {
          "days": "1",
          "max_amount": 10000
        }
      }
    }
  ]
}
```

<h3 id="List outgoing Agreements-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListOutgoingAgreementsResponse](#schemalistoutgoingagreementsresponse)|

<h1 id="Split-API-Bank-Accounts">Bank Accounts</h1>

Your currently linked up bank accounts.

## List all Bank Accounts

<a id="opIdListAllBankAccounts"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/bank_accounts \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/bank_accounts")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/bank_accounts",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/bank_accounts", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/bank_accounts")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/bank_accounts');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/bank_accounts"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /bank_accounts`

By default, all Bank Accounts will be returned. There can currently be only 1 active bank account.

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
      "branch_code": "493192",
      "bank_name": "National Australia Bank",
      "account_number": "3993013",
      "status": "active"
    },
    {
      "id": "56df206a-aaff-471a-b075-11882bc8906a",
      "branch_code": "302193",
      "bank_name": "National Australia Bank",
      "account_number": "119302",
      "status": "active"
    }
  ]
}
```

<h3 id="List all Bank Accounts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListAllBankAccountsResponse](#schemalistallbankaccountsresponse)|

<h1 id="Split-API-Bank-Connections">Bank Connections</h1>

Bank connections are read-only connections to your contacts' banking data. This allows Split (and yourself) to make intelligent transactional decisions leading to better outcomes.

Use these endpoints to:

  * List your bank connections and check their status
  * Retrieve a link that you can share with your contact to update their invalidated bank connection (e.g Their online banking credentials have changed)

<aside class="notice">To access these endpoints on behalf of another account via OAuth2, your application must request (and the user must authorise) <a href="#configuration">the bank_connections scope</a>.</aside>
<aside class="notice">Looking to add a new bank connection to a contact? See the <a href="#get-a-contact">get a contact endpoint</a></aside>

## List all Bank Connections

<a id="opIdListAllBankConnections"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/bank_connections \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/bank_connections")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/bank_connections",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/bank_connections", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/bank_connections")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/bank_connections');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/bank_connections"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /bank_connections`

By default, all Bank Connections will be returned. You can apply filters to your query to customise the returned Bank Connection list.

<h3 id="List-all-Bank-Connections-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|
|provider_name|query|string|false|Single value, exact match|
|state|query|string|false|Single value, exact match|

#### Enumerated Values

|Parameter|Value|
|---|---|
|provider_name|basiq|
|provider_name|proviso|
|provider_name|split|
|state|active|
|state|credentials_invalid|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "fb497754-87e2-407d-871f-32aec39d09d4",
      "provider_name": "split",
      "state": "active",
      "refreshed_at": "2020-02-13T20:05:00.000+11:00",
      "removed_at": null,
      "failure_reason": null,
      "institution": {
        "short_name": "CBA",
        "full_name": "Commonwealth Bank of Australia"
      },
      "contact": {
        "id": "626e15b1-aa4a-496e-b5d6-3f8c1a6d2189",
        "name": "George Morissette",
        "email": "randy@windler.net"
      },
      "links": {
        "update_bank_connection": "http://go.split.cash/authorise_bank_connections/williams-and-sons-5/fb497754-87e2-407d-871f-32aec39d09d4"
      }
    },
    {
      "id": "ddbee875-7344-4d74-9503-6233149fe1a0",
      "provider_name": "split",
      "state": "credentials_invalid",
      "refreshed_at": "2020-02-13T20:05:00.000+11:00",
      "removed_at": null,
      "failure_reason": null,
      "institution": {
        "short_name": "CBA",
        "full_name": "Commonwealth Bank of Australia"
      },
      "contact": {
        "id": "72e37667-6364-440f-b1bd-56df5654e258",
        "name": "Joel Boyle",
        "email": "travis@hermanntorp.net"
      },
      "links": {
        "update_bank_connection": "http://go.split.cash/authorise_bank_connections/williams-and-sons-5/ddbee875-7344-4d74-9503-6233149fe1a0"
      }
    }
  ]
}
```

<h3 id="List all Bank Connections-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListAllBankConnectionsResponse](#schemalistallbankconnectionsresponse)|

## Get a Bank Connection

<a id="opIdGetABankConnection"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/bank_connections/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /bank_connections/{id}`

Get a single Bank Connection by its ID

<h3 id="Get-a-Bank-Connection-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string(UUID)|true|BankConnection ID (`BankConnection.data.id`)|

> Example responses

> 200 Response

```json
{
  "data": {
    "id": "c397645b-bd4f-4fc6-b1fe-4993fef6c3c7",
    "provider_name": "split",
    "state": "credentials_invalid",
    "refreshed_at": "2020-02-13T20:01:00.000+11:00",
    "removed_at": null,
    "failure_reason": null,
    "institution": {
      "short_name": "CBA",
      "full_name": "Commonwealth Bank of Australia"
    },
    "contact": {
      "id": "72e37667-6364-440f-b1bd-56df5654e258",
      "name": "Joel Boyle",
      "email": "travis@hermanntorp.net"
    },
    "links": {
      "update_bank_connection": "https://go.sandbox.split.cash/authorise_bank_connections/thomas-morgan-1/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"
    }
  }
}
```

<h3 id="Get a Bank Connection-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetABankConnectionResponse](#schemagetabankconnectionresponse)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not Found|None|

## Remove a Bank Connection

<a id="opIdRemoveABankConnection"></a>

> Code samples

```shell
curl --request DELETE \
  --url https://api.sandbox.split.cash/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "DELETE",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de",
  "headers": {
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = { 'authorization': "Bearer {access-token}" }

conn.request("DELETE", "/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.delete("https://api.sandbox.split.cash/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de');
$request->setRequestMethod('DELETE');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/bank_connections/01586a3f-f25b-404f-b135-c42a1b7ef4de"

	req, _ := http.NewRequest("DELETE", url, nil)

	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`DELETE /bank_connections/{id}`

<aside class="notice">Removing a Bank Connection will not affect your transaction history.</aside>

<h3 id="Remove-a-Bank-Connection-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string(UUID)|true|BankConnection ID (`BankConnection.data.id`)|

<h3 id="Remove a Bank Connection-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No description|None|

<h1 id="Split-API-Contacts">Contacts</h1>

Your Contacts form an address book of parties with whom you can interact. In order to initiate any type of transaction you must first have the party in your Contact list.

<aside class="notice">In the case of Open Payment Requests & Open Agreements, the authorising party will be automatically added to your Contacts list.</aside>

## Add a Split Contact

<a id="opIdAddASplitContact"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/contacts \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"nickname":"outstanding_tours","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"nickname\":\"outstanding_tours\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  nickname: 'outstanding_tours',
  metadata: { custom_key: 'Custom string', another_custom_key: 'Maybe a URL' }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"nickname\":\"outstanding_tours\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/contacts", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/contacts")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"nickname\":\"outstanding_tours\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"nickname":"outstanding_tours","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}');

$request->setRequestUrl('https://api.sandbox.split.cash/contacts');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts"

	payload := strings.NewReader("{\"nickname\":\"outstanding_tours\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /contacts`

Add a Split Contact

> Body parameter

```json
{
  "nickname": "outstanding_tours",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

<h3 id="Add-a-Split-Contact-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[AddASplitContactRequest](#schemaaddasplitcontactrequest)|true|No description|
|» nickname|body|string|true|Split account nickname|
|» metadata|body|[Metadata](#schemametadata)|false|Use for your custom data and certain Split customisations. This data will be attached to the Payment Request and its associated Payouts, Transactions and Webhook Events.|

> Example responses

> 201 Response

```json
{
  "data": {
    "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
    "name": "Outstanding Tours Pty Ltd",
    "email": "accounts@outstandingtours.com.au",
    "type": "Split account",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    },
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "947434694",
      "branch_code": "304304",
      "bank_name": "National Australia Bank",
      "state": "active",
      "iav_provider": null,
      "iav_status": null,
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "account": {
      "id": "77be6ecc-5fa7-454b-86d6-02a5f147878d",
      "nickname": "outstanding_tours",
      "abn": "123456789",
      "name": "Outstanding Tours Pty Ltd"
    },
    "links": {
      "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

<h3 id="Add a Split Contact-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Created|[AddASplitContactResponse](#schemaaddasplitcontactresponse)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not Found|None|

## List all Contacts

<a id="opIdListAllContacts"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/contacts \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/contacts", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/contacts")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/contacts');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /contacts`

By default, all Contacts will be returned. You can apply filters to your query to customise the returned Contact list.

<h3 id="List-all-Contacts-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|
|name|query|string|false|Single value, string search|
|nickname|query|string|false|Single value, string search|
|email|query|string|false|Single value, string search|
|bank_account_id|query|string|false|Single value, exact match|
|bank_account_branch_code|query|string|false|Single value, exact match|
|bank_account_account_number|query|string|false|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
      "name": "Outstanding Tours Pty Ltd",
      "email": "accounts@outstandingtours.com.au",
      "type": "Split account",
      "bank_account": {
        "id": "095c5ab7-7fa8-40fd-b317-cddbbf4c8fbc",
        "account_number": "494307",
        "branch_code": "435434",
        "bank_name": "National Australia Bank",
        "state": "active",
        "iav_provider": "split",
        "iav_status": "active",
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "bank_connection": {
        "id": "c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"
      }
    },
    {
      "id": "49935c67-c5df-4f00-99f4-1413c18a89a0",
      "name": "Adventure Dudes Pty Ltd",
      "email": "accounts@adventuredudes.com.au",
      "type": "Split account",
      "bank_account": {
        "id": "861ff8e4-7acf-4897-9e53-e7c5ae5f7cc0",
        "account_number": "4395959",
        "branch_code": "068231",
        "bank_name": "National Australia Bank",
        "state": "active",
        "iav_provider": "split",
        "iav_status": "credentials_invalid",
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "bank_connection": {
        "id": "c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"
      }
    },
    {
      "id": "eb3266f9-e172-4b6c-b802-fe5ac4d3250a",
      "name": "Surfing World Pty Ltd",
      "email": "accounts@surfingworld.com.au",
      "type": "Split account",
      "bank_account": {
        "id": null,
        "account_number": null,
        "branch_code": null,
        "bank_name": null,
        "state": "disabled",
        "iav_provider": null,
        "iav_status": null,
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "links": {
        "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
      }
    },
    {
      "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
      "name": "Hunter Thompson",
      "email": "hunter@batcountry.com",
      "type": "anyone",
      "bank_account": {
        "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
        "account_number": "13048322",
        "branch_code": "123456",
        "bank_name": "National Australia Bank",
        "state": "pending_verification",
        "iav_provider": null,
        "iav_status": null,
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "links": {
        "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
      }
    }
  ]
}
```

<h3 id="List all Contacts-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListAllContactsResponse](#schemalistallcontactsresponse)|

## Add an Anyone Contact

<a id="opIdAddAnAnyoneContact"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/contacts/anyone \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"name":"Hunter Thompson","email":"hunter@batcountry.com","branch_code":"123456","account_number":"13048322","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts/anyone")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"name\":\"Hunter Thompson\",\"email\":\"hunter@batcountry.com\",\"branch_code\":\"123456\",\"account_number\":\"13048322\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts/anyone",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  name: 'Hunter Thompson',
  email: 'hunter@batcountry.com',
  branch_code: '123456',
  account_number: '13048322',
  metadata: { custom_key: 'Custom string', another_custom_key: 'Maybe a URL' }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"name\":\"Hunter Thompson\",\"email\":\"hunter@batcountry.com\",\"branch_code\":\"123456\",\"account_number\":\"13048322\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/contacts/anyone", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/contacts/anyone")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"name\":\"Hunter Thompson\",\"email\":\"hunter@batcountry.com\",\"branch_code\":\"123456\",\"account_number\":\"13048322\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"name":"Hunter Thompson","email":"hunter@batcountry.com","branch_code":"123456","account_number":"13048322","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}');

$request->setRequestUrl('https://api.sandbox.split.cash/contacts/anyone');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts/anyone"

	payload := strings.NewReader("{\"name\":\"Hunter Thompson\",\"email\":\"hunter@batcountry.com\",\"branch_code\":\"123456\",\"account_number\":\"13048322\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /contacts/anyone`

When you want to pay somebody that doesn't have a Split account, you can add them as an Anyone Contact.

<aside class="notice">
  Anyone Contacts added this way can be used solely as payout recipients in a Payment.
</aside>

> Body parameter

```json
{
  "name": "Hunter Thompson",
  "email": "hunter@batcountry.com",
  "branch_code": "123456",
  "account_number": "13048322",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

<h3 id="Add-an-Anyone-Contact-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[AddAnAnyoneContactRequest](#schemaaddananyonecontactrequest)|true|No description|
|» name|body|string|true|The name of the Contact|
|» email|body|string|true|The email of the Contact|
|» branch_code|body|string|true|The bank account BSB of the Contact|
|» account_number|body|string|true|The bank account number of the Contact|
|» metadata|body|[Metadata](#schemametadata)|false|Use for your custom data and certain Split customisations. This data will be attached to the Payment Request and its associated Payouts, Transactions and Webhook Events.|

> Example responses

> 201 Response

```json
{
  "data": {
    "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
    "name": "Hunter Thompson",
    "email": "hunter@batcountry.com",
    "type": "anyone",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    },
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "13048322",
      "branch_code": "123456",
      "bank_name": "National Australia Bank",
      "state": "pending_verification",
      "iav_provider": null,
      "iav_status": null,
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "links": {
      "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

<h3 id="Add an Anyone Contact-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Created|[AddAnAnyoneContactResponse](#schemaaddananyonecontactresponse)|

## Get a Contact

<a id="opIdGetAContact"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /contacts/{id}`

Get a single Contact by its ID

<h3 id="Get-a-Contact-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string(UUID)|true|Contact ID (`Contact.data.id`)|

> Example responses

> 200 Response

```json
{
  "data": {
    "id": "fcabeacb-2ef6-4b27-ba19-4f6fa0d57dcb",
    "name": "Outstanding Tours Pty Ltd",
    "email": "accounts@outstandingtours.com.au",
    "type": "Split account",
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "947434694",
      "branch_code": "304304",
      "bank_name": "National Australia Bank",
      "state": "active",
      "iav_provider": null,
      "iav_status": null,
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "links": {
      "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

<h3 id="Get a Contact-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetAContactResponse](#schemagetacontactresponse)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not Found|None|

## Remove a Contact

<a id="opIdRemoveAContact"></a>

> Code samples

```shell
curl --request DELETE \
  --url https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608 \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "DELETE",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608",
  "headers": {
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = { 'authorization': "Bearer {access-token}" }

conn.request("DELETE", "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.delete("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608');
$request->setRequestMethod('DELETE');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608"

	req, _ := http.NewRequest("DELETE", url, nil)

	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`DELETE /contacts/{id}`

<aside class="notice">Removing a Contact will not affect your transaction history.</aside>

<h3 id="Remove-a-Contact-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string(UUID)|true|Contact ID (`Contact.data.id`)|

<h3 id="Remove a Contact-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No description|None|

## Update a Contact

<a id="opIdUpdateAContact"></a>

> Code samples

```shell
curl --request PATCH \
  --url https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"name":"My very own alias","email":"updated@email.com"}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Patch.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"name\":\"My very own alias\",\"email\":\"updated@email.com\"}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "PATCH",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({ name: 'My very own alias', email: 'updated@email.com' }));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"name\":\"My very own alias\",\"email\":\"updated@email.com\"}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("PATCH", "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.patch("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"name\":\"My very own alias\",\"email\":\"updated@email.com\"}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"name":"My very own alias","email":"updated@email.com"}');

$request->setRequestUrl('https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608');
$request->setRequestMethod('PATCH');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608"

	payload := strings.NewReader("{\"name\":\"My very own alias\",\"email\":\"updated@email.com\"}")

	req, _ := http.NewRequest("PATCH", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`PATCH /contacts/{id}`

You can update the name and email of any Contact. This is essentially an alias you can use to provide something contextually meaningful.

<aside class="notice">
  Any previous transactions to that Contact will retain the Contact name that was current at the time.
</aside>

> Body parameter

```json
{
  "name": "My very own alias",
  "email": "updated@email.com"
}
```

<h3 id="Update-a-Contact-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string|true|Contact ID (`Contact.data.id`)|
|body|body|[UpdateAContactRequest](#schemaupdateacontactrequest)|true|No description|
|» name|body|string|false|The name of the Contact|
|» email|body|string|false|The email of the Contact|

> Example responses

> 200 Response

```json
{
  "data": {
    "id": "fcabeacb-2ef6-4b27-ba19-4f6fa0d57dcb",
    "name": "My very own alias",
    "email": "accounts@outstandingtours.com.au",
    "type": "Split account",
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "947434694",
      "branch_code": "304304",
      "bank_name": "National Australia Bank",
      "state": "active",
      "iav_provider": "split",
      "iav_status": "active",
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "bank_connection": {
      "id": "1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

<h3 id="Update a Contact-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[UpdateAContactResponse](#schemaupdateacontactresponse)|
|404|[Not Found](https://tools.ietf.org/html/rfc7231#section-6.5.4)|Not Found|None|

## Refresh contact bank connection

<a id="opIdRefreshBalanceContact"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance",
  "headers": {
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = { 'authorization': "Bearer {access-token}" }

conn.request("POST", "/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/contacts/55afddde-4296-4daf-8e49-7ba481ef9608/refresh_balance"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /contacts/{id}/refresh_balance`

Request the bank connection for a contact to refresh available funds. This is intended for use in conjunction with the `precheck_funds` option, see [Payment Request - Precheck Funds](/#precheck-funds-lifecycle)

<h3 id="Refresh-contact-bank-connection-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|id|path|string(UUID)|true|Contact ID (`Contact.data.id`)|

<h3 id="Refresh contact bank connection-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content (success)|None|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|Unprocessable Entity (errors)|None|

<h1 id="Split-API-Open-Agreements">Open Agreements</h1>

An Open Agreement is essentially an Agreement template with no specific authoriser. Each time an Open Agreement is accepted by either a Split account holder or anyone, the authoriser is added to your Contacts list and a new Agreement is automatically created between the Open Agreement initiator and the authoriser.

An Open Agreement can be accepted multiple times by different parties and the result is the same: A new Agreement. Additionally, an Open Agreement can be accepted by anybody, not just other Split users. This is achieved by using our [Instant Account Verification process](http://help.split.cash/bank-accounts/instant-account-verification-iav) as part of accepting an [Open Agreement](https://help.split.cash/agreements/open-agreement).

##Lifecycle

An Open Agreement can have the following statuses:

| Status | Description |
|-------|-------------|
| `active` | The Open Agreement can be viewed and accepted. |
| `closed` | The Open Agreement can not be viewed or accepted. |

<aside class="notice">When you close an Open Agreement, no new Agreements can be created from it and all past Agreement resulting from the acceptance of the Open Agreement are unaffected.</aside>

## Create an Open Agreement

<a id="opIdCreateOpenAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/open_agreements \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"title":"Subscription Plan A","terms":{"per_payout":{"min_amount":null,"max_amount":10000},"per_frequency":{"days":7,"max_amount":1000000}}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/open_agreements")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"title\":\"Subscription Plan A\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/open_agreements",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  title: 'Subscription Plan A',
  terms: {
    per_payout: { min_amount: null, max_amount: 10000 },
    per_frequency: { days: 7, max_amount: 1000000 }
  }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"title\":\"Subscription Plan A\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/open_agreements", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/open_agreements")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"title\":\"Subscription Plan A\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"title":"Subscription Plan A","terms":{"per_payout":{"min_amount":null,"max_amount":10000},"per_frequency":{"days":7,"max_amount":1000000}}}');

$request->setRequestUrl('https://api.sandbox.split.cash/open_agreements');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/open_agreements"

	payload := strings.NewReader("{\"title\":\"Subscription Plan A\",\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /open_agreements`

Create an Open Agreement that can be accepted by anyone.

<aside class="notice">You can set any of the term metrics to <code>null</code> if you wish them to not have a limit.</aside>

> Body parameter

```json
{
  "title": "Subscription Plan A",
  "terms": {
    "per_payout": {
      "min_amount": null,
      "max_amount": 10000
    },
    "per_frequency": {
      "days": 7,
      "max_amount": 1000000
    }
  }
}
```

<h3 id="Create-an-Open-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[CreateOpenAgreementRequest](#schemacreateopenagreementrequest)|true|No description|
|» title|body|string|true|Title of the Open Agreement (Visible to authorisers)|
|» terms|body|[Terms](#schematerms)|true|Terms|
|»» per_payout|body|[PerPayout](#schemaperpayout)|true|No description|
|»»» min_amount|body|number|false|Minimum amount in cents a PR can be in order to be auto-approved|
|»»» max_amount|body|number|false|Maximum amount in cents a PR can be in order to be auto-approved|
|»» per_frequency|body|[PerFrequency](#schemaperfrequency)|true|No description|
|»»» days|body|number|false|Amount of days to apply against the frequency|
|»»» max_amount|body|number|false|Maximum amount in cents the total of all PRs can be for the duration of the frequency|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "OA.1",
    "title": "Subscription Plan A",
    "status": "active",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "min_amount": null,
        "max_amount": 10000
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "link": "https://go.sandbox.split.cash/open_agreements/aecff76d-13e0-4e17-b7c5-844ae07fa675/agreements/invitation"
  }
}
```

<h3 id="Create an Open Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Created|[CreateOpenAgreementResponse](#schemacreateopenagreementresponse)|

## List all Open Agreements

<a id="opIdListAllOpenAgreements"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/open_agreements \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/open_agreements")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/open_agreements",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/open_agreements", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/open_agreements")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/open_agreements');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/open_agreements"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /open_agreements`

<h3 id="List-all-Open-Agreements-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "OA.1",
      "title": "Subscription Plan A",
      "status": "active",
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": null
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      },
      "link": "https://go.sandbox.split.cash/open_agreements/2e002c0b-ffaf-44eb-a35c-15fa384ddde1/agreements/invitation"
    },
    {
      "ref": "OA.2",
      "title": "Subscription Plan B",
      "status": "closed",
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": null
        },
        "per_frequency": {
          "days": null,
          "max_amount": null
        }
      },
      "link": "https://go.sandbox.split.cash/open_agreements/948e3662-154d-42d1-bdc9-a05c251d3981/agreements/invitation"
    }
  ]
}
```

<h3 id="List all Open Agreements-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListAllOpenAgreementsResponse](#schemalistallopenagreementsresponse)|

## Activate a closed Open Agreement

<a id="opIdActivateOpenAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/open_agreements/OA.1/activate \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/open_agreements/OA.1/activate")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/open_agreements/OA.1/activate",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/open_agreements/OA.1/activate", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/open_agreements/OA.1/activate")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/open_agreements/OA.1/activate');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/open_agreements/OA.1/activate"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /open_agreements/{open_agreement_ref}/activate`

Allow the Open Agreement to viewed and accepted

<h3 id="Activate-a-closed-Open-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|open_agreement_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "OA.1",
    "title": "Subscription Plan A",
    "status": "active",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "link": "https://go.sandbox.split.cash/open_agreements/2e002c0b-ffaf-44eb-a35c-15fa384ddde1/agreements/invitation"
  }
}
```

<h3 id="Activate a closed Open Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ActivateOpenAgreementResponse](#schemaactivateopenagreementresponse)|

## Close an active Open Agreement

<a id="opIdCloseOpenAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/open_agreements/OA.1/close \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/open_agreements/OA.1/close")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/open_agreements/OA.1/close",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/open_agreements/OA.1/close", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/open_agreements/OA.1/close")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/open_agreements/OA.1/close');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/open_agreements/OA.1/close"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /open_agreements/{open_agreement_ref}/close`

Disable the Open Agreement from being viewed or accepted

<h3 id="Close-an-active-Open-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|open_agreement_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "OA.1",
    "title": "Subscription Plan A",
    "status": "closed",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "link": "https://go.sandbox.split.cash/open_agreements/2e002c0b-ffaf-44eb-a35c-15fa384ddde1/agreements/invitation"
  }
}
```

<h3 id="Close an active Open Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[CloseOpenAgreementResponse](#schemacloseopenagreementresponse)|

<h1 id="Split-API-Payment-Requests">Payment Requests</h1>

A Payment Request (PR) is used to request payment from another party. Additionally, a Payment Request can be approved by anybody, not just other Split users. This is achieved by using our [Instant Account Verification process](http://help.split.cash/bank-accounts/instant-account-verification-iav) as part of approving a [Payment Request](https://help.split.cash/payment-requests/open-payment-requests).

<div class="middle-header">Applicable scenarios</div>

1. **A Split account sends a PR to a Contact that is a Split account:**
    1. The authoriser must either approve or decline the request; **or**
    2. Given there is an Agreement in place and the PR is within the terms of the Agreement, then the PR will be automatically approved; **or**
    3. Given the PR is **not** within the terms of the Agreement, then the authoriser must either approve or decline the request.
2. **A Split accounts sends a PR to a Contact that is not a Split account:**
    1. Given there is an Agreement in place and the PR is within the terms of the Agreement, then the PR will be automatically approved; **or**
    2. Given the PR is **not** within the terms of the Agreement, then the PR will not be created; **or**
    3. There is no Agreement in place, then the PR will not be created.

<div class="middle-header">Direction</div>

Payment Requests are broken up by direction:

1. **Incoming:** An incoming Payment Request (you are the authoriser/payer)
2. **Outgoing:** An outgoing Payment Request (you are the initiator/payee)

There are two response fields that differ depending on the direction:

| Field | Description |
|-------|-------------|
| `debit_ref` | Only visible to the PR authoriser (incoming PRs). This reference corresponds to the newly created debit to process the approved PR. |
| `credit_ref` | Only visible to the PR initiator (outgoing PRs). This reference corresponds to the newly created credit from the approved PR. |

##Lifecycle

A Payment Request can have the following statuses:

| Status | Description |
|-------|-------------|
| `pending_approval` | Waiting for the authoriser to approve the PR. |
| `unverified` | Waiting for available funds response (only applicable when `precheck_funds` enabled). |
| `approved` | The authoriser has approved the PR. |
| `declined` | The payer has declined the PR. |
| `cancelled` | The initiator has cancelled the PR. |

### Precheck funds lifecycle

Split will automatically check for available funds right before debits are sent to the bank. This check is performed for all contacts with [Instant Account Verification (IAV)](http://help.split.cash/bank-accounts/instant-account-verification-iav).

If you would like to also precheck available funds before a Payment Request is allowed to be approved, there is an optional precheck attribute as outlined below.

**Precheck for Payment Requests**

When the `precheck_funds` option is enabled, prechecking of available funds will be completed before approving the Payment Request. The authoriser contact must have valid agreement and bank connection to make use of this option.

There are **synchronous** and **asynchronous** lifecycles when the `precheck_funds` option is enabled.

+ When the available funds for a contact's bank account are considered out of date, the API response will return the Payment Request with a state of `unverified` while we refresh and ensure there are sufficient available funds. Once the precheck has completed, the Payment Request state will transition to either `approved` or `declined`. This process can be followed by subscribing to the relevant webhook events or regularly polling the Payment Request and verifying its status.
+ When the available funds for a contact are current, the API will respond immediately with a final state of either `approved` or an error message if there are insufficient funds.

You can gain some control over this process by preemptively telling Split to refresh a contact's available balance at least 1 minute before making a Payment Request. See [Contact balance refresh](/#refresh-contact-bank-connection) for more.

## Request Payment

<a id="opIdMakeAPaymentRequest"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/payment_requests \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"description":"Visible to both initiator and authoriser","matures_at":"2016-12-19T02:10:56Z","amount":99000,"authoriser_contact_id":"de86472c-c027-4735-a6a7-234366a27fc7","precheck_funds":"false","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"description\":\"Visible to both initiator and authoriser\",\"matures_at\":\"2016-12-19T02:10:56Z\",\"amount\":99000,\"authoriser_contact_id\":\"de86472c-c027-4735-a6a7-234366a27fc7\",\"precheck_funds\":\"false\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  description: 'Visible to both initiator and authoriser',
  matures_at: '2016-12-19T02:10:56Z',
  amount: 99000,
  authoriser_contact_id: 'de86472c-c027-4735-a6a7-234366a27fc7',
  precheck_funds: 'false',
  metadata: { custom_key: 'Custom string', another_custom_key: 'Maybe a URL' }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"description\":\"Visible to both initiator and authoriser\",\"matures_at\":\"2016-12-19T02:10:56Z\",\"amount\":99000,\"authoriser_contact_id\":\"de86472c-c027-4735-a6a7-234366a27fc7\",\"precheck_funds\":\"false\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/payment_requests", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/payment_requests")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"description\":\"Visible to both initiator and authoriser\",\"matures_at\":\"2016-12-19T02:10:56Z\",\"amount\":99000,\"authoriser_contact_id\":\"de86472c-c027-4735-a6a7-234366a27fc7\",\"precheck_funds\":\"false\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"description":"Visible to both initiator and authoriser","matures_at":"2016-12-19T02:10:56Z","amount":99000,"authoriser_contact_id":"de86472c-c027-4735-a6a7-234366a27fc7","precheck_funds":"false","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}');

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests"

	payload := strings.NewReader("{\"description\":\"Visible to both initiator and authoriser\",\"matures_at\":\"2016-12-19T02:10:56Z\",\"amount\":99000,\"authoriser_contact_id\":\"de86472c-c027-4735-a6a7-234366a27fc7\",\"precheck_funds\":\"false\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /payment_requests`

<aside class="notice">[23/07/2018] Please note that we've deprecated <code>authoriser_id</code> in favour of <code>authoriser_contact_id</code>. We will continue to support the use of <code>authoriser_id (Contact.data.bank_account.id)</code> but encourage you to use <code>authoriser_contact_id (Contact.data.id)</code> to identify the authoriser of the Payment Request.</aside>

> Body parameter

```json
{
  "description": "Visible to both initiator and authoriser",
  "matures_at": "2016-12-19T02:10:56Z",
  "amount": 99000,
  "authoriser_contact_id": "de86472c-c027-4735-a6a7-234366a27fc7",
  "precheck_funds": "false",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

<h3 id="Request-Payment-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[MakeAPaymentRequestRequest](#schemamakeapaymentrequestrequest)|true|No description|
|» description|body|string|true|Description visible to the initiator (payee) & authoriser (payer)|
|» matures_at|body|string|true|Date & time in UTC ISO8601 that the Payment will be processed if the request is approved. (If the request is approved after this point in time, it will be processed straight away)|
|» amount|body|number|true|Amount in cents to pay the initiator|
|» authoriser_contact_id|body|string|true|The Contact the payment will be requested from (`Contact.data.id`)'|
|» precheck_funds|body|boolean|false|Enforce prechecking of available funds before approving the Payment Request. see [Payment Request - Precheck Funds](/#precheck-funds-lifecycle)|
|» metadata|body|[Metadata](#schemametadata)|false|Use for your custom data and certain Split customisations. This data will be attached to the Payment Request and its associated Payouts, Transactions and Webhook Events.|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "970e4526-67d9-4ed9-b554-f5cf390ab775",
    "authoriser_contact_id": "de86472c-c027-4735-a6a7-234366a27fc7",
    "schedule_ref": null,
    "status": "pending_approval",
    "responded_at": null,
    "created_at": "2016-12-19T02:10:56Z",
    "credit_ref": null,
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Request Payment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Created|[MakeAPaymentRequestResponse](#schemamakeapaymentrequestresponse)|
|422|[Unprocessable Entity](https://tools.ietf.org/html/rfc2518#section-10.3)|When a payment is requested from an Anyone Contact with no valid Agreement|[MakeAPaymentRequestWithNoAgreementResponse](#schemamakeapaymentrequestwithnoagreementresponse)|

## Approve a Payment Request

<a id="opIdApprovePaymentRequest"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/payment_requests/PR.3/approve \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/PR.3/approve")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/PR.3/approve",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/payment_requests/PR.3/approve", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/payment_requests/PR.3/approve")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/PR.3/approve');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/PR.3/approve"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /payment_requests/{payment_request_ref}/approve`

<h3 id="Approve-a-Payment-Request-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|payment_request_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "d194c54b-9183-410c-966b-50485c5ce3f0",
    "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
    "schedule_ref": null,
    "status": "approved",
    "responded_at": "2016-12-19T02:38:04Z",
    "created_at": "2016-12-19T02:10:56Z",
    "debit_ref": "D.b",
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Approve a Payment Request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ApprovePaymentRequestResponse](#schemaapprovepaymentrequestresponse)|

## Decline a Payment Request

<a id="opIdDeclinePaymentRequest"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/payment_requests/PR.3/decline \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/PR.3/decline")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/PR.3/decline",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/payment_requests/PR.3/decline", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/payment_requests/PR.3/decline")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/PR.3/decline');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/PR.3/decline"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /payment_requests/{payment_request_ref}/decline`

<h3 id="Decline-a-Payment-Request-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|payment_request_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "d194c54b-9183-410c-966b-50485c5ce3f0",
    "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
    "schedule_ref": null,
    "status": "declined",
    "responded_at": "2016-12-19T02:38:04Z",
    "created_at": "2016-12-19T02:10:56Z",
    "debit_ref": null,
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Decline a Payment Request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|No description|[DeclinePaymentRequestResponse](#schemadeclinepaymentrequestresponse)|

## Get a Payment Request

<a id="opIdGetAPaymentRequest"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/payment_requests/PR.3 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/PR.3")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/PR.3",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/payment_requests/PR.3", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/payment_requests/PR.3")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/PR.3');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/PR.3"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /payment_requests/{payment_request_ref}`

<h3 id="Get-a-Payment-Request-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|payment_request_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "970e4526-67d9-4ed9-b554-f5cf390ab775",
    "authoriser_contact_id": "de86472c-c027-4735-a6a7-234366a27fc7",
    "schedule_ref": null,
    "status": "approved",
    "responded_at": "2016-12-19T02:38:04Z",
    "created_at": "2016-12-19T02:10:56Z",
    "credit_ref": "C.b",
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Get a Payment Request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetAPaymentRequestResponse](#schemagetapaymentrequestresponse)|

## Cancel a Payment Request

<a id="opIdCancelAPaymentRequest"></a>

> Code samples

```shell
curl --request DELETE \
  --url https://api.sandbox.split.cash/payment_requests/PR.3 \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/PR.3")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "DELETE",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/PR.3",
  "headers": {
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = { 'authorization': "Bearer {access-token}" }

conn.request("DELETE", "/payment_requests/PR.3", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.delete("https://api.sandbox.split.cash/payment_requests/PR.3")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/PR.3');
$request->setRequestMethod('DELETE');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/PR.3"

	req, _ := http.NewRequest("DELETE", url, nil)

	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`DELETE /payment_requests/{payment_request_ref}`

A Payment Request can be cancelled as long as the associated transaction's state is maturing or matured.

<h3 id="Cancel-a-Payment-Request-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|payment_request_ref|path|string|true|Single value, exact match|

<h3 id="Cancel a Payment Request-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|

## List incoming Payment Requests

<a id="opIdListIncomingPaymentRequests"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/payment_requests/incoming \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/incoming")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/incoming",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/payment_requests/incoming", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/payment_requests/incoming")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/incoming');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/incoming"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /payment_requests/incoming`

<h3 id="List-incoming-Payment-Requests-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "PR.2",
      "initiator_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "schedule_ref": "PRS.1",
      "status": "approved",
      "responded_at": "2016-12-19T02:10:18Z",
      "created_at": "2016-12-19T02:09:09Z",
      "debit_ref": "D.a",
      "payout": {
        "amount": 30000,
        "description": "The SuperPackage",
        "matures_at": "2016-12-20T00:00:00Z"
      }
    },
    {
      "ref": "PR.3",
      "initiator_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "schedule_ref": null,
      "status": "pending_approval",
      "responded_at": null,
      "created_at": "2016-12-19T02:10:56Z",
      "debit_ref": null,
      "payout": {
        "amount": 99000,
        "description": "The elite package for 4",
        "matures_at": "2016-12-25T00:00:00Z"
      },
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

<h3 id="List incoming Payment Requests-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListIncomingPaymentRequestsResponse](#schemalistincomingpaymentrequestsresponse)|

## List outgoing Payment Requests

<a id="opIdListOutgoingPaymentRequests"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/payment_requests/outgoing \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/outgoing")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/outgoing",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/payment_requests/outgoing", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/payment_requests/outgoing")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/outgoing');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/outgoing"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /payment_requests/outgoing`

<h3 id="List-outgoing-Payment-Requests-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "PR.4",
      "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "schedule_ref": "PRS.2",
      "status": "approved",
      "responded_at": "2016-12-19T02:10:18Z",
      "created_at": "2016-12-19T02:09:09Z",
      "credit_ref": "C.a",
      "payout": {
        "amount": 30000,
        "description": "The SuperPackage",
        "matures_at": "2016-12-20T00:00:00Z"
      }
    },
    {
      "ref": "PR.5",
      "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "schedule_ref": null,
      "status": "pending_approval",
      "responded_at": null,
      "created_at": "2016-12-19T02:10:56Z",
      "credit_ref": null,
      "payout": {
        "amount": 99000,
        "description": "The elite package for 4",
        "matures_at": "2016-12-25T00:00:00Z"
      }
    }
  ]
}
```

<h3 id="List outgoing Payment Requests-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListOutgoingPaymentRequestsResponse](#schemalistoutgoingpaymentrequestsresponse)|

## Get a Payment Request's history

<a id="opIdGetAPaymentRequest'sHistory"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/payment_requests/PR.3/history \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payment_requests/PR.3/history")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payment_requests/PR.3/history",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/payment_requests/PR.3/history", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/payment_requests/PR.3/history")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payment_requests/PR.3/history');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payment_requests/PR.3/history"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /payment_requests/{payment_request_ref}/history`

Gives you visibility of the entire Payment Request lifecycle including the generated debit and credit transactions.

<h3 id="Get-a-Payment-Request's-history-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|payment_request_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "type": "payout_request",
      "event": "requested",
      "at": "2017-01-05T07:47:45Z",
      "ref": "PR.3",
      "by": "Fancy Pants (fancy_pants)"
    },
    {
      "type": "payout_request",
      "event": "approved",
      "at": "2017-01-07T06:13:52Z",
      "ref": "PR.3",
      "by": "Lycra Co (lycra_co)"
    },
    {
      "type": "debit",
      "event": "scheduled",
      "at": "2017-01-07T06:13:52Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "scheduled",
      "at": "2017-01-07T06:13:52Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "matured",
      "at": "2017-01-08T04:30:14Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "processing",
      "at": "2017-01-08T04:30:14Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "clearing",
      "at": "2017-01-08T19:02:20Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "cleared",
      "at": "2017-01-11T19:07:52Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "matured",
      "at": "2017-01-11T19:07:52Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "processing",
      "at": "2017-01-12T04:30:25Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "clearing",
      "at": "2017-01-12T05:17:32Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "cleared",
      "at": "2017-01-15T05:27:12Z",
      "ref": "C.e",
      "by": "Split Payments"
    }
  ]
}
```

<h3 id="Get a Payment Request's history-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetAPaymentRequestsHistoryResponse](#schemagetapaymentrequestshistoryresponse)|

<h1 id="Split-API-Payments">Payments</h1>

**A Payment is made up of two parts:**

1. General details about the Payment.
2. One or many Payouts with individual recipients, amounts and descriptions.

##Lifecycle
> Example payout reversal response

```json
{
  "data": [
  {
    "ref": "C.3",
    "parent_ref": "PB.1",
    "type": "credit",
    "category": "payout_reversal",
    "created_at": "2016-12-07T23:15:00Z",
    "matures_at": "2016-12-10T23:15:00Z",
    "cleared_at": null,
    "bank_ref": null,
    "status": "maturing",
    "status_changed_at": "2016-12-08T23:15:00Z",
    "party_contact_id": "26297f44-c5e1-40a1-9864-3e0b0754c32a",
    "party_name": "Sanford-Rees",
    "party_nickname": "sanford-rees-8",
    "description": "Payout reversal of D.1 for Sanford-Rees due to no account or incorrect account number"
      "amount": 1,
    "reversal_details": {
      "source_debit_ref": "D.1",
      "source_credit_failure_reason": "incorrect_account_number",
    }
  }
  ]
}
```
The Payment is simply a group of Payouts therefore it does not have a particular status. Its Payouts however have their status regularly updated, for a list of possible Payout statuses check out the [Transactions](/#Split-API-Transactions).

When Split is unable to credit funds to a recipient, we will automatically create a payout reversal credit back to the payer. Furthermore, within the payout reversal credit, Split will include details in the `description` and under the `reversal_details` key as to why the original credit to the recipient failed.

## Make a Payment

<a id="opIdMakeAPayment"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/payments \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"description":"The SuperPackage","matures_at":"2016-09-13T00:00:00Z","payouts":[{"amount":30000,"description":"A tandem skydive jump SB23094","recipient_contact_id":"48b89364-1577-4c81-ba02-96705895d457","metadata":{"invoice_ref":"BILL-0001","invoice_id":"c80a9958-e805-47c0-ac2a-c947d7fd778d","custom_key":"Custom string","another_custom_key":"Maybe a URL"}},{"amount":30000,"description":"A scuba dive SDS5464","recipient_contact_id":"dc6f1e60-3803-43ca-a200-7d641816f57f"}],"metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payments")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"description\":\"The SuperPackage\",\"matures_at\":\"2016-09-13T00:00:00Z\",\"payouts\":[{\"amount\":30000,\"description\":\"A tandem skydive jump SB23094\",\"recipient_contact_id\":\"48b89364-1577-4c81-ba02-96705895d457\",\"metadata\":{\"invoice_ref\":\"BILL-0001\",\"invoice_id\":\"c80a9958-e805-47c0-ac2a-c947d7fd778d\",\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}},{\"amount\":30000,\"description\":\"A scuba dive SDS5464\",\"recipient_contact_id\":\"dc6f1e60-3803-43ca-a200-7d641816f57f\"}],\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payments",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  description: 'The SuperPackage',
  matures_at: '2016-09-13T00:00:00Z',
  payouts: [
    {
      amount: 30000,
      description: 'A tandem skydive jump SB23094',
      recipient_contact_id: '48b89364-1577-4c81-ba02-96705895d457',
      metadata: {
        invoice_ref: 'BILL-0001',
        invoice_id: 'c80a9958-e805-47c0-ac2a-c947d7fd778d',
        custom_key: 'Custom string',
        another_custom_key: 'Maybe a URL'
      }
    },
    {
      amount: 30000,
      description: 'A scuba dive SDS5464',
      recipient_contact_id: 'dc6f1e60-3803-43ca-a200-7d641816f57f'
    }
  ],
  metadata: { custom_key: 'Custom string', another_custom_key: 'Maybe a URL' }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"description\":\"The SuperPackage\",\"matures_at\":\"2016-09-13T00:00:00Z\",\"payouts\":[{\"amount\":30000,\"description\":\"A tandem skydive jump SB23094\",\"recipient_contact_id\":\"48b89364-1577-4c81-ba02-96705895d457\",\"metadata\":{\"invoice_ref\":\"BILL-0001\",\"invoice_id\":\"c80a9958-e805-47c0-ac2a-c947d7fd778d\",\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}},{\"amount\":30000,\"description\":\"A scuba dive SDS5464\",\"recipient_contact_id\":\"dc6f1e60-3803-43ca-a200-7d641816f57f\"}],\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/payments", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/payments")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"description\":\"The SuperPackage\",\"matures_at\":\"2016-09-13T00:00:00Z\",\"payouts\":[{\"amount\":30000,\"description\":\"A tandem skydive jump SB23094\",\"recipient_contact_id\":\"48b89364-1577-4c81-ba02-96705895d457\",\"metadata\":{\"invoice_ref\":\"BILL-0001\",\"invoice_id\":\"c80a9958-e805-47c0-ac2a-c947d7fd778d\",\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}},{\"amount\":30000,\"description\":\"A scuba dive SDS5464\",\"recipient_contact_id\":\"dc6f1e60-3803-43ca-a200-7d641816f57f\"}],\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"description":"The SuperPackage","matures_at":"2016-09-13T00:00:00Z","payouts":[{"amount":30000,"description":"A tandem skydive jump SB23094","recipient_contact_id":"48b89364-1577-4c81-ba02-96705895d457","metadata":{"invoice_ref":"BILL-0001","invoice_id":"c80a9958-e805-47c0-ac2a-c947d7fd778d","custom_key":"Custom string","another_custom_key":"Maybe a URL"}},{"amount":30000,"description":"A scuba dive SDS5464","recipient_contact_id":"dc6f1e60-3803-43ca-a200-7d641816f57f"}],"metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}');

$request->setRequestUrl('https://api.sandbox.split.cash/payments');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payments"

	payload := strings.NewReader("{\"description\":\"The SuperPackage\",\"matures_at\":\"2016-09-13T00:00:00Z\",\"payouts\":[{\"amount\":30000,\"description\":\"A tandem skydive jump SB23094\",\"recipient_contact_id\":\"48b89364-1577-4c81-ba02-96705895d457\",\"metadata\":{\"invoice_ref\":\"BILL-0001\",\"invoice_id\":\"c80a9958-e805-47c0-ac2a-c947d7fd778d\",\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}},{\"amount\":30000,\"description\":\"A scuba dive SDS5464\",\"recipient_contact_id\":\"dc6f1e60-3803-43ca-a200-7d641816f57f\"}],\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /payments`

> Body parameter

```json
{
  "description": "The SuperPackage",
  "matures_at": "2016-09-13T00:00:00Z",
  "payouts": [
    {
      "amount": 30000,
      "description": "A tandem skydive jump SB23094",
      "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
      "metadata": {
        "invoice_ref": "BILL-0001",
        "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    },
    {
      "amount": 30000,
      "description": "A scuba dive SDS5464",
      "recipient_contact_id": "dc6f1e60-3803-43ca-a200-7d641816f57f"
    }
  ],
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

<h3 id="Make-a-Payment-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[MakeAPaymentRequest](#schemamakeapaymentrequest)|true|No description|
|» description|body|string|true|User description. Only visible to the payer|
|» matures_at|body|string|true|Date & time in UTC ISO8601 the Payment should be processed. (Can not be earlier than the start of current day)|
|» payouts|body|[[Payout](#schemapayout)]|true|One or many Payouts|
|»» Payout|body|[Payout](#schemapayout)|false|The actual Payout|
|»»» amount|body|number|true|Amount in cents to pay the recipient|
|»»» description|body|string|true|Description that both the payer an recipient can see|
|»»» recipient_contact_id|body|string|true|Contact to pay (`Contact.data.id`)|
|»»» metadata|body|[Metadata](#schemametadata)|false|Use for your custom data and certain Split customisations. This data will be attached to the Payment Request and its associated Payouts, Transactions and Webhook Events.|
|»» metadata|body|[Metadata](#schemametadata)|false|Use for your custom data and certain Split customisations. This data will be attached to the Payment Request and its associated Payouts, Transactions and Webhook Events.|

#### Detailed descriptions

**»» Payout**: The actual Payout
<aside class="notice">[23/07/2018] Please note that we've deprecated <code>recipient_id</code> in favour of <code>recipient_contact_id</code>. We will continue to support the use of <code>recipient_id (Contact.data.bank_account.id)</code> but encourage you to use <code>recipient_contact_id (Contact.data.id)</code> to identify the recipient of the payout.</aside>

> Example responses

> 201 Response

```json
{
  "data": {
    "ref": "PB.1",
    "payouts": [
      {
        "ref": "D.1",
        "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44Z",
        "status": "maturing",
        "amount": 30000,
        "description": "A tandem skydive jump SB23094",
        "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
        "to_id": "21066764-c103-4e7f-b436-4cee7db5f400",
        "metadata": {
          "invoice_ref": "BILL-0001",
          "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
          "custom_key": "Custom string",
          "another_custom_key": "Maybe a URL"
        }
      },
      {
        "ref": "D.2",
        "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44Z",
        "status": "maturing",
        "amount": 30000,
        "description": "A scuba dive SDS5464",
        "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
        "to_id": "21066764-c103-4e7f-b436-4cee7db5f400"
      }
    ],
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Make a Payment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|201|[Created](https://tools.ietf.org/html/rfc7231#section-6.3.2)|Created|[MakeAPaymentResponse](#schemamakeapaymentresponse)|

## List all Payments

<a id="opIdListAllPayments"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/payments \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payments")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payments",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/payments", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/payments")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payments');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payments"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /payments`

<h3 id="List-all-Payments-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "PB.1",
      "payouts": [
        {
          "ref": "D.1",
          "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
          "batch_description": "This description is only available to the payer",
          "matures_at": "2016-09-13T23:50:44Z",
          "created_at": "2016-09-10T23:50:44Z",
          "status": "maturing",
          "amount": 30000,
          "description": "The recipient will see this description",
          "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
          "to_id": "21066764-c103-4e7f-b436-4cee7db5f400",
          "metadata": {
            "invoice_ref": "BILL-0001",
            "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
            "custom_key": "Custom string",
            "another_custom_key": "Maybe a URL"
          }
        },
        {
          "ref": "D.2",
          "recipient_contact_id": "dc6f1e60-3803-43ca-a200-7d641816f57f",
          "batch_description": "This description is only available to the payer",
          "matures_at": "2016-09-13T23:50:44Z",
          "created_at": "2016-09-10T23:50:44Z",
          "status": "maturing",
          "amount": 30000,
          "description": "The recipient will see this description",
          "from_id": "48b89364-1577-4c81-ba02-96705895d457",
          "to_id": "f989d9cd-87fc-4c73-b0a4-1eb0e8768d3b"
        }
      ],
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

<h3 id="List all Payments-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListAllPaymentsResponse](#schemalistallpaymentsresponse)|

## Get a Payment

<a id="opIdGetAPayment"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/payments/PB.1 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payments/PB.1")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payments/PB.1",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/payments/PB.1", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/payments/PB.1")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payments/PB.1');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payments/PB.1"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /payments/{payment_ref}`

Get a single payment by its reference

<h3 id="Get-a-Payment-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|payment_ref|path|string|true|Payment reference|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PB.1",
    "payouts": [
      {
        "ref": "D.1",
        "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44",
        "status": "maturing",
        "amount": 30000,
        "description": "A tandem skydive jump SB23094",
        "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
        "to_id": "21066764-c103-4e7f-b436-4cee7db5f400",
        "metadata": {
          "invoice_ref": "BILL-0001",
          "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
          "custom_key": "Custom string",
          "another_custom_key": "Maybe a URL"
        }
      },
      {
        "ref": "D.2",
        "recipient_contact_id": "dc6f1e60-3803-43ca-a200-7d641816f57f",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44Z",
        "status": "maturing",
        "amount": 30000,
        "description": "A scuba dive SDS5464",
        "from_id": "48b89364-1577-4c81-ba02-96705895d457",
        "to_id": "f989d9cd-87fc-4c73-b0a4-1eb0e8768d3b"
      }
    ],
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Get a Payment-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetAPaymentResponse](#schemagetapaymentresponse)|

<h1 id="Split-API-Payouts">Payouts</h1>

Payouts are what a Payment or Payment Request are made of and can be either a debit or a credit. One or all Payouts can be voided individually as part of the larger Payment or Payment Request.

## Retry a credit/debit Payout

<a id="opIdRetryAPayout"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/payouts/C.2/retry \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payouts/C.2/retry")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payouts/C.2/retry",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/payouts/C.2/retry", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/payouts/C.2/retry")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/payouts/C.2/retry');
$request->setRequestMethod('POST');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payouts/C.2/retry"

	req, _ := http.NewRequest("POST", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /payouts/{ref}/retry`

Split will prefail a debit and its associated credit transaction before ever sending it to the bank if we detect a high probability of insufficient funds.

This endpoint allows you to retry the payout without having to recreate the parent request. e.g A Payment or Payment Request.

<h3 id="Retry-a-credit/debit-Payout-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|path|string|true|Payout debit or credit reference|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "C.2",
    "parent_ref": "PR.039a",
    "type": "credit",
    "category": "payout",
    "created_at": "2016-12-05T23:15:00Z",
    "matures_at": "2016-12-06T23:15:00Z",
    "cleared_at": null,
    "bank_ref": null,
    "status": "maturing",
    "status_changed_at": "2016-12-05T23:15:00Z",
    "party_contact_id": "33c6e31d3-1dc1-448b-9512-0320bc44fdcf",
    "party_name": "Price and Sons",
    "party_nickname": "price-and-sons-2",
    "party_bank_ref": null,
    "description": "Money for jam",
    "amount": 1
  }
}
```

<h3 id="Retry a credit/debit Payout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[RetryPayoutResponse](#schemaretrypayoutresponse)|

## Void a Payout

<a id="opIdVoidAPayout"></a>

> Code samples

```shell
curl --request DELETE \
  --url https://api.sandbox.split.cash/payouts/D.1 \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"details":"Incorrect recipient"}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/payouts/D.1")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["content-type"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"details\":\"Incorrect recipient\"}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "DELETE",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/payouts/D.1",
  "headers": {
    "content-type": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({ details: 'Incorrect recipient' }));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"details\":\"Incorrect recipient\"}"

headers = {
    'content-type': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("DELETE", "/payouts/D.1", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.delete("https://api.sandbox.split.cash/payouts/D.1")
  .header("content-type", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"details\":\"Incorrect recipient\"}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"details":"Incorrect recipient"}');

$request->setRequestUrl('https://api.sandbox.split.cash/payouts/D.1');
$request->setRequestMethod('DELETE');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/payouts/D.1"

	payload := strings.NewReader("{\"details\":\"Incorrect recipient\"}")

	req, _ := http.NewRequest("DELETE", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`DELETE /payouts/{ref}`

You can void any Payout debit from your account that has not yet matured.

> Body parameter

```json
{
  "details": "Incorrect recipient"
}
```

<h3 id="Void-a-Payout-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|ref|path|string|true|Payout debit reference|
|body|body|[VoidAPayoutRequest](#schemavoidapayoutrequest)|false|No description|
|» details|body|string|false|Optional details about why the payout has been voided|

<h3 id="Void a Payout-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|

<h1 id="Split-API-Refunds">Refunds</h1>

Refunds can be issued for any successfully cleared Payout (credit) transactions.

<div class="middle-header">Direction</div>

Refunds are broken up by direction:

1. **Incoming:** An incoming Refund (you are the recipient of the refund)
2. **Outgoing:** An outgoing Refund (you are the issuer of the refund)

There are two response fields that differ depending on the direction:

| Field | Description |
|-------|-------------|
| `debit_ref` | Only visible to the Refund issuer (outgoing Refunds). This reference corresponds to the newly created debit to process the Refund. |
| `credit_ref` | Only visible to the Refund recipient (incoming Refunds). This reference corresponds to the newly created credit to process the Refund. |

## Issue a Refund

<a id="opIdIssueARefund"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/credits/string/refunds \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"amount":500,"reason":"Because reason","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/credits/string/refunds")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"amount\":500,\"reason\":\"Because reason\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/credits/string/refunds",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  amount: 500,
  reason: 'Because reason',
  metadata: { custom_key: 'Custom string', another_custom_key: 'Maybe a URL' }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"amount\":500,\"reason\":\"Because reason\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/credits/string/refunds", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/credits/string/refunds")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"amount\":500,\"reason\":\"Because reason\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"amount":500,"reason":"Because reason","metadata":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}');

$request->setRequestUrl('https://api.sandbox.split.cash/credits/string/refunds');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/credits/string/refunds"

	payload := strings.NewReader("{\"amount\":500,\"reason\":\"Because reason\",\"metadata\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /credits/{credit_ref}/refunds`

Certain rules apply to the issuance of a refund:

* Must be applied against a successfully cleared payout (credit)
* The refund amount may not exceed the original amount of the credit

> Body parameter

```json
{
  "amount": 500,
  "reason": "Because reason",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

<h3 id="Issue-a-Refund-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|credit_ref|path|string|true|No description|
|body|body|[IssueARefundRequest](#schemaissuearefundrequest)|true|No description|
|» amount|body|number|true|Amount in cents refund|
|» reason|body|string|false|Reason for the refund. Visible to both parties.|
|» metadata|body|[Metadata](#schemametadata)|false|Use for your custom data and certain Split customisations. This data will be attached to the Payment Request and its associated Payouts, Transactions and Webhook Events.|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PRF.1",
    "for_ref": "C.59",
    "debit_ref": "D.hi",
    "created_at": "2017-05-08T07:20:24Z",
    "amount": 500,
    "reason": "Because reason",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Issue a Refund-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Created|[IssueARefundResponse](#schemaissuearefundresponse)|

## List incoming Refunds

<a id="opIdListIncomingRefunds|Get"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/refunds/incoming \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/refunds/incoming")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/refunds/incoming",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/refunds/incoming", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/refunds/incoming")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/refunds/incoming');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/refunds/incoming"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /refunds/incoming`

<h3 id="List-incoming-Refunds-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "PRF.2",
      "for_ref": "D.5",
      "credit_ref": "C.q",
      "created_at": "2017-05-09T04:45:26Z",
      "amount": 5,
      "reason": "Because reason",
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

<h3 id="List incoming Refunds-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListIncomingRefundsResponse](#schemalistincomingrefundsresponse)|

## List outgoing Refunds

<a id="opIdListOutgoingRefunds"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/refunds/outgoing \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/refunds/outgoing")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/refunds/outgoing",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/refunds/outgoing", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/refunds/outgoing")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/refunds/outgoing');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/refunds/outgoing"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /refunds/outgoing`

<h3 id="List-outgoing-Refunds-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "PRF.2",
      "for_ref": "C.5",
      "debit_ref": "D.5a",
      "created_at": "2017-05-09T04:45:26Z",
      "amount": 5,
      "reason": "Because reason",
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

<h3 id="List outgoing Refunds-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListOutgoingRefundsResponse](#schemalistoutgoingrefundsresponse)|

## Retrieve a Refund

<a id="opIdRetrieveARefund"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/refunds/PRF.1 \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/refunds/PRF.1")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/refunds/PRF.1",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/refunds/PRF.1", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/refunds/PRF.1")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/refunds/PRF.1');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/refunds/PRF.1"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /refunds/{refund_ref}`

Get a single Refund by its reference

<h3 id="Retrieve-a-Refund-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|refund_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "PRF.1",
    "for_ref": "C.59",
    "debit_ref": "D.hi",
    "created_at": "2017-05-08T07:20:24Z",
    "amount": 500,
    "reason": "Because reason",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Retrieve a Refund-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[RetrieveARefundResponse](#schemaretrievearefundresponse)|

<h1 id="Split-API-Transactions">Transactions</h1>

The transactions endpoint provides a detailed look at all past, current and future scheduled debits & credits relating to the Split account. In other words, we not only show the transactions initiated by the Split account but also show transactions where the Split account is on the receiving end - even for payments that have not yet matured.

##Lifecycle

A transaction (debit or credit) can have the following statuses:

| Status | Description |
|--------|-------------|
| `maturing` | The maturation date has not yet been reached |
| `matured` | The maturation date has been reached and the transaction is eligible for processing. |
| `preprocessing` | The transaction is undergoing pre-checks before being sent to the bank. |
| `processing` | The transaction has been submitted to the bank. |
| `clearing` | Waiting for confirmation from the bank that the transaction has succeeded. |
| `cleared` | The transaction is complete. |
| `rejected` | The bank has rejected the transaction due to incorrect bank account details. |
| `returned` | The transaction did not successfully clear. |
| `voided` | The transaction has been cancelled and is no longer eligible for processing. |
| `pending_verification` | The bank account must be verified before the transaction can proceed. |
| `paused` | The transaction has temporary been paused by Split pending internal review. |
| `prefailed` | The transaction was never submitted to the bank because we detected that there were insufficient funds. The transaction can be retried. |

##Failure reasons

> Example response

```json
{
  "data": [
    {
      "ref": "D.3",
      "parent_ref": null,
      "type": "debit",
      "category": "payout_refund",
      "created_at": "2016-12-07T23:15:00Z",
      "matures_at": "2016-12-10T23:15:00Z",
      "cleared_at": null,
      "bank_ref": null,
      "status": "returned",
      "status_changed_at": "2016-12-08T23:15:00Z",
      "failure_reason": "user_voided",
      "failure_details": "Wrong amount - approved by Stacey"
      "party_contact_id": "26297f44-c5e1-40a1-9864-3e0b0754c32a",
      "party_name": "Sanford-Rees",
      "party_nickname": "sanford-rees-8",
      "description": null,
      "amount": 1
    }
  ]
}
```

The `rejected`, `returned`, `voided` & `prefailed` statuses are always accompanied by a `failure_reason`:

| Reason | Description |
|--------|-------------|
| `refer_to_customer` | Usually due to insufficient funds |
| `insufficient_funds` | Insufficient funds |
| `payment_stopped` | The payment was stopped at the bank. Can be due to a customer requesting a stop payment with their financial institution. |
| `invalid_bsb_number` | BSB number is invalid |
| `account_closed` | The bank account is closed |
| `customer_deceased` | Customer is deceased |
| `incorrect_account_number` | Account number is incorrect |
| `refer_to_split` | Failed due to reasons not listed here. Please contact us. |
| `user_voided` | Voided by payout initiator |
| `admin_voided` | Voided by Split Payments admin |

<aside class="notice">
  The <code>user_voided</code> and <code>admin_voided</code> <code>failure_reason</code>s can sometimes be accompanied by the <code>failure_details</code> key which includes user submitted comments relating to the <code>failure_reason</code>.
</aside>

## List all transactions

<a id="opIdListAllTransactions"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/transactions \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/transactions")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/transactions",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/transactions", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/transactions")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/transactions');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/transactions"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /transactions`

<h3 id="List-all-transactions-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|
|ref (debit or credit)|query|string|false|Single value, exact match|
|parent_ref|query|string|false|Single value, exact match|
|bank_ref|query|string|false|Single value, exact match|
|status|query|array[string]|false|Multiple values, exact match|
|category|query|array[string]|false|Multiple values, exact match|
|type|query|array[string]|false|Multiple values, exact match|
|other_party|query|string|false|Single value, string search|
|other_party_bank_ref|query|string|false|Single value, exact match|
|party_contact_id|query|string|false|Single value, exact match|
|description|query|string|false|Single value, string search|
|min_amount|query|integer|false|Cents, single value, exact match|
|max_amount|query|integer|false|Cents, single value, exact match|
|min_created_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|max_created_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|min_matured_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|max_matured_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|min_cleared_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|max_cleared_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|min_status_changed_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|
|max_status_changed_date|query|string(date-time)|false|Date/time UTC ISO 8601 format, single value, exact match|

#### Enumerated Values

|Parameter|Value|
|---|---|
|status|maturing|
|status|matured|
|status|preprocessing|
|status|processing|
|status|clearing|
|status|cleared|
|status|rejected|
|status|returned|
|status|voided|
|status|pending_verification|
|status|paused|
|category|payout|
|category|payout_refund|
|category|invoice|
|type|debit|
|type|credit|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "D.3",
      "parent_ref": null,
      "type": "debit",
      "category": "payout_refund",
      "created_at": "2016-12-07T23:15:00Z",
      "matures_at": "2016-12-10T23:15:00Z",
      "cleared_at": "2016-12-10T23:15:00Z",
      "bank_ref": "DT.9a",
      "status": "cleared",
      "status_changed_at": "2016-12-10T23:15:00Z",
      "party_contact_id": "31354923-b1e9-4d65-b03c-415ead89cbf3",
      "party_name": "Sanford-Rees",
      "party_nickname": null,
      "party_bank_ref": "CT.11",
      "description": null,
      "amount": 20000
    },
    {
      "ref": "D.2",
      "parent_ref": "PB.2",
      "type": "debit",
      "category": "payout",
      "created_at": "2016-12-06T23:15:00Z",
      "matures_at": "2016-12-09T23:15:00Z",
      "cleared_at": null,
      "bank_ref": null,
      "status": "maturing",
      "status_changed_at": "2016-12-06T23:15:00Z",
      "party_contact_id": "3c6e31d3-1dc1-448b-9512-0320bc44fdcf",
      "party_name": "Gutmann-Schmidt",
      "party_nickname": null,
      "party_bank_ref": null,
      "description": "Batteries for hire",
      "amount": 2949299
    },
    {
      "ref": "C.2",
      "parent_ref": "PB.s0z",
      "type": "credit",
      "category": "payout",
      "created_at": "2016-12-05T23:15:00Z",
      "matures_at": "2016-12-06T23:15:00Z",
      "cleared_at": "2016-12-09T23:15:00Z",
      "bank_ref": "CT.1",
      "status": "cleared",
      "status_changed_at": "2016-12-09T23:15:00Z",
      "party_contact_id": "33c6e31d3-1dc1-448b-9512-0320bc44fdcf",
      "party_name": "Price and Sons",
      "party_nickname": "price-and-sons-2",
      "party_bank_ref": null,
      "description": "Online purchase",
      "amount": 19999,
      "metadata": {
        "customer_id": "xur4492",
        "product_ref": "TSXL392110x"
      }
    }
  ]
}
```

<h3 id="List all transactions-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListAllTransactionsResponse](#schemalistalltransactionsresponse)|

<h1 id="Split-API-Unassigned-Agreements">Unassigned Agreements</h1>

An agreement with no preset authoriser that can only be accepted once and must be accepted within a predefined time period.

Unassigned Agreements are shared using the generated link available in the response body. You can then include it in an email, text message, embed it in an iFrame, etc...

Please refer to the [Unassigned Agreement](http://help.split.cash/agreements/unassigned-agreement) article in our knowledge base for more information.

## Propose an Unassigned Agreement

<a id="opIdProposeUnassignedAgreement"></a>

> Code samples

```shell
curl --request POST \
  --url https://api.sandbox.split.cash/unassigned_agreements \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}' \
  --header 'content-type: application/json' \
  --data '{"expiry_in_seconds":60,"single_use":false,"terms":{"per_payout":{"min_amount":null,"max_amount":10000},"per_frequency":{"days":7,"max_amount":1000000}},"metadata":{"type":"Metadata","description":"Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.","example":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/unassigned_agreements")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Post.new(url)
request["content-type"] = 'application/json'
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'
request.body = "{\"expiry_in_seconds\":60,\"single_use\":false,\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}},\"metadata\":{\"type\":\"Metadata\",\"description\":\"Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.\",\"example\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}}"

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "POST",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/unassigned_agreements",
  "headers": {
    "content-type": "application/json",
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.write(JSON.stringify({
  expiry_in_seconds: 60,
  single_use: false,
  terms: {
    per_payout: { min_amount: null, max_amount: 10000 },
    per_frequency: { days: 7, max_amount: 1000000 }
  },
  metadata: {
    type: 'Metadata',
    description: 'Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.',
    example: { custom_key: 'Custom string', another_custom_key: 'Maybe a URL' }
  }
}));
req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

payload = "{\"expiry_in_seconds\":60,\"single_use\":false,\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}},\"metadata\":{\"type\":\"Metadata\",\"description\":\"Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.\",\"example\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}}"

headers = {
    'content-type': "application/json",
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("POST", "/unassigned_agreements", payload, headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.post("https://api.sandbox.split.cash/unassigned_agreements")
  .header("content-type", "application/json")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .body("{\"expiry_in_seconds\":60,\"single_use\":false,\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}},\"metadata\":{\"type\":\"Metadata\",\"description\":\"Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.\",\"example\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$body = new http\Message\Body;
$body->append('{"expiry_in_seconds":60,"single_use":false,"terms":{"per_payout":{"min_amount":null,"max_amount":10000},"per_frequency":{"days":7,"max_amount":1000000}},"metadata":{"type":"Metadata","description":"Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.","example":{"custom_key":"Custom string","another_custom_key":"Maybe a URL"}}}');

$request->setRequestUrl('https://api.sandbox.split.cash/unassigned_agreements');
$request->setRequestMethod('POST');
$request->setBody($body);

$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json',
  'content-type' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"strings"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/unassigned_agreements"

	payload := strings.NewReader("{\"expiry_in_seconds\":60,\"single_use\":false,\"terms\":{\"per_payout\":{\"min_amount\":null,\"max_amount\":10000},\"per_frequency\":{\"days\":7,\"max_amount\":1000000}},\"metadata\":{\"type\":\"Metadata\",\"description\":\"Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.\",\"example\":{\"custom_key\":\"Custom string\",\"another_custom_key\":\"Maybe a URL\"}}}")

	req, _ := http.NewRequest("POST", url, payload)

	req.Header.Add("content-type", "application/json")
	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`POST /unassigned_agreements`

Create an Unassigned Agreement

<aside class="notice">You can set any of the term metrics to <code>null</code> if you wish them to not have a limit.</aside>

> Body parameter

```json
{
  "expiry_in_seconds": 60,
  "single_use": false,
  "terms": {
    "per_payout": {
      "min_amount": null,
      "max_amount": 10000
    },
    "per_frequency": {
      "days": 7,
      "max_amount": 1000000
    }
  },
  "metadata": {
    "type": "Metadata",
    "description": "Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.",
    "example": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Propose-an-Unassigned-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|body|body|[ProposeUnassignedAgreementRequest](#schemaproposeunassignedagreementrequest)|true|No description|
|» expiry_in_seconds|body|number|true|The amount of time in seconds before the Unassigned Agreement can no longer be accepted.|
|» single_use|body|boolean|false|Optionally propose a single use agreement. When the Unassigned Agreement is accepted and a Payment Request is approved according to the Agreement terms, the agreement will automatically become <code>expended</code>.<br><br>The proposed agreement must have equal max/min <code>per_payout</code> amounts and <code>null</code> <code>per_frequency</code> amounts.<br><br>Furthermore, we will automatically check that the authoriser's bank account has sufficient funds to honour the agreement terms.|
|» terms|body|[Terms](#schematerms)|true|Terms|
|»» per_payout|body|[PerPayout](#schemaperpayout)|true|No description|
|»»» min_amount|body|number|false|Minimum amount in cents a PR can be in order to be auto-approved|
|»»» max_amount|body|number|false|Maximum amount in cents a PR can be in order to be auto-approved|
|»» per_frequency|body|[PerFrequency](#schemaperfrequency)|true|No description|
|»»» days|body|number|false|Amount of days to apply against the frequency|
|»»» max_amount|body|number|false|Maximum amount in cents the total of all PRs can be for the duration of the frequency|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "A.4k",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "status": "proposed",
    "responded_at": null,
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "assignment_expires_at": "2017-03-20T00:54:27Z",
    "link": "https://go.sandbox.split.cash/unassigned_agreements/b61fc159-8779-4a17-a826-e398e3e7e211/invitation",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

<h3 id="Propose an Unassigned Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Created|[ProposeUnassignedAgreementResponse](#schemaproposeunassignedagreementresponse)|

## List all Unassigned Agreements

<a id="opIdGetUnassignedAgreements"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/unassigned_agreements \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/unassigned_agreements")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/unassigned_agreements",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/unassigned_agreements", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/unassigned_agreements")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/unassigned_agreements');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/unassigned_agreements"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /unassigned_agreements`

Will return all Unassigned Agreements that have not yet be accepted.

<h3 id="List-all-Unassigned-Agreements-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|page|query|string|false|Page of results to return, single value, exact match|
|per_page|query|string|false|Number of results per page, single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": [
    {
      "ref": "A.4k",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "status": "proposed",
      "responded_at": null,
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": null
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      },
      "assignment_expires_at": "2017-03-20T00:54:27Z",
      "link": "https://go.sandbox.split.cash/unassigned_agreements/b61fc159-8779-4a17-a826-e398e3e7e211/invitation"
    },
    {
      "ref": "A.7ea",
      "initiator_id": "b61fc159-8779-4a17-a826-e398e3e7e211",
      "status": "proposed",
      "responded_at": null,
      "created_at": "2017-03-21T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": null,
          "min_amount": null
        },
        "per_frequency": {
          "days": null,
          "max_amount": null
        }
      },
      "assignment_expires_at": "2017-03-21T00:54:27Z",
      "link": "https://go.sandbox.split.cash/unassigned_agreements/4e2728cc-b4ba-42c2-a6c3-26a7758de58d/invitation"
    }
  ]
}
```

<h3 id="List all Unassigned Agreements-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[ListUnassignedAgreementsResponse](#schemalistunassignedagreementsresponse)|

## Get an Unassigned Agreement

<a id="opIdGetAgreement"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/unassigned_agreements/A.4k \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/unassigned_agreements/A.4k")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/unassigned_agreements/A.4k",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/unassigned_agreements/A.4k", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/unassigned_agreements/A.4k")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/unassigned_agreements/A.4k');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/unassigned_agreements/A.4k"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /unassigned_agreements/{unassigned_agreement_ref}`

Get a single Unassigned Agreement by its reference

<h3 id="Get-an-Unassigned-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|unassigned_agreement_ref|path|string|true|Single value, exact match|

> Example responses

> 200 Response

```json
{
  "data": {
    "ref": "A.4k",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "status": "proposed",
    "responded_at": null,
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "assignment_expires_at": "2017-03-20T00:54:27Z",
    "link": "https://go.sandbox.split.cash/unassigned_agreements/b61fc159-8779-4a17-a826-e398e3e7e211/invitation"
  }
}
```

<h3 id="Get an Unassigned Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetUnassignedAgreementResponse](#schemagetunassignedagreementresponse)|

## Delete an Unassigned Agreement

<a id="opIdDeleteUnassignedAgreement"></a>

> Code samples

```shell
curl --request DELETE \
  --url https://api.sandbox.split.cash/unassigned_agreements/A.2 \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/unassigned_agreements/A.2")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Delete.new(url)
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "DELETE",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/unassigned_agreements/A.2",
  "headers": {
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = { 'authorization': "Bearer {access-token}" }

conn.request("DELETE", "/unassigned_agreements/A.2", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.delete("https://api.sandbox.split.cash/unassigned_agreements/A.2")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/unassigned_agreements/A.2');
$request->setRequestMethod('DELETE');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/unassigned_agreements/A.2"

	req, _ := http.NewRequest("DELETE", url, nil)

	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`DELETE /unassigned_agreements/{unassigned_agreement_ref}`

An Unassigned Agreement can be deleted at anytime as long as it has not yet been assigned an authoriser.

<h3 id="Delete-an-Unassigned-Agreement-parameters" class="parameters">Parameters</h3>

|Parameter|In|Type|Required|Description|
|---|---|---|---|---|
|unassigned_agreement_ref|path|string|true|Single value, exact match|

<h3 id="Delete an Unassigned Agreement-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|204|[No Content](https://tools.ietf.org/html/rfc7231#section-6.3.5)|No Content|None|

<h1 id="Split-API-Users">Users</h1>

All about the currently authenticated user.

## Get user details

<a id="opIdGetUserDetails"></a>

> Code samples

```shell
curl --request GET \
  --url https://api.sandbox.split.cash/user \
  --header 'accept: application/json' \
  --header 'authorization: Bearer {access-token}'
```

```ruby
require 'uri'
require 'net/http'

url = URI("https://api.sandbox.split.cash/user")

http = Net::HTTP.new(url.host, url.port)
http.use_ssl = true
http.verify_mode = OpenSSL::SSL::VERIFY_NONE

request = Net::HTTP::Get.new(url)
request["accept"] = 'application/json'
request["authorization"] = 'Bearer {access-token}'

response = http.request(request)
puts response.read_body
```

```javascript--node
var http = require("https");

var options = {
  "method": "GET",
  "hostname": "api.sandbox.split.cash",
  "port": null,
  "path": "/user",
  "headers": {
    "accept": "application/json",
    "authorization": "Bearer {access-token}"
  }
};

var req = http.request(options, function (res) {
  var chunks = [];

  res.on("data", function (chunk) {
    chunks.push(chunk);
  });

  res.on("end", function () {
    var body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```

```python
import http.client

conn = http.client.HTTPSConnection("api.sandbox.split.cash")

headers = {
    'accept': "application/json",
    'authorization': "Bearer {access-token}"
    }

conn.request("GET", "/user", headers=headers)

res = conn.getresponse()
data = res.read()

print(data.decode("utf-8"))
```

```java
HttpResponse<String> response = Unirest.get("https://api.sandbox.split.cash/user")
  .header("accept", "application/json")
  .header("authorization", "Bearer {access-token}")
  .asString();
```

```php
<?php

$client = new http\Client;
$request = new http\Client\Request;

$request->setRequestUrl('https://api.sandbox.split.cash/user');
$request->setRequestMethod('GET');
$request->setHeaders(array(
  'authorization' => 'Bearer {access-token}',
  'accept' => 'application/json'
));

$client->enqueue($request)->send();
$response = $client->getResponse();

echo $response->getBody();
```

```go
package main

import (
	"fmt"
	"net/http"
	"io/ioutil"
)

func main() {

	url := "https://api.sandbox.split.cash/user"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("authorization", "Bearer {access-token}")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := ioutil.ReadAll(res.Body)

	fmt.Println(res)
	fmt.Println(string(body))

}
```

`GET /user`

> Example responses

> 200 Response

```json
{
  "data": {
    "first_name": "Bear",
    "last_name": "Dog",
    "mobile_phone": "0456945832",
    "email": "bear@dog.com",
    "account": {
      "name": "Dog Bones Inc",
      "nickname": "dog-bones-inc",
      "abn": "129959040",
      "phone": "0418495033",
      "street_address": "98 Acme Avenue",
      "suburb": "Lead",
      "state": "NSW",
      "postcode": "2478"
    }
  }
}
```

<h3 id="Get user details-responses">Responses</h3>

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|OK|[GetUserDetailsResponse](#schemagetuserdetailsresponse)|

# Schemas

## ProposeAgreementRequest

<a id="schemaproposeagreementrequest"></a>

```json
{
  "authoriser_contact_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
  "terms": {
    "per_payout": {
      "min_amount": null,
      "max_amount": 10000
    },
    "per_frequency": {
      "days": 7,
      "max_amount": 1000000
    }
  }
}
```

### Properties

*Propose an Agreement (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|authoriser_contact_id|string|true|The Authoriser's contact ID (`Contact.data.id`)|
|terms|[Terms](#schematerms)|true|No description|

## Terms

<a id="schematerms"></a>

```json
{
  "per_payout": {
    "min_amount": 0,
    "max_amount": 10000
  },
  "per_frequency": {
    "days": 7,
    "max_amount": 1000000
  }
}
```

### Properties

*Agreement terms*

|Name|Type|Required|Description|
|---|---|---|---|
|per_payout|[PerPayout](#schemaperpayout)|true|No description|
|per_frequency|[PerFrequency](#schemaperfrequency)|true|No description|

## PerPayout

<a id="schemaperpayout"></a>

```json
{
  "min_amount": 0,
  "max_amount": 10000
}
```

### Properties

*Per payout terms*

|Name|Type|Required|Description|
|---|---|---|---|
|min_amount|number|false|Minimum amount in cents a PR can be in order to be auto-approved|
|max_amount|number|false|Maximum amount in cents a PR can be in order to be auto-approved|

## PerFrequency

<a id="schemaperfrequency"></a>

```json
{
  "days": 7,
  "max_amount": 1000000
}
```

### Properties

*Per frequency terms*

|Name|Type|Required|Description|
|---|---|---|---|
|days|number|false|Amount of days to apply against the frequency|
|max_amount|number|false|Maximum amount in cents the total of all PRs can be for the duration of the frequency|

## ProposeAgreementResponse

<a id="schemaproposeagreementresponse"></a>

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "contact_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "proposed",
    "status_reason": null,
    "responded_at": null,
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

### Properties

*Propose an Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ApproveAgreementResponse

<a id="schemaapproveagreementresponse"></a>

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "accepted",
    "status_reason": null,
    "responded_at": "2017-03-20T02:13:11Z",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": 1
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

### Properties

*Approve an Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## DeclineAgreementResponse

<a id="schemadeclineagreementresponse"></a>

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "declined",
    "status_reason": null,
    "responded_at": "2017-03-20T02:13:11Z",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": 1
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

### Properties

*Decline an Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## GetAgreementResponse

<a id="schemagetagreementresponse"></a>

```json
{
  "data": {
    "ref": "A.2",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
    "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
    "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
    "status": "approved",
    "status_reason": null,
    "responded_at": "2017-03-20T02:13:11Z",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": 1
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    }
  }
}
```

### Properties

*Get an Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListIncomingAgreementsResponse

<a id="schemalistincomingagreementsresponse"></a>

```json
{
  "data": [
    {
      "ref": "A.2",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
      "contact_id": "0d290763-bd5a-4b4d-a8ce-06c64c4a697b",
      "bank_account_id": "fb9381ec-22af-47fd-8998-804f947aaca3",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": 1
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      }
    },
    {
      "ref": "A.1",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "56df206a-aaff-471a-b075-11882bc8906a",
      "contact_id": "a80ac411-c8fb-45c0-9557-607c54649907",
      "bank_account_id": "fa80ac411-c8fb-45c0-9557-607c54649907",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-16T22:51:48Z",
      "terms": {
        "per_payout": {
          "max_amount": 5000,
          "min_amount": 0
        },
        "per_frequency": {
          "days": "1",
          "max_amount": 10000
        }
      }
    }
  ]
}
```

### Properties

*List incoming Agreements (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ListOutgoingAgreementsResponse

<a id="schemalistoutgoingagreementsresponse"></a>

```json
{
  "data": [
    {
      "ref": "A.4",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "8df89c16-330f-462b-8891-808d7bdceb7f",
      "contact_id": "a80ac411-c8fb-45c0-9557-607c54649907",
      "bank_account_id": "fa80ac411-c8fb-45c0-9557-607c54649907",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": 1
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      }
    },
    {
      "ref": "A.3",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "authoriser_id": "56df206a-aaff-471a-b075-11882bc8906a",
      "contact_id": "a80ac411-c8fb-45c0-9557-607c54649907",
      "bank_account_id": "fa80ac411-c8fb-45c0-9557-607c54649907",
      "status": "proposed",
      "status_reason": null,
      "responded_at": null,
      "created_at": "2017-03-16T22:51:48Z",
      "terms": {
        "per_payout": {
          "max_amount": 5000,
          "min_amount": 0
        },
        "per_frequency": {
          "days": "1",
          "max_amount": 10000
        }
      }
    }
  ]
}
```

### Properties

*List outgoing Agreements (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ListAllBankAccountsResponse

<a id="schemalistallbankaccountsresponse"></a>

```json
{
  "data": [
    {
      "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
      "branch_code": "493192",
      "bank_name": "National Australia Bank",
      "account_number": "3993013",
      "status": "active"
    },
    {
      "id": "56df206a-aaff-471a-b075-11882bc8906a",
      "branch_code": "302193",
      "bank_name": "National Australia Bank",
      "account_number": "119302",
      "status": "active"
    }
  ]
}
```

### Properties

*List all Bank Accounts (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ListAllBankConnectionsResponse

<a id="schemalistallbankconnectionsresponse"></a>

```json
{
  "data": [
    {
      "id": "fb497754-87e2-407d-871f-32aec39d09d4",
      "provider_name": "split",
      "state": "active",
      "refreshed_at": "2020-02-13T20:05:00.000+11:00",
      "removed_at": null,
      "failure_reason": null,
      "institution": {
        "short_name": "CBA",
        "full_name": "Commonwealth Bank of Australia"
      },
      "contact": {
        "id": "626e15b1-aa4a-496e-b5d6-3f8c1a6d2189",
        "name": "George Morissette",
        "email": "randy@windler.net"
      },
      "links": {
        "update_bank_connection": "http://go.split.cash/authorise_bank_connections/williams-and-sons-5/fb497754-87e2-407d-871f-32aec39d09d4"
      }
    },
    {
      "id": "ddbee875-7344-4d74-9503-6233149fe1a0",
      "provider_name": "split",
      "state": "credentials_invalid",
      "refreshed_at": "2020-02-13T20:05:00.000+11:00",
      "removed_at": null,
      "failure_reason": null,
      "institution": {
        "short_name": "CBA",
        "full_name": "Commonwealth Bank of Australia"
      },
      "contact": {
        "id": "72e37667-6364-440f-b1bd-56df5654e258",
        "name": "Joel Boyle",
        "email": "travis@hermanntorp.net"
      },
      "links": {
        "update_bank_connection": "http://go.split.cash/authorise_bank_connections/williams-and-sons-5/ddbee875-7344-4d74-9503-6233149fe1a0"
      }
    }
  ]
}
```

### Properties

*List all Bank Connections (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## CreateOpenAgreementRequest

<a id="schemacreateopenagreementrequest"></a>

```json
{
  "title": "Subscription Plan A",
  "terms": {
    "per_payout": {
      "min_amount": null,
      "max_amount": 10000
    },
    "per_frequency": {
      "days": 7,
      "max_amount": 1000000
    }
  }
}
```

### Properties

*Create an Open Agreement (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|title|string|true|Title of the Open Agreement (Visible to authorisers)|
|terms|[Terms](#schematerms)|true|No description|

## CreateOpenAgreementResponse

<a id="schemacreateopenagreementresponse"></a>

```json
{
  "data": {
    "ref": "OA.1",
    "title": "Subscription Plan A",
    "status": "active",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "min_amount": null,
        "max_amount": 10000
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "link": "https://go.sandbox.split.cash/open_agreements/aecff76d-13e0-4e17-b7c5-844ae07fa675/agreements/invitation"
  }
}
```

### Properties

*Create an Open Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListAllOpenAgreementsResponse

<a id="schemalistallopenagreementsresponse"></a>

```json
{
  "data": [
    {
      "ref": "OA.1",
      "title": "Subscription Plan A",
      "status": "active",
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": null
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      },
      "link": "https://go.sandbox.split.cash/open_agreements/2e002c0b-ffaf-44eb-a35c-15fa384ddde1/agreements/invitation"
    },
    {
      "ref": "OA.2",
      "title": "Subscription Plan B",
      "status": "closed",
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": null
        },
        "per_frequency": {
          "days": null,
          "max_amount": null
        }
      },
      "link": "https://go.sandbox.split.cash/open_agreements/948e3662-154d-42d1-bdc9-a05c251d3981/agreements/invitation"
    }
  ]
}
```

### Properties

*List all Open Agreements (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ActivateOpenAgreementResponse

<a id="schemaactivateopenagreementresponse"></a>

```json
{
  "data": {
    "ref": "OA.1",
    "title": "Subscription Plan A",
    "status": "active",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "link": "https://go.sandbox.split.cash/open_agreements/2e002c0b-ffaf-44eb-a35c-15fa384ddde1/agreements/invitation"
  }
}
```

### Properties

*Activate Open Agreement Request (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## CloseOpenAgreementResponse

<a id="schemacloseopenagreementresponse"></a>

```json
{
  "data": {
    "ref": "OA.1",
    "title": "Subscription Plan A",
    "status": "closed",
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "link": "https://go.sandbox.split.cash/open_agreements/2e002c0b-ffaf-44eb-a35c-15fa384ddde1/agreements/invitation"
  }
}
```

### Properties

*Close Open Agreement Request (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## AddASplitContactRequest

<a id="schemaaddasplitcontactrequest"></a>

```json
{
  "nickname": "outstanding_tours",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

### Properties

*Add a Split Contact (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|nickname|string|true|Split account nickname|
|metadata|[Metadata](#schemametadata)|false|No description|

## AddASplitContactResponse

<a id="schemaaddasplitcontactresponse"></a>

```json
{
  "data": {
    "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
    "name": "Outstanding Tours Pty Ltd",
    "email": "accounts@outstandingtours.com.au",
    "type": "Split account",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    },
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "947434694",
      "branch_code": "304304",
      "bank_name": "National Australia Bank",
      "state": "active",
      "iav_provider": null,
      "iav_status": null,
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "account": {
      "id": "77be6ecc-5fa7-454b-86d6-02a5f147878d",
      "nickname": "outstanding_tours",
      "abn": "123456789",
      "name": "Outstanding Tours Pty Ltd"
    },
    "links": {
      "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

### Properties

*Add a Split Contact (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListAllContactsResponse

<a id="schemalistallcontactsresponse"></a>

```json
{
  "data": [
    {
      "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
      "name": "Outstanding Tours Pty Ltd",
      "email": "accounts@outstandingtours.com.au",
      "type": "Split account",
      "bank_account": {
        "id": "095c5ab7-7fa8-40fd-b317-cddbbf4c8fbc",
        "account_number": "494307",
        "branch_code": "435434",
        "bank_name": "National Australia Bank",
        "state": "active",
        "iav_provider": "split",
        "iav_status": "active",
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "bank_connection": {
        "id": "c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"
      }
    },
    {
      "id": "49935c67-c5df-4f00-99f4-1413c18a89a0",
      "name": "Adventure Dudes Pty Ltd",
      "email": "accounts@adventuredudes.com.au",
      "type": "Split account",
      "bank_account": {
        "id": "861ff8e4-7acf-4897-9e53-e7c5ae5f7cc0",
        "account_number": "4395959",
        "branch_code": "068231",
        "bank_name": "National Australia Bank",
        "state": "active",
        "iav_provider": "split",
        "iav_status": "credentials_invalid",
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "bank_connection": {
        "id": "c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"
      }
    },
    {
      "id": "eb3266f9-e172-4b6c-b802-fe5ac4d3250a",
      "name": "Surfing World Pty Ltd",
      "email": "accounts@surfingworld.com.au",
      "type": "Split account",
      "bank_account": {
        "id": null,
        "account_number": null,
        "branch_code": null,
        "bank_name": null,
        "state": "disabled",
        "iav_provider": null,
        "iav_status": null,
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "links": {
        "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
      }
    },
    {
      "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
      "name": "Hunter Thompson",
      "email": "hunter@batcountry.com",
      "type": "anyone",
      "bank_account": {
        "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
        "account_number": "13048322",
        "branch_code": "123456",
        "bank_name": "National Australia Bank",
        "state": "pending_verification",
        "iav_provider": null,
        "iav_status": null,
        "blocks": {
          "debits_blocked": false,
          "credits_blocked": false
        }
      },
      "links": {
        "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
      }
    }
  ]
}
```

### Properties

*List all Contacts (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## AddAnAnyoneContactRequest

<a id="schemaaddananyonecontactrequest"></a>

```json
{
  "name": "Hunter Thompson",
  "email": "hunter@batcountry.com",
  "branch_code": "123456",
  "account_number": "13048322",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

### Properties

*Add an Anyone Contact (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|name|string|true|The name of the Contact|
|email|string|true|The email of the Contact|
|branch_code|string|true|The bank account BSB of the Contact|
|account_number|string|true|The bank account number of the Contact|
|metadata|[Metadata](#schemametadata)|false|No description|

## AddAnAnyoneContactResponse

<a id="schemaaddananyonecontactresponse"></a>

```json
{
  "data": {
    "id": "6a7ed958-f1e8-42dc-8c02-3901d7057357",
    "name": "Hunter Thompson",
    "email": "hunter@batcountry.com",
    "type": "anyone",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    },
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "13048322",
      "branch_code": "123456",
      "bank_name": "National Australia Bank",
      "state": "pending_verification",
      "iav_provider": null,
      "iav_status": null,
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "links": {
      "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

### Properties

*Add an Anyone Contact (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## GetABankConnectionResponse

<a id="schemagetabankconnectionresponse"></a>

```json
{
  "data": {
    "id": "c397645b-bd4f-4fc6-b1fe-4993fef6c3c7",
    "provider_name": "split",
    "state": "credentials_invalid",
    "refreshed_at": "2020-02-13T20:01:00.000+11:00",
    "removed_at": null,
    "failure_reason": null,
    "institution": {
      "short_name": "CBA",
      "full_name": "Commonwealth Bank of Australia"
    },
    "contact": {
      "id": "72e37667-6364-440f-b1bd-56df5654e258",
      "name": "Joel Boyle",
      "email": "travis@hermanntorp.net"
    },
    "links": {
      "update_bank_connection": "https://go.sandbox.split.cash/authorise_bank_connections/thomas-morgan-1/c397645b-bd4f-4fc6-b1fe-4993fef6c3c7"
    }
  }
}
```

### Properties

*Get a BankConnection (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## GetAContactResponse

<a id="schemagetacontactresponse"></a>

```json
{
  "data": {
    "id": "fcabeacb-2ef6-4b27-ba19-4f6fa0d57dcb",
    "name": "Outstanding Tours Pty Ltd",
    "email": "accounts@outstandingtours.com.au",
    "type": "Split account",
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "947434694",
      "branch_code": "304304",
      "bank_name": "National Australia Bank",
      "state": "active",
      "iav_provider": null,
      "iav_status": null,
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "links": {
      "add_bank_connection": "https://go.sandbox.split.cash/invite_contact/thomas-morgan-1/1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

### Properties

*Get a Contact (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## UpdateAContactRequest

<a id="schemaupdateacontactrequest"></a>

```json
{
  "name": "My very own alias",
  "email": "updated@email.com"
}
```

### Properties

*Update a Contact (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|name|string|false|The name of the Contact|
|email|string|false|The email of the Contact|

## UpdateAContactResponse

<a id="schemaupdateacontactresponse"></a>

```json
{
  "data": {
    "id": "fcabeacb-2ef6-4b27-ba19-4f6fa0d57dcb",
    "name": "My very own alias",
    "email": "accounts@outstandingtours.com.au",
    "type": "Split account",
    "bank_account": {
      "id": "55afddde-4296-4daf-8e49-7ba481ef9608",
      "account_number": "947434694",
      "branch_code": "304304",
      "bank_name": "National Australia Bank",
      "state": "active",
      "iav_provider": "split",
      "iav_status": "active",
      "blocks": {
        "debits_blocked": false,
        "credits_blocked": false
      }
    },
    "bank_connection": {
      "id": "1030bfef-cef5-4938-b10b-5841cafafc80"
    }
  }
}
```

### Properties

*Update a Contact (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## MakeAPaymentRequest

<a id="schemamakeapaymentrequest"></a>

```json
{
  "description": "The SuperPackage",
  "matures_at": "2016-09-13T00:00:00Z",
  "payouts": [
    {
      "amount": 30000,
      "description": "A tandem skydive jump SB23094",
      "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
      "metadata": {
        "invoice_ref": "BILL-0001",
        "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    },
    {
      "amount": 30000,
      "description": "A scuba dive SDS5464",
      "recipient_contact_id": "dc6f1e60-3803-43ca-a200-7d641816f57f"
    }
  ],
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

### Properties

*Make a Payment (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|description|string|true|User description. Only visible to the payer|
|matures_at|string|true|Date & time in UTC ISO8601 the Payment should be processed. (Can not be earlier than the start of current day)|
|payouts|[[Payout](#schemapayout)]|true|One or many Payouts|
|metadata|[Metadata](#schemametadata)|false|No description|

## Payout

<a id="schemapayout"></a>

```json
{
  "amount": 30000,
  "description": "A tandem skydive jump SB23094",
  "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

### Properties

*Payout*

|Name|Type|Required|Description|
|---|---|---|---|
|amount|number|true|Amount in cents to pay the recipient|
|description|string|true|Description that both the payer an recipient can see|
|recipient_contact_id|string|true|Contact to pay (`Contact.data.id`)|
|metadata|[Metadata](#schemametadata)|false|No description|

## VoidAPayoutRequest

<a id="schemavoidapayoutrequest"></a>

```json
{
  "details": "Incorrect recipient"
}
```

### Properties

*Void a Payout (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|details|string|false|Optional details about why the payout has been voided|

## Metadata

<a id="schemametadata"></a>

```json
{
  "custom_key": "Custom string",
  "another_custom_key": "Maybe a URL"
}
```

### Properties

*Metadata*

*None*

## MakeAPaymentResponse

<a id="schemamakeapaymentresponse"></a>

```json
{
  "data": {
    "ref": "PB.1",
    "payouts": [
      {
        "ref": "D.1",
        "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44Z",
        "status": "maturing",
        "amount": 30000,
        "description": "A tandem skydive jump SB23094",
        "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
        "to_id": "21066764-c103-4e7f-b436-4cee7db5f400",
        "metadata": {
          "invoice_ref": "BILL-0001",
          "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
          "custom_key": "Custom string",
          "another_custom_key": "Maybe a URL"
        }
      },
      {
        "ref": "D.2",
        "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44Z",
        "status": "maturing",
        "amount": 30000,
        "description": "A scuba dive SDS5464",
        "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
        "to_id": "21066764-c103-4e7f-b436-4cee7db5f400"
      }
    ],
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Make a Payment (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ApprovePaymentRequestResponse

<a id="schemaapprovepaymentrequestresponse"></a>

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "d194c54b-9183-410c-966b-50485c5ce3f0",
    "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
    "schedule_ref": null,
    "status": "approved",
    "responded_at": "2016-12-19T02:38:04Z",
    "created_at": "2016-12-19T02:10:56Z",
    "debit_ref": "D.b",
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Approve Payment Request (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## DeclinePaymentRequestResponse

<a id="schemadeclinepaymentrequestresponse"></a>

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "d194c54b-9183-410c-966b-50485c5ce3f0",
    "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
    "schedule_ref": null,
    "status": "declined",
    "responded_at": "2016-12-19T02:38:04Z",
    "created_at": "2016-12-19T02:10:56Z",
    "debit_ref": null,
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Decline a Payment Request (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListAllPaymentsResponse

<a id="schemalistallpaymentsresponse"></a>

```json
{
  "data": [
    {
      "ref": "PB.1",
      "payouts": [
        {
          "ref": "D.1",
          "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
          "batch_description": "This description is only available to the payer",
          "matures_at": "2016-09-13T23:50:44Z",
          "created_at": "2016-09-10T23:50:44Z",
          "status": "maturing",
          "amount": 30000,
          "description": "The recipient will see this description",
          "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
          "to_id": "21066764-c103-4e7f-b436-4cee7db5f400",
          "metadata": {
            "invoice_ref": "BILL-0001",
            "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
            "custom_key": "Custom string",
            "another_custom_key": "Maybe a URL"
          }
        },
        {
          "ref": "D.2",
          "recipient_contact_id": "dc6f1e60-3803-43ca-a200-7d641816f57f",
          "batch_description": "This description is only available to the payer",
          "matures_at": "2016-09-13T23:50:44Z",
          "created_at": "2016-09-10T23:50:44Z",
          "status": "maturing",
          "amount": 30000,
          "description": "The recipient will see this description",
          "from_id": "48b89364-1577-4c81-ba02-96705895d457",
          "to_id": "f989d9cd-87fc-4c73-b0a4-1eb0e8768d3b"
        }
      ],
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

### Properties

*List all Payments (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## GetAPaymentResponse

<a id="schemagetapaymentresponse"></a>

```json
{
  "data": {
    "ref": "PB.1",
    "payouts": [
      {
        "ref": "D.1",
        "recipient_contact_id": "48b89364-1577-4c81-ba02-96705895d457",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44",
        "status": "maturing",
        "amount": 30000,
        "description": "A tandem skydive jump SB23094",
        "from_id": "83623359-e86e-440c-9780-432a3bc3626f",
        "to_id": "21066764-c103-4e7f-b436-4cee7db5f400",
        "metadata": {
          "invoice_ref": "BILL-0001",
          "invoice_id": "c80a9958-e805-47c0-ac2a-c947d7fd778d",
          "custom_key": "Custom string",
          "another_custom_key": "Maybe a URL"
        }
      },
      {
        "ref": "D.2",
        "recipient_contact_id": "dc6f1e60-3803-43ca-a200-7d641816f57f",
        "batch_description": "The SuperPackage",
        "matures_at": "2016-09-13T23:50:44Z",
        "created_at": "2016-09-10T23:50:44Z",
        "status": "maturing",
        "amount": 30000,
        "description": "A scuba dive SDS5464",
        "from_id": "48b89364-1577-4c81-ba02-96705895d457",
        "to_id": "f989d9cd-87fc-4c73-b0a4-1eb0e8768d3b"
      }
    ],
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Get a Payment (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## MakeAPaymentRequestRequest

<a id="schemamakeapaymentrequestrequest"></a>

```json
{
  "description": "Visible to both initiator and authoriser",
  "matures_at": "2016-12-19T02:10:56Z",
  "amount": 99000,
  "authoriser_contact_id": "de86472c-c027-4735-a6a7-234366a27fc7",
  "precheck_funds": "false",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

### Properties

*Make a Payment Request (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|description|string|true|Description visible to the initiator (payee) & authoriser (payer)|
|matures_at|string|true|Date & time in UTC ISO8601 that the Payment will be processed if the request is approved. (If the request is approved after this point in time, it will be processed straight away)|
|amount|number|true|Amount in cents to pay the initiator|
|authoriser_contact_id|string|true|The Contact the payment will be requested from (`Contact.data.id`)'|
|precheck_funds|boolean|false|Enforce prechecking of available funds before approving the Payment Request. see [Payment Request - Precheck Funds](/#precheck-funds-lifecycle)|
|metadata|[Metadata](#schemametadata)|false|No description|

## MakeAPaymentRequestResponse

<a id="schemamakeapaymentrequestresponse"></a>

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "970e4526-67d9-4ed9-b554-f5cf390ab775",
    "authoriser_contact_id": "de86472c-c027-4735-a6a7-234366a27fc7",
    "schedule_ref": null,
    "status": "pending_approval",
    "responded_at": null,
    "created_at": "2016-12-19T02:10:56Z",
    "credit_ref": null,
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Make a Payment Request (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## MakeAPaymentRequestWithNoAgreementResponse

<a id="schemamakeapaymentrequestwithnoagreementresponse"></a>

```json
{
  "errors": "Authoriser contact (de86472c-c027-4735-a6a7-234366a27fc7) is not a Split account holder and therefore must have a valid agreement in place before a Payment Request can be issued."
}
```

### Properties

*Make a Payment Request to an Anyone Contact with no valid Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|errors|string|true|No description|

## GetAPaymentRequestResponse

<a id="schemagetapaymentrequestresponse"></a>

```json
{
  "data": {
    "ref": "PR.3",
    "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
    "authoriser_id": "970e4526-67d9-4ed9-b554-f5cf390ab775",
    "authoriser_contact_id": "de86472c-c027-4735-a6a7-234366a27fc7",
    "schedule_ref": null,
    "status": "approved",
    "responded_at": "2016-12-19T02:38:04Z",
    "created_at": "2016-12-19T02:10:56Z",
    "credit_ref": "C.b",
    "payout": {
      "amount": 99000,
      "description": "The elite package for 4",
      "matures_at": "2016-12-25T00:00:00Z"
    },
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Get a Payment Request (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListIncomingPaymentRequestsResponse

<a id="schemalistincomingpaymentrequestsresponse"></a>

```json
{
  "data": [
    {
      "ref": "PR.2",
      "initiator_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "schedule_ref": "PRS.1",
      "status": "approved",
      "responded_at": "2016-12-19T02:10:18Z",
      "created_at": "2016-12-19T02:09:09Z",
      "debit_ref": "D.a",
      "payout": {
        "amount": 30000,
        "description": "The SuperPackage",
        "matures_at": "2016-12-20T00:00:00Z"
      }
    },
    {
      "ref": "PR.3",
      "initiator_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "schedule_ref": null,
      "status": "pending_approval",
      "responded_at": null,
      "created_at": "2016-12-19T02:10:56Z",
      "debit_ref": null,
      "payout": {
        "amount": 99000,
        "description": "The elite package for 4",
        "matures_at": "2016-12-25T00:00:00Z"
      },
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

### Properties

*List incoming Payment Requests (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ListOutgoingPaymentRequestsResponse

<a id="schemalistoutgoingpaymentrequestsresponse"></a>

```json
{
  "data": [
    {
      "ref": "PR.4",
      "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "schedule_ref": "PRS.2",
      "status": "approved",
      "responded_at": "2016-12-19T02:10:18Z",
      "created_at": "2016-12-19T02:09:09Z",
      "credit_ref": "C.a",
      "payout": {
        "amount": 30000,
        "description": "The SuperPackage",
        "matures_at": "2016-12-20T00:00:00Z"
      }
    },
    {
      "ref": "PR.5",
      "initiator_id": "ca7bc5b3-e47f-4153-96fb-bbe326b42772",
      "authoriser_id": "de86472c-c027-4735-a6a7-234366a27fc7",
      "authoriser_contact_id": "fb6a9252-3818-44dc-b5aa-2195391a746f",
      "schedule_ref": null,
      "status": "pending_approval",
      "responded_at": null,
      "created_at": "2016-12-19T02:10:56Z",
      "credit_ref": null,
      "payout": {
        "amount": 99000,
        "description": "The elite package for 4",
        "matures_at": "2016-12-25T00:00:00Z"
      }
    }
  ]
}
```

### Properties

*List outgoing Payment Requests (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## GetAPaymentRequestsHistoryResponse

<a id="schemagetapaymentrequestshistoryresponse"></a>

```json
{
  "data": [
    {
      "type": "payout_request",
      "event": "requested",
      "at": "2017-01-05T07:47:45Z",
      "ref": "PR.3",
      "by": "Fancy Pants (fancy_pants)"
    },
    {
      "type": "payout_request",
      "event": "approved",
      "at": "2017-01-07T06:13:52Z",
      "ref": "PR.3",
      "by": "Lycra Co (lycra_co)"
    },
    {
      "type": "debit",
      "event": "scheduled",
      "at": "2017-01-07T06:13:52Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "scheduled",
      "at": "2017-01-07T06:13:52Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "matured",
      "at": "2017-01-08T04:30:14Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "processing",
      "at": "2017-01-08T04:30:14Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "clearing",
      "at": "2017-01-08T19:02:20Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "debit",
      "event": "cleared",
      "at": "2017-01-11T19:07:52Z",
      "ref": "D.n",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "matured",
      "at": "2017-01-11T19:07:52Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "processing",
      "at": "2017-01-12T04:30:25Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "clearing",
      "at": "2017-01-12T05:17:32Z",
      "ref": "C.e",
      "by": "Split Payments"
    },
    {
      "type": "credit",
      "event": "cleared",
      "at": "2017-01-15T05:27:12Z",
      "ref": "C.e",
      "by": "Split Payments"
    }
  ]
}
```

### Properties

*Get a Payment Request's history (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## IssueARefundRequest

<a id="schemaissuearefundrequest"></a>

```json
{
  "amount": 500,
  "reason": "Because reason",
  "metadata": {
    "custom_key": "Custom string",
    "another_custom_key": "Maybe a URL"
  }
}
```

### Properties

*Issue a Refund (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|amount|number|true|Amount in cents refund|
|reason|string|false|Reason for the refund. Visible to both parties.|
|metadata|[Metadata](#schemametadata)|false|No description|

## IssueARefundResponse

<a id="schemaissuearefundresponse"></a>

```json
{
  "data": {
    "ref": "PRF.1",
    "for_ref": "C.59",
    "debit_ref": "D.hi",
    "created_at": "2017-05-08T07:20:24Z",
    "amount": 500,
    "reason": "Because reason",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Issue a Refund (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListIncomingRefundsResponse

<a id="schemalistincomingrefundsresponse"></a>

```json
{
  "data": [
    {
      "ref": "PRF.2",
      "for_ref": "D.5",
      "credit_ref": "C.q",
      "created_at": "2017-05-09T04:45:26Z",
      "amount": 5,
      "reason": "Because reason",
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

### Properties

*List incoming Refunds (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ListOutgoingRefundsResponse

<a id="schemalistoutgoingrefundsresponse"></a>

```json
{
  "data": [
    {
      "ref": "PRF.2",
      "for_ref": "C.5",
      "debit_ref": "D.5a",
      "created_at": "2017-05-09T04:45:26Z",
      "amount": 5,
      "reason": "Because reason",
      "metadata": {
        "custom_key": "Custom string",
        "another_custom_key": "Maybe a URL"
      }
    }
  ]
}
```

### Properties

*List outgoing Refunds (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## RetrieveARefundResponse

<a id="schemaretrievearefundresponse"></a>

```json
{
  "data": {
    "ref": "PRF.1",
    "for_ref": "C.59",
    "debit_ref": "D.hi",
    "created_at": "2017-05-08T07:20:24Z",
    "amount": 500,
    "reason": "Because reason",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Retrieve a Refund (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## RetryPayoutResponse

<a id="schemaretrypayoutresponse"></a>

```json
{
  "data": {
    "ref": "C.2",
    "parent_ref": "PR.039a",
    "type": "credit",
    "category": "payout",
    "created_at": "2016-12-05T23:15:00Z",
    "matures_at": "2016-12-06T23:15:00Z",
    "cleared_at": null,
    "bank_ref": null,
    "status": "maturing",
    "status_changed_at": "2016-12-05T23:15:00Z",
    "party_contact_id": "33c6e31d3-1dc1-448b-9512-0320bc44fdcf",
    "party_name": "Price and Sons",
    "party_nickname": "price-and-sons-2",
    "party_bank_ref": null,
    "description": "Money for jam",
    "amount": 1
  }
}
```

### Properties

*Retry a payout (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ListAllTransactionsResponse

<a id="schemalistalltransactionsresponse"></a>

```json
{
  "data": [
    {
      "ref": "D.3",
      "parent_ref": null,
      "type": "debit",
      "category": "payout_refund",
      "created_at": "2016-12-07T23:15:00Z",
      "matures_at": "2016-12-10T23:15:00Z",
      "cleared_at": "2016-12-10T23:15:00Z",
      "bank_ref": "DT.9a",
      "status": "cleared",
      "status_changed_at": "2016-12-10T23:15:00Z",
      "party_contact_id": "31354923-b1e9-4d65-b03c-415ead89cbf3",
      "party_name": "Sanford-Rees",
      "party_nickname": null,
      "party_bank_ref": "CT.11",
      "description": null,
      "amount": 20000
    },
    {
      "ref": "D.2",
      "parent_ref": "PB.2",
      "type": "debit",
      "category": "payout",
      "created_at": "2016-12-06T23:15:00Z",
      "matures_at": "2016-12-09T23:15:00Z",
      "cleared_at": null,
      "bank_ref": null,
      "status": "maturing",
      "status_changed_at": "2016-12-06T23:15:00Z",
      "party_contact_id": "3c6e31d3-1dc1-448b-9512-0320bc44fdcf",
      "party_name": "Gutmann-Schmidt",
      "party_nickname": null,
      "party_bank_ref": null,
      "description": "Batteries for hire",
      "amount": 2949299
    },
    {
      "ref": "C.2",
      "parent_ref": "PB.s0z",
      "type": "credit",
      "category": "payout",
      "created_at": "2016-12-05T23:15:00Z",
      "matures_at": "2016-12-06T23:15:00Z",
      "cleared_at": "2016-12-09T23:15:00Z",
      "bank_ref": "CT.1",
      "status": "cleared",
      "status_changed_at": "2016-12-09T23:15:00Z",
      "party_contact_id": "33c6e31d3-1dc1-448b-9512-0320bc44fdcf",
      "party_name": "Price and Sons",
      "party_nickname": "price-and-sons-2",
      "party_bank_ref": null,
      "description": "Online purchase",
      "amount": 19999,
      "metadata": {
        "customer_id": "xur4492",
        "product_ref": "TSXL392110x"
      }
    }
  ]
}
```

### Properties

*List all transactions (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## ProposeUnassignedAgreementRequest

<a id="schemaproposeunassignedagreementrequest"></a>

```json
{
  "expiry_in_seconds": 60,
  "single_use": false,
  "terms": {
    "per_payout": {
      "min_amount": null,
      "max_amount": 10000
    },
    "per_frequency": {
      "days": 7,
      "max_amount": 1000000
    }
  },
  "metadata": {
    "type": "Metadata",
    "description": "Use for your custom data and certain Split customisations. This data will be attached to the resulting Agreement and included in the Agreement Webhook Events.",
    "example": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Propose an Unassigned Agreement (request)*

|Name|Type|Required|Description|
|---|---|---|---|
|expiry_in_seconds|number|true|The amount of time in seconds before the Unassigned Agreement can no longer be accepted.|
|single_use|boolean|false|Optionally propose a single use agreement. When the Unassigned Agreement is accepted and a Payment Request is approved according to the Agreement terms, the agreement will automatically become <code>expended</code>.<br><br>The proposed agreement must have equal max/min <code>per_payout</code> amounts and <code>null</code> <code>per_frequency</code> amounts.<br><br>Furthermore, we will automatically check that the authoriser's bank account has sufficient funds to honour the agreement terms.|
|terms|[Terms](#schematerms)|true|No description|

## ProposeUnassignedAgreementResponse

<a id="schemaproposeunassignedagreementresponse"></a>

```json
{
  "data": {
    "ref": "A.4k",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "status": "proposed",
    "responded_at": null,
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "assignment_expires_at": "2017-03-20T00:54:27Z",
    "link": "https://go.sandbox.split.cash/unassigned_agreements/b61fc159-8779-4a17-a826-e398e3e7e211/invitation",
    "metadata": {
      "custom_key": "Custom string",
      "another_custom_key": "Maybe a URL"
    }
  }
}
```

### Properties

*Propose an Unassigned Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## ListUnassignedAgreementsResponse

<a id="schemalistunassignedagreementsresponse"></a>

```json
{
  "data": [
    {
      "ref": "A.4k",
      "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
      "status": "proposed",
      "responded_at": null,
      "created_at": "2017-03-20T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": 10000,
          "min_amount": null
        },
        "per_frequency": {
          "days": 7,
          "max_amount": 1000000
        }
      },
      "assignment_expires_at": "2017-03-20T00:54:27Z",
      "link": "https://go.sandbox.split.cash/unassigned_agreements/b61fc159-8779-4a17-a826-e398e3e7e211/invitation"
    },
    {
      "ref": "A.7ea",
      "initiator_id": "b61fc159-8779-4a17-a826-e398e3e7e211",
      "status": "proposed",
      "responded_at": null,
      "created_at": "2017-03-21T00:53:27Z",
      "terms": {
        "per_payout": {
          "max_amount": null,
          "min_amount": null
        },
        "per_frequency": {
          "days": null,
          "max_amount": null
        }
      },
      "assignment_expires_at": "2017-03-21T00:54:27Z",
      "link": "https://go.sandbox.split.cash/unassigned_agreements/4e2728cc-b4ba-42c2-a6c3-26a7758de58d/invitation"
    }
  ]
}
```

### Properties

*List Unassigned Agreements (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|[object]|true|No description|

## GetUnassignedAgreementResponse

<a id="schemagetunassignedagreementresponse"></a>

```json
{
  "data": {
    "ref": "A.4k",
    "initiator_id": "4e2728cc-b4ba-42c2-a6c3-26a7758de58d",
    "status": "proposed",
    "responded_at": null,
    "created_at": "2017-03-20T00:53:27Z",
    "terms": {
      "per_payout": {
        "max_amount": 10000,
        "min_amount": null
      },
      "per_frequency": {
        "days": 7,
        "max_amount": 1000000
      }
    },
    "assignment_expires_at": "2017-03-20T00:54:27Z",
    "link": "https://go.sandbox.split.cash/unassigned_agreements/b61fc159-8779-4a17-a826-e398e3e7e211/invitation"
  }
}
```

### Properties

*Get an Unassigned Agreement (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

## GetUserDetailsResponse

<a id="schemagetuserdetailsresponse"></a>

```json
{
  "data": {
    "first_name": "Bear",
    "last_name": "Dog",
    "mobile_phone": "0456945832",
    "email": "bear@dog.com",
    "account": {
      "name": "Dog Bones Inc",
      "nickname": "dog-bones-inc",
      "abn": "129959040",
      "phone": "0418495033",
      "street_address": "98 Acme Avenue",
      "suburb": "Lead",
      "state": "NSW",
      "postcode": "2478"
    }
  }
}
```

### Properties

*Get User details (response)*

|Name|Type|Required|Description|
|---|---|---|---|
|data|object|true|No description|

