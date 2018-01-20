Creates a channel.

## Facts

| Method URL: | `https://slack.com/api/channels.create` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | [`application/x-www-form-urlencoded`](/web#post_bodies "Learn more about sending requests"), [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`channels:write`](/scopes/channels:write) |
| [user](/docs/token-types#user) | [`channels:write`](/scopes/channels:write) [`read`](/scopes/read) |

 |

* * *

This method is used to create a channel.

To create a private channel, use [`groups.create`](/methods/groups.create).

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `name` | `mychannel` | Required | Name of channel to create |
| `validate` | `true` | Optional | Whether to return errors on invalid channel name instead of modifying it to meet the specified criteria. |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Naming

Channel names can only contain lowercase letters, numbers, hyphens, and underscores, and must be 21 characters or less. We will validate the submitted channel name and modify it to meet the above criteria. When calling this method, we recommend storing the channel's `name` value that is returned in the response.

## Response

Typical success response resulting in a new channel

```
{
    "ok": true,
    "channel": {
        "id": "C0DEL09A5",
        "name": "endeavor",
        "is_channel": true,
        "created": 1502833204,
        "creator": "U061F7AUR",
        "is_archived": false,
        "is_general": false,
        "name_normalized": "endeavor",
        "is_shared": false,
        "is_org_shared": false,
        "is_member": true,
        "is_private": false,
        "is_mpim": false,
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
        "members": [
            "U061F7AUR"
        ],
        "topic": {
            "value": "",
            "creator": "",
            "last_set": 0
        },
        "purpose": {
            "value": "",
            "creator": "",
            "last_set": 0
        },
        "previous_names": []
    }
}
```

Example error response when an invalid name is provided

```
{
    "ok": false,
    "error": "invalid_name_specials",
    "detail": "Value passed for `name` contained unallowed special characters."
}
```

If successful, the command returns a [channel object](/types/channel), including state information.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `name_taken` | A channel cannot be created with the given name. |
| `restricted_action` | A team preference prevents the authenticated user from creating channels. |
| `no_channel` | Value passed for `name` was empty. |
| `invalid_name_required` | Value passed for `name` was empty. |
| `invalid_name_punctuation` | Value passed for `name` contained only punctuation. |
| `invalid_name_maxlength` | Value passed for `name` exceeded max length. |
| `invalid_name_specials` | Value passed for `name` contained unallowed special characters or upper case characters. |
| `invalid_name` | Value passed for `name` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |
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

