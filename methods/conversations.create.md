Initiates a public or private channel-based conversation

## Facts

| Method URL: | `https://slack.com/api/conversations.create` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`channels:manage`](/scopes/channels:manage)&nbsp; [`groups:write`](/scopes/groups:write)&nbsp; [`im:write`](/scopes/im:write)&nbsp; [`mpim:write`](/scopes/mpim:write)&nbsp; |
| [user](/docs/token-types#user) | [`channels:write`](/scopes/channels:write)&nbsp; [`groups:write`](/scopes/groups:write)&nbsp; [`im:write`](/scopes/im:write)&nbsp; [`mpim:write`](/scopes/mpim:write)&nbsp; |

 |

* * *

<ts-icon class="ts_icon_comment"></ts-icon>This [Conversations API](/docs/conversations-api) method's required scopes depend on the type of channel-like object you're working with. To use the method, you'll need at least one of the `channels:`, `groups:`, `im:` or `mpim:` scopes corresponding to the conversation type you're working with.

Create a public or private channel using this [Conversations API](/docs/conversations-api) method.

Use [`conversations.open`](/methods/conversations.open) to initiate or resume a direct message or multi-person direct message.

### Limits for workspace apps

At least one user needs to be invited when creating a public or private conversation. Otherwise workspace apps could create invisible channels, which might cause a few problems.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `name` | `mychannel` | Required | Name of the public or private channel to create |
| `is_private` | `true` | Optional | Create a private channel instead of a public one |
| `user_ids` | `W1234567890,U2345678901,U3456789012` | Optional | **Required** for workspace apps. A list of between 1 and 30 human users that will be added to the newly-created conversation. This argument has no effect when used by classic Slack apps. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Naming

Channel names may only contain lowercase letters, numbers, hyphens, and underscores, and must be 80 characters or less. When calling this method, we recommend storing both the channel's `id` and `name` value that returned in the response.

Channel names are always validated by this method, unlike [`channels.create`](/methods/channels.create).

## Response

If successful, the command returns a rather stark [conversation object](/types/conversation)

```
{
    "ok": true,
    "channel": {
        "id": "C0EAQDV4Z",
        "name": "endeavor",
        "is_channel": true,
        "is_group": false,
        "is_im": false,
        "created": 1504554479,
        "creator": "U0123456",
        "is_archived": false,
        "is_general": false,
        "unlinked": 0,
        "name_normalized": "endeavor",
        "is_shared": false,
        "is_ext_shared": false,
        "is_org_shared": false,
        "pending_shared": [],
        "is_pending_ext_shared": false,
        "is_member": true,
        "is_private": false,
        "is_mpim": false,
        "last_read": "0000000000.000000",
        "latest": null,
        "unread_count": 0,
        "unread_count_display": 0,
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
        "previous_names": [],
        "priority": 0
    }
}
```

Typical error response when name already in use

```
{
    "ok": false,
    "error": "name_taken"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `name_taken` | A channel cannot be created with the given name. |
| `restricted_action` | A team preference prevents the authenticated user from creating channels. |
| `no_channel` | Value passed for `name` was empty. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invalid_name_required` | Value passed for `name` was empty. |
| `invalid_name_punctuation` | Value passed for `name` contained only punctuation. |
| `invalid_name_maxlength` | Value passed for `name` exceeded max length. |
| `invalid_name_specials` | Value passed for `name` contained unallowed special characters or upper case characters. |
| `invalid_name` | Value passed for `name` was invalid. |
| `invalid_users` | Value passed for `user_ids` was empty or invalid. |
| `user_not_found` | One or more users in `user_ids` was not found. |
| `too_many_convos_for_team` | The workspace has exceeded its limit of public and private channels. |
| `too_many_convos_for_app_on_team` | This app has exceeded its per-workspace limit of public and private channels. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `is_bot` | This method cannot be called by a bot user. |
| `user_is_ultra_restricted` | This method cannot be called by a single channel guest. |
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

