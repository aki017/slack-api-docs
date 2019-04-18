## Facts

| Method URL: | `https://slack.com/api/conversations.replies` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`channels:history`](/scopes/channels:history)&nbsp; [`groups:history`](/scopes/groups:history)&nbsp; [`im:history`](/scopes/im:history)&nbsp; [`mpim:history`](/scopes/mpim:history)&nbsp; |

 |

* * *

<ts-icon class="ts_icon_comment"></ts-icon>As part of the [Conversations API](/docs/conversations-api), this method's required scopes depend on the type of channel-like object you're working with. For classic Slack apps, a corresponding `channels:` scope is required when working with public channels, `groups:` for private channels, also the same rules are applied for `im:` and `mpim:`. For workspace apps, a `conversations:` scope is all that's needed.

This [Conversations API](/docs/conversations-api) method returns an entire thread (a message plus all the messages in reply to it), while [`conversations.history`](/methods/conversations.history) method returns only parent messages.

The scopes and token types required to use this method vary by conversation type.

[Bot user tokens](/docs/token-types#bot) may use this method for direct message and multi-party direct message conversations but lack sufficient permissions to use this method on public and private channels.

To use `conversations.replies` with public or private channel threads, use a [user token](/docs/token-types#user) with [`channels:history`](/scopes/channels:history) or [`groups:history`](/scopes/groups:history) scopes.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Conversation ID to fetch thread from. |
| `ts` | `1234567890.123456` | Required | Unique identifier of a thread's parent message. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Paginate through collections of data by setting the `cursor` parameter to a `next_cursor` attribute returned by a previous request's `response_metadata`. Default value fetches the first "page" of the collection. See [pagination](/docs/pagination) for more detail. |
| `inclusive` | `true` | Optional, default=0 | Include messages with latest or oldest timestamp in results only when either timestamp is specified. |
| `latest` | `1234567890.123456` | Optional, default=now | End of time range of messages to include in results. |
| `limit` | `20` | Optional, default=10 | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the users list hasn't been reached. |
| `oldest` | `1234567890.123456` | Optional, default=0 | Start of time range of messages to include in results. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

The `channel` and `ts` arguments are always required. `ts` must be the timestamp of an existing message with 0 or more replies. If there are no replies then just the single message referenced by `ts` will return - it is just an ordinary message.

## Response

Typical success response

```
{
    "messages": [
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "island",
            "thread_ts": "1482960137.003543",
            "reply_count": 3,
            "replies": [
                {
                    "user": "U061F7AUR",
                    "ts": "1483037603.017503"
                },
                {
                    "user": "U061F7AUR",
                    "ts": "1483051909.018632"
                },
                {
                    "user": "U061F7AUR",
                    "ts": "1483125339.020269"
                }
            ],
            "subscribed": true,
            "last_read": "1484678597.521003",
            "unread_count": 0,
            "ts": "1482960137.003543"
        },
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "one island",
            "thread_ts": "1482960137.003543",
            "parent_user_id": "U061F7AUR",
            "ts": "1483037603.017503"
        },
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "two island",
            "thread_ts": "1482960137.003543",
            "parent_user_id": "U061F7AUR",
            "ts": "1483051909.018632"
        },
        {
            "type": "message",
            "user": "U061F7AUR",
            "text": "three for the land",
            "thread_ts": "1482960137.003543",
            "parent_user_id": "U061F7AUR",
            "ts": "1483125339.020269"
        }
    ],
    "has_more": true,
    "ok": true,
    "response_metadata": {
        "next_cursor": "bmV4dF90czoxNDg0Njc4MjkwNTE3MDkx"
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "thread_not_found"
}
```

## Pagination
This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.  
  
Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.  
  
 See [pagination](/docs/pagination) for more information.  
  

### Pagination by time

You can also get messages of certain time range by specifying either or combination of both `latest` and `oldest`.

If a message has the same timestamp as `latest` or `oldest` it will not be included in the list. This allows a client to fetch all messages in a hole in channel history, by calling `conversations.replies` with `latest` set to the oldest message they have after the hole, and `oldest` to the latest message they have before the hole. However, you can cover the hole and include the exact moment of the specified timestamp, set `inclusive` to `true`. The `inclusive` is ignored when `latest` or `oldest` is not specified.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value for `channel` was missing or invalid. |
| `thread_not_found` | Value for `ts` was missing or invalid. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `invalid_ts_latest` | Value passed for `latest` was invalid |
| `invalid_ts_oldest` | Value passed for `oldest` was invalid |
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

