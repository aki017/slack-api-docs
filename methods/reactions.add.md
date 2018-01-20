Adds a reaction to an item.

## Facts

| Method URL: | `https://slack.com/api/reactions.add` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | [`application/x-www-form-urlencoded`](/web#post_bodies "Learn more about sending requests"), [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [workspace](/docs/token-types#workspace) | [`reactions:write`](/scopes/reactions:write) |
| [user](/docs/token-types#user) | [`reactions:write`](/scopes/reactions:write) [`post`](/scopes/post) |

 |

* * *

This method adds a reaction (emoji) to an item (file, file comment, channel message, group message, or direct message). One of `file`, `file_comment`, or the combination of `channel` and `timestamp` must be specified.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `name` | `thumbsup` | Required | Reaction (emoji) name. |
| `channel` | `C1234567890` | Optional | Channel where the message to add reaction to was posted. |
| `file` | `F1234567890` | Optional | File to add reaction to. |
| `file_comment` | `Fc1234567890` | Optional | File comment to add reaction to. |
| `timestamp` | `1234567890.123456` | Optional | Timestamp of the message to add reaction to. |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

Typical success response

```
{
    "ok": true
}
```

Typical error response

```
{
    "error": "already_reacted",
    "ok": false
}
```

After making this call, the reaction is saved and [a `reaction_added` event](/events/reaction_added) is broadcast through the [RTM API](/rtm) for the calling user.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `file_not_found` | File specified by `file` does not exist. |
| `file_comment_not_found` | File comment specified by `file_comment` does not exist. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `no_item_specified` | `file`, `file_comment`, or combination of `channel` and `timestamp` was not specified. |
| `invalid_name` | Value passed for `name` was invalid. |
| `already_reacted` | The specified item already has the user/reaction combination. |
| `too_many_emoji` | The limit for distinct reactions (i.e emoji) on the item has been reached. |
| `too_many_reactions` | The limit for reactions a person may add to the item has been reached. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
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

