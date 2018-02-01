Leaves a conversation.

## Facts

| Method URL: | `https://slack.com/api/conversations.leave` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | [`application/x-www-form-urlencoded`](/web#post_bodies "Learn more about sending requests"), [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`channels:write`](/scopes/channels:write) [`groups:write`](/scopes/groups:write) [`im:write`](/scopes/im:write) [`mpim:write`](/scopes/mpim:write) [`post`](/scopes/post) |

 |

* * *

<ts-icon class="ts_icon_comment"></ts-icon> As part of the [Conversations API](/docs/conversations-api), this method's required scopes depend on the type of channel-like object you're working with. A corresponding `channels:` scope is required when working with public channels, `groups:` for private channels, also the same rules are applied for `im:` and `mpim:`.

This [Conversations API](/docs/conversations-api) method makes like a tree and leaves a conversation.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Conversation to leave |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

Typical success response

```
{
    "ok": true
}
```

This method will not return an error if the user was not in the conversation before it was called. Instead the response will include a `not_in_channel` property:

```
{
    "ok": true,
    "not_in_channel": true
}
```

Typical error response when attempting to leave a workspace's "general" channel

```
{
    "ok": false,
    "error": "cant_leave_general"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `is_archived` | Channel has been archived. |
| `method_not_supported_for_channel_type` | This type of conversation cannot be used with this method. |
| `missing_scope` | The calling token is not granted the necessary scopes to complete this operation. |
| `cant_leave_general` | Authenticated user cannot leave the general channel |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |
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
