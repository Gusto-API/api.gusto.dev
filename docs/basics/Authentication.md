---
tags: [Auth]
---

# Authentication

## Safety First!

Due to the sensitive nature of payroll, all potential integrations must be vetted and approved by Gusto.

When developing an integration, all API calls should be made to `api.gusto-demo.com`. Once the integration is reviewed by Gusto, we’ll request a callback URI for your production system. After you receive production API keys, all calls should be made to `api.gusto.com`.

To obtain your API keys, please sign up for a [Gusto Developer Account](https://dev.gusto.com). Please note that at this time we do not support additional integrations related to medical benefits, worker’s compensation, background checks, other payroll providers, or platform integrators

## OAuth

### Getting Started
Authentication is done using [OAuth2](http://oauth.net/2/). Numerous libraries implementing the protocol can be found on the [OAuth2 homepage](http://oauth.net/2/).
 
Should you choose to implement your own flow, or if you just want to know more about what goes on behind the scenes, we'll walk through the basics of authenticating with Gusto via OAuth. It can be a bit tricky - even with a library - so if you have questions at any point, please send us an email.
 
Only primary admins or full access admins on the Gusto account can enable and authenticate a new application’s access (the “user” in the outline below). A user may also be an administrator for multiple companies. This is not uncommon and your application should prepare for it. Accountants use Gusto to manage payroll for many companies simultaneously and many customers can be associated with multiple companies.

![](../../assets/images/oauth-final.png)

During the authorization step, Gusto will automatically prompt the user to select which company or companies they would like to provide access to. A multi-user scenario can be handled in two ways: 

1. Create the option for the user to sync information from multiple Gusto accounts into your application by creating a UI that leverages the company ID - the unique identifier for each company - when pulling in payroll in order to differentiate the payroll information for each individual company.
2. Force the user to select which Gusto company they want to pull payroll information from. This can be done simply via in-app education prior to the authentication step (ex. please select one company to connect) or in the event that the user authorizes multiple companies, you can create a UI within your application that forces the user to select one company to sync information from.


### Outline:

-   Direct user to authorize
-   User authorizes application to access their information
-   User redirected to partner site with authorization code
-   Exchange authorization code for access/refresh token pair
-   Make requests, always including the access token parameter
-   Exchange refresh token for new access/refresh tokens

Here is the sample application information we'll use throughout:

      id:           bbb286ff1a4fe6b84742b0d49b8d0d65bd0208d27d3d50333591df71c45da519
      secret:       cb06cb755b868a819ead51671f0f7e9c35c7c4cbbae0e38bef167e0e4ba64ee6
      redirect_uri: https://example.com/callback

The `redirect_uri` is sometimes referred to as a `callback URI` and the id can also be called a `client_id`. The id and secret will be generated when you supply a `redirect_uri` to Gusto. OAuth2 does not support wildcard URIs or URIs with fragments (e.g #).

*Note: when you receive your API keys for Gusto, we will include an API Token. This API token is only used when creating a company through the API.*

#### Authorization Code

> **Expiration Time:** 10 minutes
>
> **HTTP Method:** GET
>
> **URL:** <https://api.gusto.com/oauth/authorize>
>
> **Parameters:**
>
> -   `client_id` your client id
> -   `redirect_uri` [percent-encoded](https://en.wikipedia.org/wiki/Percent-encoding) url you submitted when signing up > for the Gusto API. Should the user accept integration, the user will be returned to this url with the `code` parameter set to the authorization > code.
> -   `response_type` the literal string `code`

The first step is a user authorizing your application to access their information on Gusto. To do this, you'll create a link to Gusto where they can approve access.

The link contains the parameters outlined above. For this sample application, the link would look something like this:

```html
  <a href="https://api.gusto.com/oauth/authorize?client_id=bbb286ff1a4fe6b84742b0d49b8d0d65bd0208d27d3d50333591df71c45da519&redirect_uri=https%3A%2F%2Fexample.com%2Fcallback&response_type=code">Authorize with Gusto</a>
```

On Gusto, the user will be prompted to log in to their Gusto account and authorize integration with your application for one or more of their companies.

After accepting, Gusto will generate an authorization code and the user will be redirected to the redirect_uri with that code attached. In this case, the user will be sent to a url like this:

    https://example.com/callback?code=51d5d63ae28783aecd59e7834be2c637a9ee260f241b191565aa10fe380471db

This parameter contains the authorization code that you will then use to obtain your first access token.

#### Access Token

> **Expiration Time:** 2 hours.
>
> **HTTP Method:** POST
>
> **URL:** <https://api.gusto.com/oauth/token>
>
> **Parameters:**
>
> -   `client_id` - your client id
> -   `client_secret` - your client secret
> -   `redirect_uri` - the [percent-encoded](https://en.wikipedia.org/wiki/Percent-encoding) url you submitted when signing up for the Gusto API.
> -   `code` - the code being exchanged for an access token. This should be the Authorization Code received above (`51d5d63ae28783aecd59e7834be2c637a9ee260f241b191565aa10fe380471db`.)
> -   `grant_type` - this should be the literal string "authorization_code"

Next, you will make a server-side request to Gusto with your authorization code to `https://api.gusto.com/oauth/token` with the parameters outlined above. These parameters should be POST data in the **body** of the request, rather than its query parameters. In this case, the body of the request to `https://api.gusto.com/oauth/token` would look like this with a `Content-Type` of `application/json`:

```json
{
  "client_id": "bbb286ff1a4fe6b84742b0d49b8d0d65bd0208d27d3d50333591df71c45da519",
  "client_secret": "cb06cb755b868a819ead51671f0f7e9c35c7c4cbbae0e38bef167e0e4ba64ee6",
  "redirect_uri": "https://example.com/callback",
  "code": "51d5d63ae28783aecd59e7834be2c637a9ee260f241b191565aa10fe380471db",
  "grant_type": "authorization_code"
}
```

That's a lot of information.

The `client_id`, `client_secret`, and `redirect_uri` are used to identify your application. We ensure that not only is it a valid application requesting a token, but also that it is the same application to which the included `code` was granted. The `code` matches the application to the user and the `grant_type` tells us what type of code is included.

Upon successful authentication, the response will look like this:

```json
{
  "access_token": "de6780bc506a0446309bd9362820ba8aed28aa506c71eedbe1c5c4f9dd350e54",
  "token_type": "bearer",
  "expires_in": 7200,
  "refresh_token": "8257e65c97202ed1726cf9571600918f3bffb2544b26e00a61df9897668c33a1"
}
```

The `access_token` should be included in the `Authorization` HTTP header with every call to the API. Failure to include the `access_token` or using an expired token will result in a 401 response. For example, to use the above token in a subsequent request, include the following in the request's HTTP headers:

    Authorization: Bearer de6780bc506a0446309bd9362820ba8aed28aa506c71eedbe1c5c4f9dd350e54

### Refresh Token

> **Expiration Time:** Never (invalid after one use)
>
> **HTTP Method** POST
>
> **URL** <https://api.gusto.com/oauth/token>
>
> **Parameters:**
>
> -   `client_id` your client id
> -   `client_secret` your client secret
> -   `redirect_uri` the percent-encoded url you submitted when signing up for the Gusto API.
> -   `refresh_token` the refresh_token being exchanged for an access token code.
> -   `grant_type` this should be the literal string 'refresh_token'

Access tokens expire 2 hours after they are issued.

You may exchange your refresh token for a new access token once, making a request very similar to exchanging authorization code for an access token.

The only difference is that `code` is now `refresh_token` and `grant_type` is set to "refresh_token". Assuming you are refreshing the access token received in the previous section, here are POST paramters in the body for the request you would make to `https://api.gusto.com/oauth/token`:

```json
{
  "client_id": "bbb286ff1a4fe6b84742b0d49b8d0d65bd0208d27d3d50333591df71c45da519",
  "client_secret": "cb06cb755b868a819ead51671f0f7e9c35c7c4cbbae0e38bef167e0e4ba64ee6",
  "redirect_uri": "https://example.com/callback",
  "refresh_token": "8257e65c97202ed1726cf9571600918f3bffb2544b26e00a61df9897668c33a1",
  "grant_type": "refresh_token"
}
```

The corresponding response, including both a fresh access token and a new refresh token, will look something like this:

```json
{
  "access_token": "de6780bc506a0446309bd9362820ba8aed28aa506c71eedbe1c5c4f9dd350e54",
  "token_type": "bearer",
  "expires_in": 7200,
  "refresh_token": "8257e65c97202ed1726cf9571600918f3bffb2544b26e00a61df9897668c33a1"
}
```

## API Token Authentication

When creating a new Gusto company via the API, the application is acting on behalf of
itself rather than a Gusto user. For these, certified partners are granted
an API token in their [Developer Account](https://dev.gusto.com/) under Organizations. This token is included in the authorization HTTP header with the
`Token` scheme.

### Example

**HTTP Headers**

    Content-Type: application/json
    Authorization: Token bbb286ff1a4fe6b84742b0d49b8d0d65bd0208d27d3d50333591df71c45da519
