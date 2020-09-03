# test-markdown

| table | col1 | col2 |
| --- | --- | --- |
| text | <ul><li>item1</li><li>item1</li></ul> | text col2 |
| text | aaa <ul><li>item1</li><li>item1</li></ul> | text col2 |

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
