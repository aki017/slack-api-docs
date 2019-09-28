Fetches a conversation's history of messages and events.

## Facts

| Method URL: | `https://slack.com/api/conversations.history` |
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

This method returns a portion of [message events](/events/message) from the specified conversation.

To read the entire history for a conversation, call the method with no `latest` or`oldest` arguments, and then continue paging using the instructions below.

The scopes and token types required to use this method vary by conversation type.

[Bot user tokens](/docs/token-types#bot) may use this method for direct message and multi-party direct message conversations but lack sufficient permissions to use this method on public and private channels.

To use `conversations.history` with public or private channel threads, use a [user token](/docs/token-types#user) with [`channels:history`](/scopes/channels:history) or [`groups:history`](/scopes/groups:history) scopes.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Conversation ID to fetch history for. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Paginate through collections of data by setting the `cursor` parameter to a `next_cursor` attribute returned by a previous request's `response_metadata`. Default value fetches the first "page" of the collection. See [pagination](/docs/pagination) for more detail. |
| `inclusive` | `true` | Optional, default=0 | Include messages with latest or oldest timestamp in results only when either timestamp is specified. |
| `latest` | `1234567890.123456` | Optional, default=now | End of time range of messages to include in results. |
| `limit` | `20` | Optional, default=100 | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the users list hasn't been reached. |
| `oldest` | `1234567890.123456` | Optional, default=0 | Start of time range of messages to include in results. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Typical success response containing a channel's messages

```
{
    "ok": true,
    "messages": {
        "0": {
            "type": "message",
            "user": "U012AB3CDE",
            "text": "I find you punny and would like to smell your nose letter",
            "ts": "1512085950.000216"
        },
        "1": {
            "type": "message",
            "user": "U061F7AUR",
            "text": "What, you want to smell my shoes better?",
            "ts": "1512104434.000490"
        }
    },
    "has_more": true,
    "pin_count": 0,
    "response_metadata": {
        "next_cursor": "bmV4dF90czoxNTEyMDg1ODYxMDAwNTQz"
    }
}
```

Typical success response included formatted messages from bots and incoming webhooks

```
{
    "ok": true,
    "messages": {
        "0": {
            "type": "message",
            "user": "U012AB3CDE",
            "text": "I find you punny and would like to smell your nose letter",
            "ts": "1512085950.000216"
        },
        "1": {
            "type": "message",
            "user": "U061F7AUR",
            "text": "Isn't this whether dreadful? <https://badpuns.example.com/puns/123>",
            "attachments": {
                "0": {
                    "service_name": "Leg end nary a laugh, Ink.",
                    "text": "This is likely a pun about the weather.",
                    "fallback": "We're withholding a pun from you",
                    "thumb_url": "https://badpuns.example.com/puns/123.png",
                    "thumb_width": 1920,
                    "thumb_height": 700,
                    "id": 1
                }
            },
            "ts": "1512085950.218404"
        }
    },
    "has_more": true,
    "pin_count": 0,
    "response_metadata": {
        "next_cursor": "bmV4dF90czoxNTEyMTU0NDA5MDAwMjU2"
    }
}
```

Typical success response with `latest` timestamp and `inclusive` parameters specified

```
{
    "ok": true,
    "latest": "1512085950.000216",
    "messages": {
        "0": {
            "type": "message",
            "user": "U012AB3CDE",
            "text": "I find you punny and would like to smell your nose letter",
            "ts": "1512085950.000216"
        }
    },
    "has_more": true,
    "pin_count": 0,
    "response_metadata": {
        "next_cursor": "bmV4dF90czoxNTEyMzU2NTI2MDAwMTMw"
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "channel_not_found"
}
```

## Pagination
This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.  
  
Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.  
  
 See [pagination](/docs/pagination) for more information.  
  

### Pagination by time

This form of pagination can be used in conjunction with cursors.

The `messages` array contains up to 100 messages between `latest` and `oldest`. If there were more than 100 messages between those two points, then `has_more`will be true.

Provide timestamps in `latest` and `oldest` to specify the period in channel's history you want to cover. If a message has the same timestamp as `latest` or `oldest` it will not be included in the list, unless `inclusive` is true. The `inclusive` parameter is ignored when `latest` or `oldest` is not specified.

If the response includes `has_more` then the client can make another call, using the `ts` value of the final messages as the `latest` param to get the next page of messages.

If there are more than 100 messages between the two timestamps then the messages returned are the ones closest to `latest`. In most cases an application will want the most recent messages and will page backward from there. If `oldest` is provided but not `latest` then the messages returned are those closest to `oldest`, allowing you to page forward through history if desired.

### Retrieving a single message

`conversations.history` can also be used to pluck a single message from the archive.

You'll need a message's `ts` value, uniquely identifying it within a conversation. You'll also need that conversation's ID.

Provide another message's `ts` value _as_ the `latest` parameter. Set `limit` to `1`. If it exists, you'll receive the queried message in return.

Finally, use `inclusive=true` because otherwise we'll never retrieve the message we're actually after, just the ones that come after it.

```
GET /api/channels.history?token=TOKEN_WITH_CHANNELS_HISTORY_SCOPE&channel=C2EB2QT8A&latest=1476909142.000007&inclusive=true&limit=1
```

You can easily generate a permalink URL for any specific message using [`chat.getPermalink`](/methods/chat.getPermalink).

* * *

## Message types

Messages of type `"message"` are user-entered text messages sent to the channel, while other types are events that happened within the channel. All messages have both a `type` and a sortable `ts`, but the other fields depend on the `type`. For a list of all possible events, see the [channel messages](/events/message) documentation.

Messages that have been reacted to by team members will have a [reactions](/events/message#stars __pins__ and_reactions) array delightfully included.

If a message has been starred by the calling user, the `is_starred` property will be present and true. This property is only added for starred items, so is not present in the majority of messages.

The `is_limited` boolean property is only included for free teams that have reached the free message limit. If true, there are messages before the current result set, but they are beyond the message limit.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `pagination_not_available` | Pagination does not currently function on Enterprise Grid workspaces. |
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

