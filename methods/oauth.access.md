This method allows you to exchange a temporary OAuth `code` for an API access token. This is used as part of the [OAuth authentication flow](/docs/oauth).

As discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1) it is possible to supply the Client ID and Client Secret using the HTTP Basic authentication scheme. If HTTP Basic authentication is used you do not need to supply the`client_id` and `client_secret` parameters as part of the request.

## Arguments

This method has the URL `https://slack.com/api/oauth.access` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `client_id` | `4b39e9-752c4` | Required | Issued when you created your application. |
| `client_secret` | `33fea0113f5b1` | Required | Issued when you created your application. |
| `code` | `ccdaa72ad` | Required | The `code` param returned via the OAuth callback. |
| `redirect_uri` | `http://example.com` | Optional | This must match the originally submitted URI (if one was sent). |

## Response

```
{
    "access_token": "xoxt-23984754863-2348975623103",
    "scope": "read"
}
```

You can use the returned token to call protected API methods on behalf of the user.

# Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_client_id` | Value passed for `client_id` was invalid. |
| `bad_client_secret` | Value passed for `client_secret` was invalid. |
| `invalid_code` | Value passed for `code` was invalid. |
| `bad_redirect_uri` | Value passed for `redirect_uri` did not match the `redirect_uri` in the original request. |

