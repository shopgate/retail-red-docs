# Authentication

All Shopgate services use the authentication [OAuth 2](https://oauth.net/2/).
Before you can use an API, you must fetch a token from the authentication service. This token can be fetched via the Auth Layer Service reachable at `https://auth.shopgate.io`.

## Requesting a New Token

There are two authentication methods to get a new token:

- password
- refresh_token

### Password

To request a new token via the password method, call the endpoint `/oauth/token`.
Enter the clientId and clientSecret (provided by retail.red) in the `Authorization`, base64 encoded, like the basic authentication method.
The body must contain the email address of the API user in the username and password. Enter the tenantType (always "merchant" for now). Set the merchantCode of the merchant for whom you want to send the request as the `tenantId`.

> **Note**
> If you don't have a clientId and clientSecret, please contact <mail@retai.red>

The body follows the content type: x-www-form-urlencoded.

**Format of the request**

URL:

- Production `https://auth.shopgate.io/oauth/token`
- Staging: `https://auth.demo.shopgatedev.io/oauth/token`

Headers:

```
- `Authorization`: Basic base64(`${clientId}`, `${clientSecret}`)
- `Content-Type`: `application/x-www-form-urlencoded`
```

Body (Parameter as x-www-form-urlencoded):

```
- grant_type: `password`
- username: `${emailAddressOfTheAPIUser}`
- password: `${passwordOfTheAPIUser}`
- tenantType: `merchant`
- tenantId: `${merchantCode}`
```

**Format of the response**

```jsonld=
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjMiLCJlbWFpbCI6InRlc3RAc2hvcGdhdGUuY29tIiwiZmlyc3ROYW1lIjoiSm9obiIsImxhc3ROYW1lIjoiRG9lIiwic2NvcGVzIjpbIkNBQyJdLCJ0ZW5hbnRJZCI6InNvbWVNZXJjaGFudCIsInRlbmFudFR5cGUiOiJtZXJjaGFudCIsImlhdCI6MTU4MTI4NDQzMywiZXhwIjoxNTgxMjg4MDMzfQ.iybpll1c-QLz2U6bn-SDuiDIXcA3N3rH4WpCpD7Sh8w",
    "token_type": "Bearer",
    "expires_in": 3600,
    "refresh_token": "kcLKlSPYc2fDr4c2n0ApnYeGCFFCPmX3UJmNjwmi"
}
```

The response contains the accessToken and the refreshToken.
The accessToken must be sent with every request to the service to authenticate the request.
The token is only valid for 1 hour. After this time, no further request can be made to the services.

After 1 hour, a new access token can be requested by using the grant type `refresh` with the refresh token you got in the initial request.

### Refresh Token

The refresh is valid up to 3 months and can be used to request a new access token.
To request a new access token, send the following request:

**Format of the request**

URL:

- Production `https://auth.shopgate.io/oauth/token`
- Staging: `https://auth.demo.shopgatedev.io/oauth/token`

Headers:

```
- `Content-Type`: `application/x-www-form-urlencoded`
```

Body (Parameter as x-www-form-urlencoded):

```
- grant_type: `refresh_token`
- refresh_token: `${refresh_token}`
- tenantType: `merchant`
- tenantId: `${merchantCode}`
```

**Format of the response**

```jsonld=
{
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpZCI6IjMiLCJlbWFpbCI6InRlc3RAc2hvcGdhdGUuY29tIiwiZmlyc3ROYW1lIjoiSm9obiIsImxhc3ROYW1lIjoiRG9lIiwic2NvcGVzIjpbIkNBQyJdLCJ0ZW5hbnRJZCI6InNvbWVNZXJjaGFudCIsInRlbmFudFR5cGUiOiJtZXJjaGFudCIsImlhdCI6MTU4MTI4NDQzMywiZXhwIjoxNTgxMjg4MDMzfQ.iybpll1c-QLz2U6bn-SDuiDIXcA3N3rH4WpCpD7Sh8w",
    "token_type": "Bearer",
    "expires_in": 3600
}
```

The response contains the new access token. The refresh token is kept untouched and can be used to request the next access token.
