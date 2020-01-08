Gets information about a channel.

## Facts

| Method URL: | `https://slack.com/api/channels.info` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`channels:read`](/scopes/channels:read)&nbsp; |
| [user](/docs/token-types#user) | [`channels:read`](/scopes/channels:read)&nbsp; |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

<ts-icon class="ts_icon_warning"></ts-icon>
**This method is deprecated.** [&nbsp;Learn more](/changelog/2020-01-deprecating-antecedents-to-the-conversations-api).

Please use these methods instead:

- [`conversations.info`](/methods/conversations.info)

This method returns information about a team channel.

To retrieve information on a private channel, use [`groups.info`](/methods/groups.info).

[Shared channels](/shared-channels) are not returned by this method. Use the [Conversations API](/docs/conversations-api) methods instead, like [`conversations.info`](/methods/conversations.info).

The `members` array found in this and other methods will begin automatically truncating at 1,500 and eventually fewer results beginning December 1, 2017. As of March, 2018 the cap is 500. Please Use [`conversations.members`](/methods/conversations.members) to manage memberships instead. [Read on to learn more.](/changelog/2017-10-members-array-truncating)

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel to get info on |
| `include_locale` | `true` | Optional | Set this to `true` to receive the locale for this channel. Defaults to `false` |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a [channel object](/types/channel).

Typical success response

```
{
    "ok": true,
    "channel": {
        "id": "C1H9RESGL",
        "name": "busting",
        "is_channel": true,
        "created": 1466025154,
        "creator": "U0G9QF9C6",
        "is_archived": false,
        "is_general": false,
        "name_normalized": "busting",
        "is_shared": false,
        "is_org_shared": false,
        "is_member": true,
        "is_private": false,
        "is_mpim": false,
        "last_read": "1503435939.000101",
        "latest": {
            "text": "Containment unit is 98% full",
            "username": "ecto1138",
            "bot_id": "B19LU7CSY",
            "attachments": [
                {
                    "text": "Don't get too attached",
                    "id": 1,
                    "fallback": "This is an attachment fallback"
                }
            ],
            "type": "message",
            "subtype": "bot_message",
            "ts": "1503435956.000247"
        },
        "unread_count": 1,
        "unread_count_display": 1,
        "members": [
            "U0G9QF9C6",
            "U1QNSQB9U"
        ],
        "topic": {
            "value": "Spiritual containment strategies",
            "creator": "U0G9QF9C6",
            "last_set": 1503435128
        },
        "purpose": {
            "value": "Discuss busting ghosts",
            "creator": "U0G9QF9C6",
            "last_set": 1503435128
        },
        "previous_names": [
            "dusting"
        ]
    }
}
```

Error response when the specified channel cannot be found

```
{
    "ok": false,
    "error": "channel_not_found"
}
```

An `is_org_shared` attribute may appear set to `true` when the channel is part of a shared channel between multiple teams of an [Enterprise Grid](/enterprise-grid). You'll also find the team's `enterprise_id`. See the [Enterprise Grid shared channels documentation](/enterprise-grid#shared_channels) for more detail.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
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
| `member_list_truncated` | A `members` array included in a channel object was truncated at 1500 results. Use `conversations.members` to retrieve all results. |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

