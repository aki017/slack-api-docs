This method returns a portion of messages/events from the specified multiparty direct message channel. To read the entire history for a multiparty direct message, call the method with no `latest` or`oldest` arguments, and then continue paging using the instructions below.

## Arguments

This method has the URL `https://slack.com/api/mpim.history` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `mpim:history`) |
| `channel` | &nbsp; | Required | Multiparty direct message to fetch history for. |
| `latest` | `1234567890.123456` | Optional, default=now | End of time range of messages to include in results. |
| `oldest` | `1234567890.123456` | Optional, default=0 | Start of time range of messages to include in results. |
| `inclusive` | `1` | Optional, default=0 | Include messages with latest or oldest timestamp in results. |
| `count` | `100` | Optional, default=100 | Number of messages to return, between 1 and 1000. |
| `unreads` | `1` | Optional, default=0 | Include `unread_count_display` in the output? |

## Response

```
{
    "ok": true,
    "latest": "1358547726.000003",
    "messages": [
        {
            "type": "message",
            "ts": "1358546515.000008",
            "user": "U2147483896",
            "text": "Hello"
        },
        {
            "type": "message",
            "ts": "1358546515.000007",
            "user": "U2147483896",
            "text": "World",
            "is_starred": true,
        },
        {
            "type": "something_else",
            "ts": "1358546515.000007",
            "wibblr": true
        }
    ],
    "has_more": false
}
```

The `messages` array up to 100 messages between `latest` and `oldest`. If there were more than 100 messages between those two points, then `has_more`will be true.

If a message has the same timestamp as `latest` or `oldest` it will not be included in the list, unless `inclusive` is true. This allows a client to fetch all messages in a hole in channel history, by calling channels.history with `latest` set to the oldest message they have after the hole, and `oldest`to the latest message they have before the hole. If the response includes`has_more` then the client can make another call, using the `ts` value of the final messages as the `latest` param to get the next page of messages.

If there are more than 100 messages between the two timestamps then the messages returned are the ones closest to `latest`. In most cases an application will want the most recent messages and will page backward from there. If `oldest` is provided but not `latest` then the messages returned are those closest to `oldest`, allowing you to page forward through history if desired.

Messages of type `"message"` are user-entered text messages sent to the multiparty direct message channel, while other types are events that happened within the multiparty direct message channel. All messages have both a `type` and a sortable`ts`, but the other fields depend on the `type`. For a list of all possible events, see the [channel messages](/docs/messages) documentation.

If a message has been starred by the calling user, the `is_starred` property will be present and true. This property is only added for starred items, so is not present in the majority of messages.

The `is_limited` boolean property is only included for free teams that have reached the free message limit. If true, there are messages before the current result set, but they are beyond the message limit.

# Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `invalid_ts_latest` | Value passed for `latest` was invalid |
| `invalid_ts_oldest` | Value passed for `oldest` was invalid |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

