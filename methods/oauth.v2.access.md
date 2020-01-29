Exchanges a temporary OAuth verifier code for an access token.

## Facts

| Method URL: | `https://slack.com/api/oauth.v2.access` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 4](/docs/rate-limits#tier_t4) |

* * *

This method allows you to exchange a temporary OAuth `code` for an API access token.

This is the third step of the [V2 OAuth authentication flow](/authentication/oauth-v2). Check out our [guide to new Slack apps](/authentication/basics) for more information.

We strongly recommend supplying the Client ID and Client Secret using the HTTP Basic authentication scheme, as discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1).

If at all possible, avoid sending `client_id` and `client_secret` as parameters in your request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `code` | `ccdaa72ad` | Required | The `code` param returned via the OAuth callback. |
| `client_id` | `4b39e9-752c4` | Optional | Issued when you created your application. |
| `client_secret` | `33fea0113f5b1` | Optional | Issued when you created your application. |
| `redirect_uri` | `http://example.com` | Optional | This must match the originally submitted URI (if one was sent). |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

A potential gotcha: while `redirect_uri` is optional, it is _required_ if your app passed it as a parameter to `oauth/authorization` in the first step of the OAuth flow.

## Response

Successful token request with scopes for both a bot user and a user token

```
{
    "ok": true,
    "access_token": "xoxb-17653672481-19874698323-pdFZKVeTuE8sk7oOcBrzbqgy",
    "token_type": "bot",
    "scope": "commands,incoming-webhook",
    "bot_user_id": "U0KRQLJ9H",
    "app_id": "A0KRD7HC3",
    "team": {
        "name": "Slack Softball Team",
        "id": "T9TK3CUKW"
    },
    "enterprise": {
        "name": "slack-sports",
        "id": "E12345678"
    },
    "authed_user": {
        "id": "U1234",
        "scope": "chat:write",
        "access_token": "xoxp-1234",
        "token_type": "user"
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_grant_type` | Value passed for `grant_type` was invalid. |
| `invalid_client_id` | Value passed for `client_id` was invalid. |
| `bad_client_secret` | Value passed for `client_secret` was invalid. |
| `invalid_code` | Value passed for `code` was invalid. |
| `bad_redirect_uri` | Value passed for `redirect_uri` did not match the `redirect_uri` in the original request. |
| `oauth_authorization_url_mismatch` | The OAuth flow was initiated on an incorrect version of the authorization url. The flow must be initiated via /oauth/v2/authorize . |
| `preview_feature_not_available` | Returned when the API method is not yet available on the team in context. |
| `invalid_arguments` | The method was called with invalid arguments. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The workspace associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |
| `fatal_error` | The server could not complete your operation(s) without encountering a catastrophic error. It's possible some aspect of the operation succeeded before the error was raised. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

