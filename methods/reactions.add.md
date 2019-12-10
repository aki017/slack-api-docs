Adds a reaction to an item.

## Facts

| Method URL: | `https://slack.com/api/reactions.add` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`reactions:write`](/scopes/reactions:write)&nbsp; |
| [user](/docs/token-types#user) | [`reactions:write`](/scopes/reactions:write)&nbsp; |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

This method adds a reaction (emoji) to a message. Now that [file threads](/changelog/2018-05-file-threads-soon-tread#whats_changed) work the way you'd expect, the `file` and `file_comment` arguments are deprecated. Specify only `channel` and `timestamp` instead.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel where the message to add reaction to was posted. |
| `name` | `thumbsup` | Required | Reaction (emoji) name. |
| `timestamp` | `1234567890.123456` | Required | Timestamp of the message to add reaction to. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

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

After making this call, the reaction is saved and [a `reaction_added` event](/events/reaction_added) is broadcast via the [Events](/events-api) and [RTM](/rtm) APIs.

A `not_reactable` error is thrown when we decline the opportunity to attach your app's personally selected emoji reaction to a file or file comment. It's not because of how your app feels, it's because that approach is retired. Your app can express its inner reacji for any message though, by specifying `channel` and `timestamp`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_reactable` | Whatever you passed in, like a `file` or `file_comment`, can't be reacted to anymore. Your app can react to messages though. |
| `invalid_name` | Value passed for `name` was invalid. |
| `too_many_emoji` | The limit for distinct reactions (i.e emoji) on the item has been reached. |
| `message_not_found` | Message specified by `channel` and `timestamp` does not exist. |
| `already_reacted` | The specified item already has the user/reaction combination. |
| `bad_timestamp` | Value passed for `timestamp` was invalid. |
| `too_many_reactions` | The limit for reactions a person may add to the item has been reached. |
| `no_item_specified` | combination of `channel` and `timestamp` was not specified. |
| `is_archived` | Channel specified has been archived. |
| `channel_not_found` | Value passed for `channel` is invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
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

