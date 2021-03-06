Sends a message to a channel.

## Facts

| Method URL: | `https://slack.com/api/chat.postMessage` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Special](/docs/rate-limits#tier_t5) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`chat:write`](/scopes/chat:write)&nbsp; |
| [user](/docs/token-types#user) | [`chat:write:user`](/scopes/chat:write:user)&nbsp; [`chat:write:bot`](/scopes/chat:write:bot)&nbsp; |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

This method posts [a message](/docs/messages) to a public channel, private channel, or direct message/IM channel.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel, private group, or IM channel to send message to. Can be an encoded ID, or a name. See below for more details. |
| `text` | `Hello world` | Required | How this field works and whether it is required depends on other fields you use in your API call. See below for more detail. |
| `as_user` | `true` | Optional | Pass true to post the message as the authed user, instead of as a bot. Defaults to false. See authorship below. This argument may not be used with newer [bot tokens](/docs/token-types#granular_bot). |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | A JSON-based array of structured attachments, presented as a URL-encoded string. |
| `blocks` | `[{"type": "section", "text": {"type": "plain_text", "text": "Hello world"}}]` | Optional | A JSON-based array of structured blocks, presented as a URL-encoded string. |
| `icon_emoji` | `:chart_with_upwards_trend:` | Optional | Emoji to use as the icon for this message. Overrides `icon_url`. Must be used in conjunction with `as_user` set to `false`, otherwise ignored. See authorship below. This argument may not be used with newer [bot tokens](/docs/token-types#granular_bot). |
| `icon_url` | `http://lorempixel.com/48/48` | Optional | URL to an image to use as the icon for this message. Must be used in conjunction with `as_user` set to false, otherwise ignored. See authorship below. This argument may not be used with newer [bot tokens](/docs/token-types#granular_bot). |
| `link_names` | `true` | Optional | Find and link channel names and usernames. |
| `mrkdwn` | `false` | Optional, default=true | Disable Slack markup parsing by setting to `false`. Enabled by default. |
| `parse` | `full` | Optional | Change how messages are treated. Defaults to `none`. See below. |
| `reply_broadcast` | `true` | Optional | Used in conjunction with `thread_ts` and indicates whether reply should be made visible to everyone in the channel or conversation. Defaults to `false`. |
| `thread_ts` | `1234567890.123456` | Optional | Provide another message's `ts` value to make this message a reply. Avoid using a reply's `ts` value; use its parent instead. |
| `unfurl_links` | `true` | Optional | Pass true to enable unfurling of primarily text-based content. |
| `unfurl_media` | `false` | Optional | Pass false to disable unfurling of media content. |
| `username` | `My Bot` | Optional | Set your bot's user name. Must be used in conjunction with `as_user` set to false, otherwise ignored. See authorship below. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

Please note that the `as_user` parameter _may not_ be used by [new Slack apps](/authentication/basics). Read more about Authorship to understand how `as_user` works for classic Slack apps.

**Using `text` with `blocks` or `attachments`**

The usage of the `text` field changes depending on whether you're using `blocks`. If you are using `blocks`, this is used as a fallback string to display in notifications. If you aren't, this is the main body text of the message. It can be formatted as plain text, or with `mrkdwn`.

The `text` field is not enforced as required when using `blocks` or `attachments`. However, we highly recommended that you include `text` to provide a fallback when using `blocks`, as described above.

### JSON POST support

As of October 2017, it's now possible to send a well-formatted `application/json` POST body to `chat.postMessage` and other [Web API](/web) write methods. No need to carefully URL-encode your JSON `attachments` and present all other fields as URL encoded key/value pairs; just send JSON instead.

Now you can send messages lovingly authored with the [message builder](/docs/messages/builder) to `chat.postMessage` without further modification.

Learn more about this support in the [Web API](/web) docs or [this changelog](/changelog/2017-10-keeping-up-with-the-jsons).

## Response

Typical success response

```
{
    "ok": true,
    "channel": "C1H9RESGL",
    "ts": "1503435956.000247",
    "message": {
        "text": "Here's a message for you",
        "username": "ecto1",
        "bot_id": "B19LU7CSY",
        "attachments": [
            {
                "text": "This is an attachment",
                "id": 1,
                "fallback": "This is an attachment's fallback"
            }
        ],
        "type": "message",
        "subtype": "bot_message",
        "ts": "1503435956.000247"
    }
}
```

Typical error response if too many attachments are included

```
{
    "ok": false,
    "error": "too_many_attachments"
}
```

The response includes the "timestamp ID" (`ts`) and the channel-like thing where the message was posted. It also includes the complete message object, as parsed by our servers. This may differ from the provided arguments as our servers sanitize links, attachments, and other properties. Your message may mutate.

## Formatting messages

Messages are formatted as described in the [formatting spec](/docs/message-formatting). You can specify values for `parse` and `link_names` to change formatting behavior.

When POSTing with `application/x-www-form-urlencoded` data, the optional `attachments` argument should contain a JSON-encoded array of attachments. Make it easy on yourself and send your entire messages as `application/json` instead.

For more information, see the [attachments spec](/docs/message-attachments). If you're using a [Slack app](/slack-apps), you can also use this method to attach [message buttons](/docs/message-buttons).

By default links to media are unfurled, but links to text content are not. For more information on the differences and how to control this, see the [the unfurling documentation](/docs/message-attachments#unfurling).

Use the [**Message Builder**](/docs/messages/builder) to preview your message formatting and attachments in real time! It's easy to translate your JSON examples to the parameters understood by `chat.postMessage`.

For best results, limit the number of characters in the `text` field to 4,000 characters. Ideally, messages should be short and human-readable. Slack will [truncate messages](/changelog/2018-truncating-really-long-messages) containing more than 40,000 characters.

If you need to post longer messages, please consider [uploading a snippet instead](/methods/files.upload).

Consider reviewing our [message guidelines](/docs/message-guidelines), especially if you're using attachments or message buttons.

## Threads and replies

Provide a `thread_ts` value for the posted message to act as a reply to a parent message. Sparingly set `reply_broadcast` to `true` if your reply is important enough for everyone in the channel to receive.

See [message threading](/docs/message-threading) for a more in depth look at message threading.

## Channels

You **must** specify a public channel, private channel, or an IM channel with the `channel` argument. Each one behaves slightly differently based on the authenticated user's permissions and additional arguments:

#### Post to a public channel

You can either pass the channel's name (`#general`) or encoded ID (`C024BE91L`), and the message will be posted to that channel. The channel's ID can be retrieved through the [channels.list](/methods/channels.list) API method.

#### Post to a private group

As long as the authenticated user is a member of the private group, you can either pass the group's name (`secret-group`) or encoded ID (`G012AC86C`), and the message will be posted to that group. The private group's ID can be retrieved through the [groups.list](/methods/groups.list) API method.

#### Post to an IM channel

Warning: here be dragons. Posting to an IM channel is a little more complex depending on the value of `as_user` and the type of token associated with your app.

- If `as_user` is false:
  - Pass the IM channel's ID (`D023BB3L2`) as the value of `channel` to post to that IM channel _as the app, bot, or user associated with the token_. You can change the icon and username that go with the message using the `icon_url` and `username` parameters. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.
- If `as_user` is true (for workspace apps, this is always the case):
  - Pass the IM channel's ID (`D023BB3L2`) or a user's ID (`U0G9QF9C6`) as the value of `channel` to post to that IM channel _as the app, bot, or user associated with the token_. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method. When `as_user` is true, the caller may _not_ manipulate the icon and username on the message. You might receive a `channel_not_found` error if your app doesn't have permission to enter into an IM with the intended user.

To send a direct message to the user _owning_ the token used in the request, provide the `channel` field with a conversation/IM ID value found in a method like [`im.list`](/methods/im.list).

<ts-icon class="ts_icon_info_circle"></ts-icon> We are phasing out support for ambiguously passing a "username" as a `channel` value. Please _always_ use channel-like IDs instead.

## Begin a conversation in a user's App Home

Start a conversation with users in your [App Home](/reference/app-home).

With the `chat:write` scope enabled, call `chat.postMessage` and pass a user's ID (`U0G9QF9C6`) as the value of `channel` to post to that user's App Home channel. You can use their direct message channel ID (as found with `im.open`, for instance) instead.

## Rate limiting

`chat.postMessage` has special [rate limiting](/docs/rate-limits) conditions. It will generally allow an app to post 1 message per second to a specific channel. There are limits governing your app's relationship with the entire workspace above that, limiting posting to several hundred messages per minute. Generous burst behavior is also granted.

## Authorship

The `as_user` parameter _may not_ be used by [new Slack apps](/authentication/basics) (i.e., apps installed using our [V2 of Oauth 2.0](/authentication/oauth-v2)). New Slack apps act on their own behalf, rather than a user's.

For older Slack apps, message authorship and attribution varies. **The rest of this Authorship section applies only to classic Slack apps**.

The best way to control the authorship of a message is to be explicit with the `as_user` parameter.

Otherwise, `chat.postMessage` will guess the most appropriate `as_user` interpretation based on the kind of token you're using.

If `as_user` is not provided at all, then the value is inferred, based on the scopes granted to the caller: If the caller _could_ post with `as_user` passed as `false`, then that is how the method behaves; otherwise, the method behaves as if `as_user` were passed as `true`.

### When `as_user` is false

When the `as_user` parameter is set to `false`, messages are posted as " [`bot_messages`](/events/message/bot_message)", with message authorship attributed to the user name and icons associated with the [Slack App](/slack-apps).

With `as_user` set to `false`, you may also provide a `username` to explicitly specify the bot user's identity for this message, along with `icon_url` or `icon_emoji`.

##### Effect on identity

Token types provide varying default identity values for `username`, `icon_url`, and `icon_emoji`.

- [test tokens](/docs/oauth-test-tokens)
  - inherits the icon and username of the token owner
- [Slack App user token](/slack-apps) with [`chat:write:user`](/docs/oauth-scopes)
  - inherits icon and username of the token owner
- [Slack App bot user token](/bot-users#share_your_bot_user_as_a_slack_app)
  - inherits Slack App's icon and app's bot username

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
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
| `as_user_not_supported` | The `as_user` parameter does not function with workspace apps. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
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

