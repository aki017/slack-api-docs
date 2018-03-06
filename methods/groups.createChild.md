Clones and archives a private channel.

## Facts

| Method URL: | `https://slack.com/api/groups.createChild` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`groups:write`](/scopes/groups:write) |
| [user](/docs/token-types#user) | [`groups:write`](/scopes/groups:write) [`post`](/scopes/post) |

 |

* * *

This method takes an existing private channel and performs the following steps:

- Renames the existing private channel (from "example" to "example-archived").
- Archives the existing private channel.
- Creates a new private channel with the name of the existing private channel.
- Adds all members of the existing private channel to the new private channel.

This is useful when inviting a new member to an existing private channel while hiding all previous chat history from them. In this scenario you can call`groups.createChild` followed by `groups.invite`.

The new private channel will have a special `parent_group` property pointing to the original archived private channel. This will only be returned for members of both private channels, so will not be visible to any newly invited members.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `G1234567890` | Required | Private channel to clone and archive. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

If successful, the command returns the new [group object](/types/group):

```
{
    "ok": true,
    "group": {
        "id": "G024BE91L",
        "name": "secretplans",
        "is_group": "true",
        "created": 1360782804,
        "creator": "U024BE7LH",
        "is_archived": false,
        "members": [
            "U024BE7LH"
        ],
        …
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `already_archived` | An archived group cannot be cloned |
| `restricted_action` | A team preference prevents the authenticated user from creating groups. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
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

