#### Required Options

##### `issuer`

The URL for your Okta organization or an Okta authentication server. [About the issuer](#about-the-issuer)

#### Additional Options

##### `clientId`

Client Id pre-registered with Okta for the OIDC authentication flow. [Creating your Okta application](#creating-your-okta-appliation)

##### `redirectUri`

The url that is redirected to when using `token.getWithRedirect`. This must be listed in your Okta application's [Login redirect URIs](#login-redirect-uris). If no `redirectUri` is provided, defaults to the current origin (`window.location.origin`). [Configuring your Okta application](#configuring-your-okta-application)

##### `postLogoutRedirectUri`

Specify the url where the browser should be redirected after [signOut](#signout). This url must be listed in your Okta application's [Logout redirect URIs](#logout-redirect-uris). If not specified, your application's origin (`window.location.origin`) will be used.  [Configuring your Okta application](#configuring-your-okta-application) |

##### `responseMode`

Applicable only for SPA clients using [PKCE OAuth Flow](#pkce-oauth-20-flow). By default, the authorization code is requested and parsed from the search query. Setting this value to `fragment` will cause the URL hash fragment to be used instead. If your application uses or alters the search query portion of the `redirectUri`, you may want to set this option to "fragment". This option affects both [token.getWithRedirect](#tokengetwithredirectoptions) and [token.parseFromUrl](#tokenparsefromurloptions)

##### `pkce`

Enable the [PKCE OAuth Flow](#pkce-oauth-20-flow). Default value is `true`. If set to `false`, the authorization flow will use the [Implicit OAuth Flow](#implicit-oauth-20-flow). When PKCE flow is enabled the authorize request will use `response_type=code` and `grant_type=authorization_code` on the token request. All these details are handled for you, including the creation and verification of code verifiers. Tokens can be retrieved on the login callback by calling [token.parseFromUrl](#tokenparsefromurloptions)

##### `authorizeUrl`

Specify a custom authorizeUrl to perform the OIDC flow. Defaults to the issuer plus "/v1/authorize".

##### `userinfoUrl`

Specify a custom userinfoUrl. Defaults to the issuer plus "/v1/userinfo".

##### `tokenUrl`

Specify a custom tokenUrl. Defaults to the issuer plus "/v1/token".

##### `ignoreSignature`

> :warning: This option should be used only for browser support and testing purposes.

ID token signatures are validated by default when `token.getWithoutPrompt`, `token.getWithPopup`,  `token.getWithRedirect`, and `token.verify` are called. To disable ID token signature validation for these methods, set this value to `true`.

##### `maxClockSkew`

Defaults to 300 (five minutes). This is the maximum difference allowed between a client's clock and Okta's, in seconds, when validating tokens. Setting this to 0 is not recommended, because it increases the likelihood that valid tokens will fail validation.

##### `tokenManager`

An object containing additional properties used to configure the internal token manager.

###### `autoRenew`

By default, the library will attempt to renew tokens before they expire. If you wish to  to disable auto renewal of tokens, set autoRenew to false.

###### `storage`

You may pass an object or a string. If passing an object, it should meet the requirements of a [custom storage provider](#storage). Pass a string to specify one of the built-in storage types: 

* [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) (default)
* [`sessionStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage)
* [`cookie`](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)
* `memory`: a simple in-memory storage provider

###### `storageKey`

By default all tokens will be stored under the key `okta-token-storage`. You may want to change this if you have multiple apps running on a single domain which share the same storage type. Giving each app a unique storage key will prevent them from reading or writing each other's token values.

##### `cookies`

An object containing additional properties used when setting cookies

###### `secure`

Defaults to `true`, unless the application origin is `http://localhost`, in which case it is forced to `false`. If `true`, the SDK will set the "Secure" option on all cookies. When this option is `true`, an exception will be thrown if the application origin is not using the HTTPS protocol. Setting to `false` will allow setting cookies on an HTTP origin, but is not recommended for production applications.

###### `sameSite`

Defaults to `none` if the `secure` option is `true`, or `lax` if the `secure` option is false. Allows fine-grained control over the same-site cookie setting. A value of `none` allows embedding within an iframe. A value of `lax` will avoid being blocked by user "3rd party" cookie settings. A value of `strict` will block all cookies when redirecting from Okta and is not recommended.

#### Example Client

```javascript
var config = {
  // Required config
  issuer: 'https://{yourOktaDomain}/oauth2/default',

  // Required for login flow using getWithRedirect()
  clientId: 'GHtf9iJdr60A9IYrR0jw',
  redirectUri: 'https://acme.com/oauth2/callback/home',

  // Parse authorization code from hash fragment instead of search query
  responseMode: 'fragment',

  // Configure TokenManager to use sessionStorage instead of localStorage
  tokenManager: {
    storage: 'sessionStorage'
  }
};

var authClient = new OktaAuth(config);
```
