This method attaches Slack [app unfurl behavior](/docs/message-link-unfurling#slack_app_unfurling) to a specified and relevant message. A user token is required as this method does not support bot user tokens.

The first time this method is executed with a particular `ts` and `channel` combination, the valid `unfurls` attachments you provide will be attached to the message. Subsequent attempts with the same `ts` and `channel` values will modify the same attachments, rather than adding more.

## Arguments

This method has the URL `https://slack.com/api/chat.unfurl` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `links:write` |
| `channel` | `C1234567890` | Required | Channel ID of the message |
| `ts` | &nbsp; | Required | Timestamp of the message to add unfurl behavior to |
| `unfurls` | &nbsp; | Required | JSON mapping a set of URLs from the message to their unfurl attachments |
| `user_auth_required` | `true` | Optional, default=0 | Set to `true` or `1` to indicate the user must install your Slack app to trigger unfurls for this domain |

The provided `ts` value must correspond to a message in the specified `channel`. Additionally, the message must contain a fully-qualified URL pointing to a domain that is already registered and associated with your Slack app.

The `unfurls` parameter expects a URL-encoded string of JSON. Unlike [`chat.postMessage`](/methods/chat.postMessage)'s `attachments` parameter, it does not expect a JSON array but instead, a hash keyed on the specific URLs you're offering an unfurl for. Each URL can have a [single attachment](/docs/message-attachments), including message buttons.

Consult the [unfurling](/docs/message-link-unfurling#slack_app_unfurling) docs for [more guidance](/docs/message-link-unfurling#unfurls_parameter) on this parameter.

The `user_auth_required` parameter is optional and requires that the user posting the link to authenticate with your app first. See authenticated unfurls.

## Response

```
{
    "ok": true
}
```

As you can see, we provide a minimal positive response when your unfurl attempt is successful. When it is not, you'll receive one of the errors below and `ok` will not be `false`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

