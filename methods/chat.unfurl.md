Provide custom unfurl behavior for user-posted URLs

## Facts

| Method URL: | `https://slack.com/api/chat.unfurl` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`links:write`](/scopes/links:write) |
| [user](/docs/token-types#user) | [`links:write`](/scopes/links:write) |

 |

* * *

This method attaches Slack [app unfurl behavior](/docs/message-link-unfurling#slack_app_unfurling) to a specified and relevant message. A user token is required as this method does not support bot user tokens.

The first time this method is executed with a particular `ts` and `channel` combination, the valid `unfurls` attachments you provide will be attached to the message. Subsequent attempts with the same `ts` and `channel` values will modify the same attachments, rather than adding more.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel ID of the message |
| `ts` | &nbsp; | Required | Timestamp of the message to add unfurl behavior to. |
| `unfurls` | &nbsp; | Required | URL-encoded JSON map with keys set to URLs featured in the the message, pointing to their unfurl message attachments. |
| `user_auth_message` | &nbsp; | Optional | Provide a simply-formatted string to send as an ephemeral message to the user as invitation to authenticate further and enable full unfurling behavior |
| `user_auth_required` | `true` | Optional, default=0 | Set to `true` or `1` to indicate the user must install your Slack app to trigger unfurls for this domain |
| `user_auth_url` | `https://example.com/onboarding?user_id=xxx` | Optional | Send users to this custom URL where they will complete authentication in your app to fully trigger unfurling. Value should be properly URL-encoded. |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

The provided `ts` value must correspond to a message in the specified `channel`. Additionally, the message must contain a fully-qualified URL pointing to a domain that is already registered and associated with your Slack app.

The `unfurls` parameter expects a URL-encoded string of JSON. Unlike [`chat.postMessage`](/methods/chat.postMessage)'s `attachments` parameter, it does not expect a JSON array but instead, a hash keyed on the specific URLs you're offering an unfurl for. Each URL can have a [single attachment](/docs/message-attachments), including message buttons.

Consult the [unfurling](/docs/message-link-unfurling#slack_app_unfurling) docs for [more guidance](/docs/message-link-unfurling#unfurls_parameter) on this parameter.

The `user_auth_required` parameter is optional. By providing a `1` or `true` value, it will require the user posting the link first authenticate themselves with your app. See also the [authenticated unfurling docs](/docs/message-link-unfurling#authenticated_unfurls).

To point users to a specific page on your server to authenticate, pass a valid URL using the `user_auth_url` parameter. When sending this parameter via `application/x-www-form-urlencoded` GETs or POSTs, values must be URL-encoded such that `https://example.com/onboarding?user_id=xxx` becomes `https%3A%2F%2Fexample.com%2Fonboarding%3Fuser_id%3Dxxx`.

Or, you can send an ephemeral message to that user by providing a simple `user_auth_message` value. [Simple slack message formatting](/docs/message-formatting) like `*bold*`, `_italics_`, and linking is supported, so you can wrap your custom URLs in a blanket of situationally accurate, actionable text.

Specifying `user_auth_url` or `user_auth_message` will automatically imply `user_auth_required` being set to `true`. If both `user_auth_url` and `user_auth_message` are provided, `user_auth_message` takes precedence.

## Response

Typical, minimal success response

```
{
    "ok": true
}
```

Typical error response

```
{
    "ok": false,
    "error": "cannot_unfurl_url"
}
```

As you can see, we provide a minimal positive response when your unfurl attempt is successful. When it is not, you'll receive one of the errors below and `ok` will not be `false`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `cannot_unfurl_url` | The URL cannot be unfurled. This error may be returned if you haven't acknowledged a `link_shared` event tied to the same URL. It is also returned when the domain appears in a workspace's administrative blacklists. |
| `cannot_find_message` | The `ts` value in the request does not match a message. |
| `cannot_find_service` | A record of your app being allowed to unfurl for this workspace could not be found. |
| `missing_unfurls` | The request is missing the `unfurls` parameter. |
| `cannot_prompt` | The current user has already interacted with and dismissed a prompt for this application. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
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

