Fetches history of messages and events from a private channel.

## Facts

| Method URL: | `https://slack.com/api/groups.history` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`groups:history`](/scopes/groups:history)&nbsp; |

 |

* * *

This method returns a portion of messages/events from the specified private channel. To read the entire history for a private channel, call the method with no `latest` or`oldest` arguments, and then continue paging using the instructions below.

<ts-icon class="ts_icon_warning"></ts-icon> **Bots belonging to Slack apps are not supported**  

This method cannot be called with bot user tokens belonging to Slack apps, although legacy bot tokens will work. To use this method in a Slack app, use a [user token](/docs/token-types#user) imbued with the necessary scope. [**Stay tuned**](/changelog) for updates as we bring a fuller feast of features to bots belonging to Slack apps.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `G1234567890` | Required | Private channel to fetch history for. |
| `count` | `100` | Optional, default=100 | Number of messages to return, between 1 and 1000. |
| `inclusive` | `true` | Optional, default=0 | Include messages with latest or oldest timestamp in results. |
| `latest` | `1234567890.123456` | Optional, default=now | End of time range of messages to include in results. |
| `oldest` | `1234567890.123456` | Optional, default=0 | Start of time range of messages to include in results. |
| `unreads` | `true` | Optional, default=0 | Include `unread_count_display` in the output? |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Typical success response

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
            "is_starred": true
        },
        {
            "type": "something_else",
            "ts": "1358546515.000007"
        }
    ],
    "has_more": false
}
```

Typical error response

```
{
    "ok": false,
    "error": "channel_not_found"
}
```

The `messages` array up to 100 messages between `latest` and `oldest`. If there were more than 100 messages between those two points, then `has_more`will be true.

If a message has the same timestamp as `latest` or `oldest` it will not be included in the list, unless `inclusive` is true. This allows a client to fetch all messages in a hole in channel history, by calling channels.history with `latest` set to the oldest message they have after the hole, and `oldest`to the latest message they have before the hole. If the response includes`has_more` then the client can make another call, using the `ts` value of the final messages as the `latest` param to get the next page of messages.

If there are more than 100 messages between the two timestamps then the messages returned are the ones closest to `latest`. In most cases an application will want the most recent messages and will page backward from there. If `oldest` is provided but not `latest` then the messages returned are those closest to `oldest`, allowing you to page forward through history if desired.

Messages of type `"message"` are user-entered text messages sent to the group, while other types are events that happened within the group. All messages have both a `type` and a sortable`ts`, but the other fields depend on the `type`. For a list of all possible events, see the [channel messages](/docs/messages) documentation.

If a message has been starred by the calling user, the `is_starred` property will be present and true. This property is only added for starred items, so is not present in the majority of messages.

The `is_limited` boolean property is only included for free teams that have reached the free message limit. If true, there are messages before the current result set, but they are beyond the message limit.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `invalid_ts_latest` | Value passed for `latest` was invalid |
| `invalid_ts_oldest` | Value passed for `oldest` was invalid |
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
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

