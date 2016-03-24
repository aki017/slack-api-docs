This method allows you to exchange a temporary OAuth `code` for an API access token. This is used as part of the [OAuth authentication flow](/docs/oauth).

As discussed [in RFC 6749](https://tools.ietf.org/html/rfc6749#section-2.3.1) it is possible to supply the Client ID and Client Secret using the HTTP Basic authentication scheme. If HTTP Basic authentication is used you do not need to supply the`client_id` and `client_secret` parameters as part of the request.

**Keep your tokens secure**. Do not share tokens with users or anyone else.

## Arguments

This method has the URL `https://slack.com/api/oauth.access` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `client_id` | `4b39e9-752c4` | Optional | Issued when you created your application. |
| `client_secret` | `33fea0113f5b1` | Optional | Issued when you created your application. |
| `code` | `ccdaa72ad` | Optional | The `code` param returned via the OAuth callback. |
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

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_client_id` | Value passed for `client_id` was invalid. |
| `bad_client_secret` | Value passed for `client_secret` was invalid. |
| `invalid_code` | Value passed for `code` was invalid. |
| `bad_redirect_uri` | Value passed for `redirect_uri` did not match the `redirect_uri` in the original request. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

