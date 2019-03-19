## Facts

| Method URL: | `https://slack.com/api/chat.scheduleMessage` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`chat:write:user`](/scopes/chat:write:user)&nbsp; [`chat:write:bot`](/scopes/chat:write:bot)&nbsp; |

 |

* * *

This method schedules [a message](/docs/messages) for delivery to a public channel, private channel, or direct message/IM channel at a specified time in the future.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel, private group, or DM channel to send message to. Can be an encoded ID, or a name. See below for more details. |
| `post_at` | `299876400` | Required | Unix EPOCH timestamp of time in future to send the message. |
| `text` | `Hello world` | Required | Text of the message to send. See below for an explanation of formatting. This field is usually required, unless you're providing only `attachments` instead. Provide no more than 40,000 characters or [risk truncation](/changelog/2018-04-truncating-really-long-messages). |
| `as_user` | `true` | Optional | Pass true to post the message as the authed user, instead of as a bot. Defaults to false. See authorship below. |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | A JSON-based array of structured attachments, presented as a URL-encoded string. |
| `blocks` | `[{"type": "section", "text": {"type": "plain_text", "text": "Hello world"}}]` | Optional | A JSON-based array of structured blocks, presented as a URL-encoded string. |
| `link_names` | `true` | Optional | Find and link channel names and usernames. |
| `parse` | `full` | Optional | Change how messages are treated. Defaults to `none`. See below. |
| `reply_broadcast` | `true` | Optional | Used in conjunction with `thread_ts` and indicates whether reply should be made visible to everyone in the channel or conversation. Defaults to `false`. |
| `thread_ts` | `1234567890.123456` | Optional | Provide another message's `ts` value to make this message a reply. Avoid using a reply's `ts` value; use its parent instead. |
| `unfurl_links` | `true` | Optional | Pass true to enable unfurling of primarily text-based content. |
| `unfurl_media` | `false` | Optional | Pass false to disable unfurling of media content. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

The `post_at` arg is a Unix EPOCH timestamp, representing the time the message should post to Slack in the future.

With the exception of the additional `post_at` argument, this method behaves identically to [`chat.postMessage`](/methods/chat.postMessage). Peruse that page for in-depth documentation on each parameter that this method accepts.

A message must have either `text` or `attachments` or both. The `text` parameter is required unless you provide `attachments`. You can use both parameters in conjunction with each other to create awesome messages.

## Restrictions

You will only be able to schedule a message up to 120 days into the future. If you specify a `post_at` timestamp beyond this limit, you’ll receive a `time_too_far` error response.

## Response

Typical success response

```
{
    "ok": true,
    "channel": "C1H9RESGL",
    "scheduled_message_id": "Q1298393284",
    "post_at": "1562180400",
    "message": {
        "text": "Here's a message for you in the future",
        "username": "ecto1",
        "bot_id": "B19LU7CSY",
        "attachments": [
            {
                "text": "This is an attachment",
                "id": 1,
                "fallback": "This is an attachment's fallback"
            }
        ],
        "type": "delayed_message",
        "subtype": "bot_message"
    }
}
```

Typical error response if the `post_at` is invalid (ex. in the past or too far into the future)

```
{
    "ok": false,
    "error": "time_in_past"
}
```

The response includes the `scheduled_message_id` assigned to your message. Use it with the [`chat.deleteScheduledMessage`](/methods/chat.deleteScheduledMessage) method to delete the message before it is sent.

For details on formatting, usage in threads, and rate limiting, check out [`chat.postMessage`](/methods/chat.postMessage) documentation.

## Channels

You **must** specify a public channel, private channel, or an IM channel with the `channel` argument. Each one behaves slightly differently based on the authenticated user's permissions and additional arguments:

#### Post to a channel

You can either pass the channel's name (`#general`) or encoded ID (`C024BE91L`), and the message will be posted to that channel. The channel's ID can be retrieved through the [channels.list](/methods/channels.list) API method.

#### Post to a DM

Pass the IM channel's ID (`D023BB3L2`) or a user's ID (`U0G9QF9C6`) as the value of `channel` to post to that IM channel as the app. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.

You might receive a `channel_not_found` error if your app doesn't have permission to enter into an IM with the intended user.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_time` | value passed for `post_time` was invalid. |
| `time_in_past` | value passed for `post_time` was in the past. |
| `time_too_far` | value passed for `post_time` was too far into the future. |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_in_channel` | Cannot post user messages to a channel they are not in. |
| `is_archived` | Channel has been archived. |
| `msg_too_long` | Message text is too long |
| `no_text` | No message text provided |
| `restricted_action` | A workspace preference prevents the authenticated user from posting. |
| `restricted_action_read_only_channel` | Cannot post any message into a read-only channel. |
| `restricted_action_thread_only_channel` | Cannot post top-level messages into a thread-only channel. |
| `restricted_action_non_threadable_channel` | Cannot post thread replies into a non\_threadable channel. |
| `too_many_attachments` | Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message. |
| `rate_limited` | Application has posted too many messages, [read the Rate Limit documentation](/docs/rate-limits) for more information |
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
| `message_truncated` | The `text` field of a message should have no more than 40,000 characters. We [truncate really long messages](/changelog/2018-04-truncating-really-long-messages). |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

