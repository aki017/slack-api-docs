This method posts a message to a public channel, private group, or IM channel.

## Arguments

This method has the URL `` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `chat:write:bot` or `chat:write:user`) |
| `channel` | `C1234567890` | Required | Channel, private group, or IM channel to send message to. Can be an encoded ID, or a name. See below for more details. |
| `text` | `Hello world` | Required | Text of the message to send. See below for an explanation of formatting. |
| `parse` | `full` | Optional | Change how messages are treated. Defaults to `none`. See below. |
| `link_names` | `1` | Optional | Find and link channel names and usernames. |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | Structured message attachments. |
| `unfurl_links` | `true` | Optional | Pass true to enable unfurling of primarily text-based content. |
| `unfurl_media` | `false` | Optional | Pass false to disable unfurling of media content. |
| `username` | `My Bot` | Optional | Set your bot's user name. Must be used in conjunction with `as_user` set to false, otherwise ignored. See authorship below. |
| `as_user` | `true` | Optional | Pass true to post the message as the authed user, instead of as a bot. Defaults to false. See authorship below. |
| `icon_url` | `http://lorempixel.com/48/48` | Optional | URL to an image to use as the icon for this message. Must be used in conjunction with `as_user` set to false, otherwise ignored. See authorship below. |
| `icon_emoji` | `:chart_with_upwards_trend:` | Optional | emoji to use as the icon for this message. Overrides `icon_url`. Must be used in conjunction with `as_user` set to false, otherwise ignored. See authorship below. |

## Formatting

Messages are formatted as described in the [formatting spec](/docs/formatting). You can specify values for `parse` and `link_names` to change formatting behavior.

The optional `attachments` argument should contain a JSON-encoded array of attachments. For more information, see the [attachments spec](/docs/attachments).

By default links to media are unfurled, but links to text content are not. For more information on the differences and how to control this, see the [the unfurling documentation](/docs/unfurling).

Use the [**Message Builder**](/docs/formatting/builder) to preview your message formatting and attachments in real time! It's easy to translate your JSON examples to the parameters understood by `chat.postMessage`.

## Authorship

By default, the `as_user` parameter is set to false and messages are posted as [bot\_messages](/events/message/bot_message), with message authorship attributed to the default user name and icons associated with the [Custom Integration](/custom-integrations) or [Slack App](/slack-apps).

With `as_user` set to false, you may also provide a `username` to explicitly specify the bot user's identity for this message, along with `icon_url` or `icon_emoji`.

Set `as_user` to `true` and the authenticated user will appear as the author of the message, ignoring any values provided for `username`, `icon_url`, and `icon_emoji`. Posting as the authenticated user **requires** the`client` or `chat:write:user` [scopes](/docs/oauth#auth_scopes).

## Channels

You **must** specify a public channel, private group, or an IM channel with the `channel` argument. Each one behaves slightly differently based on the authenticated user's permissions and additional arguments:

#### Post to a public channel

You can either pass the channel's name (`#general`) or encoded ID (`C024BE91L`), and the message will be posted to that channel. The channel's ID can be retrieved through the [channels.list](/methods/channels.list) API method.

#### Post to a private group

As long as the authenticated user is a member of the private group, you can either pass the group's name (`secret-group`) or encoded ID (`G012AC86C`), and the message will be posted to that group. The private group's ID can be retrieved through the [groups.list](/methods/groups.list) API method.

#### Post to an IM channel

Posting to an IM channel is a little more complex depending on the value of `as_user`.

- If `as_user` is false:
  - Pass a username (`@chris`) as the value of `channel` to post to that user's @slackbot channel _as the bot_.
  - Pass the IM channel's ID (`D023BB3L2`) as the value of `channel` to post to that IM channel _as the bot_. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.
- If `as_user` is true:
  - Pass the IM channel's ID (`D023BB3L2`) as the value of `channel` to post to that IM channel _as the authenticated user_. The IM channel's ID can be retrieved through the [im.list](/methods/im.list) API method.

## Response

```
{
    "ok": true,
    "ts": "1405895017.000506",
    "channel": "C024BE91L",
    "message": {
        ...
    }
}
```

The response includes the timestamp (`ts`) and channel for the posted message. It also includes the complete message object, as it was parsed by our servers. This may differ from the provided arguments as our servers sanitize links, attachments and other properties.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `not_in_channel` | Cannot post user messages to a channel they are not in. |
| `is_archived` | Channel has been archived. |
| `msg_too_long` | Message text is too long |
| `no_text` | No message text provided |
| `rate_limited` | Application has posted too many messages, [read the Rate Limit documentation](/docs/rate-limits) for more information |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

