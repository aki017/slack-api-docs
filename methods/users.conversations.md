List conversations the calling user may access.

## Facts

| Method URL: | `https://slack.com/api/users.conversations` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`channels:read`](/scopes/channels:read) [`groups:read`](/scopes/groups:read) [`im:read`](/scopes/im:read) [`mpim:read`](/scopes/mpim:read) |

 |

* * *

<ts-icon class="ts_icon_comment"></ts-icon> As part of the [Conversations API](/docs/conversations-api), this method's required scopes depend on the type of channel-like object you're working with. For classic Slack apps, a corresponding `channels:` scope is required when working with public channels, `groups:` for private channels, also the same rules are applied for `im:` and `mpim:`. For workspace apps, a `conversations:` scope is all that's needed.

This method helps answer questions like:

- Which conversations am I a member of?
- Which public channels is my bot user in?
- Do I have any direct messages open with my friend Suzy?
- Is my bot a member of any private channels?

`users.conversations` returns a list of all [channel-like conversations](/types/conversation) accessible to the user or app tied to the presented token, as part of our [Conversations API](/docs/conversations-api).

Browse the public channel membership of other users with the `user` parameter. Private channel membership is only listed when the calling user, bot user, or app shares membership in a direct message, multi-person direct message, or private channel. Further filter channels by type with the `types` parameter.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Paginate through collections of data by setting the `cursor` parameter to a `next_cursor` attribute returned by a previous request's `response_metadata`. Default value fetches the first "page" of the collection. See [pagination](/docs/pagination) for more detail. |
| `exclude_archived` | `true` | Optional, default=false | Set to `true` to exclude archived channels from the list |
| `limit` | `20` | Optional, default=100 | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the list hasn't been reached. Must be an integer no larger than 1000. |
| `types` | `im,mpim` | Optional, default=public\_channel | Mix and match channel types by providing a comma-separated list of any combination of `public_channel`, `private_channel`, `mpim`, `im` |
| `user` | `W0B2345D` | Optional | Browse conversations by a specific user ID's membership. Non-public channels are restricted to those where the calling user shares membership. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a list of limited channel-like [conversation objects](/types/conversation).

Typical success response with only public channels. Note how `num_members` and `is_member` are not returned like typical `conversations` objects.

```
{
    "ok": true,
    "channels": [
        {
            "id": "C012AB3CD",
            "name": "general",
            "is_channel": true,
            "is_group": false,
            "is_im": false,
            "created": 1449252889,
            "creator": "U012A3CDE",
            "is_archived": false,
            "is_general": true,
            "unlinked": 0,
            "name_normalized": "general",
            "is_shared": false,
            "is_ext_shared": false,
            "is_org_shared": false,
            "pending_shared": [],
            "is_pending_ext_shared": false,
            "is_private": false,
            "is_mpim": false,
            "topic": {
                "value": "Company-wide announcements and work-based matters",
                "creator": "",
                "last_set": 0
            },
            "purpose": {
                "value": "This channel is for team-wide communication and announcements. All team members are in this channel.",
                "creator": "",
                "last_set": 0
            },
            "previous_names": []
        },
        {
            "id": "C061EG9T2",
            "name": "random",
            "is_channel": true,
            "is_group": false,
            "is_im": false,
            "created": 1449252889,
            "creator": "U061F7AUR",
            "is_archived": false,
            "is_general": false,
            "unlinked": 0,
            "name_normalized": "random",
            "is_shared": false,
            "is_ext_shared": false,
            "is_org_shared": false,
            "pending_shared": [],
            "is_pending_ext_shared": false,
            "is_private": false,
            "is_mpim": false,
            "topic": {
                "value": "Non-work banter and water cooler conversation",
                "creator": "",
                "last_set": 0
            },
            "purpose": {
                "value": "A place for non-work-related flimflam, faffing, hodge-podge or jibber-jabber you'd prefer to keep out of more focused work-related channels.",
                "creator": "",
                "last_set": 0
            },
            "previous_names": []
        }
    ],
    "response_metadata": {
        "next_cursor": "dGVhbTpDMDYxRkE1UEI="
    }
}
```

Example response when mixing different conversation types together, like `im` and `mpim`

```
{
    "ok": true,
    "channels": [
        {
            "id": "G0AKFJBEU",
            "name": "mpdm-mr.banks--slactions-jackson--beforebot-1",
            "is_channel": false,
            "is_group": true,
            "is_im": false,
            "created": 1493657761,
            "creator": "U061F7AUR",
            "is_archived": false,
            "is_general": false,
            "unlinked": 0,
            "name_normalized": "mpdm-mr.banks--slactions-jackson--beforebot-1",
            "is_shared": false,
            "is_ext_shared": false,
            "is_org_shared": false,
            "pending_shared": [],
            "is_pending_ext_shared": false,
            "is_private": true,
            "is_mpim": true,
            "is_open": true,
            "topic": {
                "value": "Group messaging",
                "creator": "U061F7AUR",
                "last_set": 1493657761
            },
            "purpose": {
                "value": "Group messaging with: @mr.banks @slactions-jackson @beforebot",
                "creator": "U061F7AUR",
                "last_set": 1493657761
            },
            "priority": 0
        },
        {
            "id": "D0C0F7S8Y",
            "created": 1498500348,
            "is_im": true,
            "is_org_shared": false,
            "user": "U0BS9U4SV",
            "is_user_deleted": false,
            "priority": 0
        },
        {
            "id": "D0BSHH4AD",
            "created": 1498511030,
            "is_im": true,
            "is_org_shared": false,
            "user": "U0C0NS9HN",
            "is_user_deleted": false,
            "priority": 0
        }
    ],
    "response_metadata": {
        "next_cursor": "aW1faWQ6RDBCSDk1RExI"
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

To get a full [conversation object](/types/conversations), call the [`conversations.info`](/methods/conversations.info) method. We omit the `is_member` and `num_members` fields in this method's response.

See [conversation object](/types/conversations) for more detail on returned fields.

## Pagination

This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.

Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.

See [pagination](/docs/pagination) for more information.

## Token support

Using a [bot user token](/docs/token-types#bot), this method returns the channels and conversations your bot is party to. Specifying a `user` parameter filters to conversations your bot shares with that user.

A [user token](/docs/token-types#user) armed with [`channels:read`](/scopes/channels:read) will similarly supply the channels the calling user is a member of. Supplying the `user` parameter narrows results to conversations featuring both the calling user and the identified user.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `method_not_supported_for_channel_type` | This type of conversation cannot be used with this method. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invalid_types` | Value passed for `type` could not be used based on the method's capabilities or the permission scopes granted to the used token. |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `invalid_limit` | Value passed for `limit` is not understood. |
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

