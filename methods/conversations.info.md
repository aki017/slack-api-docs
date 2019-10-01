Retrieve information about a conversation.

## Facts

| Method URL: | `https://slack.com/api/conversations.info` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`channels:read`](/scopes/channels:read)&nbsp; [`groups:read`](/scopes/groups:read)&nbsp; [`im:read`](/scopes/im:read)&nbsp; [`mpim:read`](/scopes/mpim:read)&nbsp; |

 |

* * *

<ts-icon class="ts_icon_comment"></ts-icon>As part of the [Conversations API](/docs/conversations-api), this method's required scopes depend on the type of channel-like object you're working with. For classic Slack apps, a corresponding `channels:` scope is required when working with public channels, `groups:` for private channels, also the same rules are applied for `im:` and `mpim:`. For workspace apps, a `conversations:` scope is all that's needed.

This [Conversations API](/docs/conversations-api) method returns information about a workspace [conversation](/types/conversation).

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Conversation ID to learn more about |
| `include_locale` | `true` | Optional | Set this to `true` to receive the locale for this conversation. Defaults to `false` |
| `include_num_members` | `true` | Optional, default=false | Set to `true` to include the member count for the specified conversation. Defaults to `false` |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a [conversation object](/types/conversation), which could be a public channel, private channel, direct message, multi-person direct message, depending completely on the `channel` ID and the permissions granted to your token.

Typical success response for a public channel. (Also, a response from a private channel and a multi-party IM is very similar to this example.)

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
        "parent_conversation": null,
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
        "locale": "en-US"
    }
}
```

Typical success response for a 1:1 direct message

```
{
    "ok": true,
    "channel": {
        "id": "C012AB3CD",
        "created": 1507235627,
        "is_im": true,
        "is_org_shared": false,
        "user": "U27FFLNF4",
        "last_read": "1513718191.000038",
        "latest": {
            "type": "message",
            "user": "U5R3PALPN",
            "text": "Psssst!",
            "ts": "1513718191.000038"
        },
        "unread_count": 0,
        "unread_count_display": 0,
        "is_open": true,
        "locale": "en-US",
        "priority": 0.043016851216706
    }
}
```

When using the method with the `include_num_members` parameter, we return a `num_members` field

```
{
    "ok": true,
    "channel": {
        "id": "C012AB3CD",
        "created": 1507235627,
        "is_im": true,
        "is_org_shared": false,
        "user": "U27FFLNF4",
        "last_read": "1513718191.000038",
        "latest": {
            "type": "message",
            "user": "U5R3PALPN",
            "text": "Psssst!",
            "ts": "1513718191.000038"
        },
        "unread_count": 0,
        "unread_count_display": 0,
        "is_open": true,
        "locale": "en-US",
        "priority": 0.043016851216706,
        "num_members": 2
    }
}
```

Typical error response when a channel cannot be found

```
{
    "ok": false,
    "error": "channel_not_found"
}
```

Some fields in the response, like `unread_count` and `unread_count_display`, are included for DM conversations only.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
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

