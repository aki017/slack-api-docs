This method returns a list of all User Groups in the team. This can optionally include disabled User Groups.

## Arguments

This method has the URL `https://slack.com/api/usergroups.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `usergroups:read`) |
| `include_disabled` | `1` | Optional | Include disabled User Groups. |
| `include_count` | `1` | Optional | Include the number of users in each User Group. |
| `include_users` | `1` | Optional | Include the list of users for each User Group. |

## Response

Returns a list of [usergroup objects](/types/usergroup), in no particular order:

```
{
    "ok": true,
    "usergroups": [
        {
            "id": "S0614TZR7",
            "team_id": "T060RNRCH",
            "is_usergroup": true,
            "name": "Team Admins",
            "description": "A group of all Administrators on your team.",
            "handle": "admins",
            "is_external": false,
            "date_create": 1446598059,
            "date_update": 1446670362,
            "date_delete": 0,
            "auto_type": "admin",
            "created_by": "USLACKBOT",
            "updated_by": "U060RNRCZ",
            "deleted_by": null,
            "prefs": {
                "channels": [

                ],
                "groups": [

                ]
            },
            "user_count": "2"
        },
        {
            "id": "S06158AV7",
            "team_id": "T060RNRCH",
            "is_usergroup": true,
            "name": "Team Owners",
            "description": "A group of all Owners on your team.",
            "handle": "owners",
            "is_external": false,
            "date_create": 1446678371,
            "date_update": 1446678371,
            "date_delete": 0,
            "auto_type": "owner",
            "created_by": "USLACKBOT",
            "updated_by": "USLACKBOT",
            "deleted_by": null,
            "prefs": {
                "channels": [

                ],
                "groups": [

                ]
            },
            "user_count": "1"
        },
        {
            "id": "S0615G0KT",
            "team_id": "T060RNRCH",
            "is_usergroup": true,
            "name": "Marketing Team",
            "description": "Marketing gurus, PR experts and product advocates.",
            "handle": "marketing-team",
            "is_external": false,
            "date_create": 1446746793,
            "date_update": 1446747767,
            "date_delete": 1446748865,
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
    ]
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `user_is_restricted` | This method cannot be called by a restricted user or single channel guest. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
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

