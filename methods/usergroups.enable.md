Enable a User Group

## Facts

| Method URL: | `https://slack.com/api/usergroups.enable` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`usergroups:write`](/scopes/usergroups:write) |
| [user](/docs/token-types#user) | [`usergroups:write`](/scopes/usergroups:write) [`post`](/scopes/post) |

 |

* * *

This method enables a User Group which was previously disabled.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `usergroup` | `S0604QSJC` | Required | The encoded ID of the User Group to enable. |
| `include_count` | `true` | Optional | Include the number of users in the User Group. |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

```
{
    "ok": true,
    "usergroup": {
        "id": "S0615G0KT",
        "team_id": "T060RNRCH",
        "is_usergroup": true,
        "name": "Marketing Team",
        "description": "Marketing gurus, PR experts and product advocates.",
        "handle": "marketing-team",
        "is_external": false,
        "date_create": 1446746793,
        "date_update": 1446747767,
        "date_delete": 0,
        "auto_type": null,
        "created_by": "U060RNRCZ",
        "updated_by": "U060RNRCZ",
        "deleted_by": null,
        "prefs": {
            "channels": [

            ],
            "groups": [

            ]
        },
        "user_count": "0"
    }
}
```

When a User Group is enabled, it's `date_delete` parameter will be `0` (zero).

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `permission_denied` | The user does not have permission to enables the User Group. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
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

