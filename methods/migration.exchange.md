For Enterprise Grid workspaces, map local user IDs to global user IDs

## Facts

| Method URL: | `https://slack.com/api/migration.exchange` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`tokens.basic`](/scopes/tokens.basic) |

 |

* * *

Easily convert your vintage user IDs to [Enterprise Grid](/enterprise-grid)-friendly global user IDs.

This method is best used in conjunction with turning off the [translation layer](/enterprise-grid#translation_layer) for your app, as a bulk conversion step just after a workspace migrates to Enterprise Grid.

By providing a list of "local" user IDs associated with the same workspace as your token, you can exchange your IDs for "global" user IDs beginning with the letter `W`.

You can use any existing tokens authorized for the team to request for the user mappings.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `users` | `W1234567890,U2345678901,U3456789012` | Required | A comma-separated list of user ids, up to 400 per request |
| `to_old` | `true` | Optional, default= | Specify `true` to convert `W` global user IDs to workspace-specific `U` IDs. Defaults to `false`. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

The `to_old` parameter is `false` by default. When `false`, the method returns a `user_id_map` mapping from local user IDs to global user IDs. For a reverse mapping from _global_ user IDs back to _local_ user IDs, set `to_old` to `true`.

## Response

Typical success response when mappings exist for the specified user IDs

```
{
    "ok": true,
    "team_id": "T1KR7PE1W",
    "enterprise_id": "E1KQTNXE1",
    "user_id_map": {
        "U06UBSUN5": "W06M56XJM",
        "U06UEB62U": "W06PTT6GH",
        "U06UBSVB3": "W06PUUDLY",
        "U06UBSVDX": "W06PUUDMW",
        "W06UAZ65Q": "W06UAZ65Q"
    },
    "invalid_user_ids": [
        "U21ABZZXX"
    ]
}
```

Typical error response when there are no mappings to provide

```
{
    "ok": false,
    "error": "not_enterprise_team"
}
```

The method may only be used on workspaces that have migrated to enterprise. When used on typical workspaces, a `not_enterprise_team` error is thrown.

## Additional nuance

Users that were already part of a workspace migrating to Enterprise Grid have two user IDs: a local user ID and a global user ID. Users that are created post-migration or on workspaces that are created _after_ migration have only global user IDs.

When using this method and attempting to convert a global user ID to a local user ID and that corresponding user _only_ has a global user ID, you'll receive the global user ID on both sides of the map.

Providing invalid users or user IDs not belonging to the related workspace will result with those IDs being listed in an `invalid_user_ids` array.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_enterprise_team` | The workspace associated with the token is not part of an Enterprise organization. User IDs have not changed and there is nothing to map. |
| `too_many_users` | Too many user IDs provided in `users`. Up to 400 user IDs are allowed per request. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
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

