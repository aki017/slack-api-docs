Invites users to a channel.

## Facts

| Method URL: | `https://slack.com/api/conversations.invite` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`channels:manage`](/scopes/channels:manage)&nbsp; [`groups:write`](/scopes/groups:write)&nbsp; [`im:write`](/scopes/im:write)&nbsp; [`mpim:write`](/scopes/mpim:write)&nbsp; |
| [user](/docs/token-types#user) | [`channels:write`](/scopes/channels:write)&nbsp; [`groups:write`](/scopes/groups:write)&nbsp; [`im:write`](/scopes/im:write)&nbsp; [`mpim:write`](/scopes/mpim:write)&nbsp; |

 |

* * *

<ts-icon class="ts_icon_comment"></ts-icon>This [Conversations API](/docs/conversations-api) method's required scopes depend on the type of channel-like object you're working with. To use the method, you'll need at least one of the `channels:`, `groups:`, `im:` or `mpim:` scopes corresponding to the conversation type you're working with.

This [Conversations API](/docs/conversations-api) method invites 1-1000 users to a public or private channel. The calling user must be a member of the channel.

### Limits for workspace apps

Because workspace apps can't act on behalf of users, they don't have the power to invite users to conversations, except when they're the owner/creator of the conversation.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | The ID of the public or private channel to invite user(s) to. |
| `users` | `W1234567890,U2345678901,U3456789012` | Required | A comma separated list of user IDs. Up to 1000 users may be listed. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

Typical success response when an invitation is extended

```
{
    "ok": true,
    "channel": {
        "id": "C012AB3CD",
        "name": "general",
        "is_channel": true,
        "is_group": false,
        "is_im": false,
        "created": 1449252889,
        "creator": "W012A3BCD",
        "is_archived": false,
        "is_general": true,
        "unlinked": 0,
        "name_normalized": "general",
        "is_read_only": false,
        "is_shared": false,
        "is_ext_shared": false,
        "is_org_shared": false,
        "pending_shared": [],
        "is_pending_ext_shared": false,
        "is_member": true,
        "is_private": false,
        "is_mpim": false,
        "last_read": "1502126650.228446",
        "topic": {
            "value": "For public discussion of generalities",
            "creator": "W012A3BCD",
            "last_set": 1449709364
        },
        "purpose": {
            "value": "This part of the workspace is for fun. Make fun here.",
            "creator": "W012A3BCD",
            "last_set": 1449709364
        },
        "previous_names": [
            "specifics",
            "abstractions",
            "etc"
        ],
        "num_members": 23,
        "locale": "en-US"
    }
}
```

Typical error response when an invite is attempted on a conversation type that does not support it

```
{
    "ok": false,
    "error": "method_not_supported_for_channel_type"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `user_not_found` | Value passed for `users` was invalid. |
| `no_user` | No value was passed for `users`. |
| `cant_invite_self` | Authenticated user cannot invite themselves to a channel. |
| `not_in_channel` | Authenticated user is not in the channel. |
| `already_in_channel` | Invited user is already in the channel. |
| `is_archived` | Channel has been archived. |
| `cant_invite` | User cannot be invited to this channel. |
| `method_not_supported_for_channel_type` | This type of conversation cannot be used with this method. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invitee_cant_see_channel` | The Grid multi-workspace channel you are inviting a user to is not shared with any workspaces the user is currently a member of. |
| `ura_max_channels` | URA is already in the maximum number of channels. |
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

