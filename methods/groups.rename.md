This method renames a private channel.

## Arguments

This method has the URL `https://slack.com/api/groups.rename` and follows the [Slack Web API calling conventions](/web#basics). <aside class="small">Present these parameters as part of an <code>application/x-www-form-urlencoded</code> querystring or POST body. <code>application/json</code> is not currently accepted.</aside>

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `groups:write` |
| `channel` | `G1234567890` | Required | Private channel to rename |
| `name` | &nbsp; | Required | New name for private channel. |
| `validate` | `true` | Optional | Whether to return errors on invalid channel name instead of modifying it to meet the specified criteria. |

## Naming

Private channel names can only contain lowercase letters, numbers, hyphens, and underscores, and must be 21 characters or less. We will validate the submitted channel name and modify it to meet the above criteria. When calling this method, we recommend storing the channel's `name` value that is returned in the response.

## Response

Returns a [group object](/types/group):

```
{
    "ok": true,
    "channel": {
        "id": "G024BE91L",
        "name": "secretplans",
        "is_group": "true",
        "created": 1360782804,
        "creator": "U024BE7LH",
        "is_archived": false,
        "members": [
            "U024BE7LH"
        ],

        "topic": { … },
        "purpose": { … },

        "last_read": "1401383885.000061",
        "latest": { … }
        "unread_count": 0,
        "unread_count_display": 0

    },
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `invalid_name` | Value passed for `name` was invalid. |
| `name_taken` | New channel name is taken |
| `invalid_name_required` | Value passed for `name` was empty. |
| `invalid_name_punctuation` | Value passed for `name` contained only punctuation. |
| `invalid_name_maxlength` | Value passed for `name` exceeded max length. |
| `invalid_name_specials` | Value passed for `name` contained unallowed special characters or upper case characters. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The team associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

