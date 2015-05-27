This method posts a message to a channel.

## Arguments

This method has the URL `https://slack.com/api/chat.postMessage` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `post`) |
| `channel` | `C1234567890` | Required | Channel to send message to. Can be a public channel, private group or IM channel. Can be an encoded ID, or a name. |
| `text` | `Hello world` | Required | Text of the message to send. See below for an explanation of formatting. |
| `username` | `My Bot` | Optional | Name of bot. |
| `as_user` | `true` | Optional | Pass true to post the message as the authed user, instead of as a bot |
| `parse` | `full` | Optional | Change how messages are treated. See below. |
| `link_names` | `1` | Optional | Find and link channel names and usernames. |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | Structured message attachments. |
| `unfurl_links` | `true` | Optional | Pass true to enable unfurling of primarily text-based content. |
| `unfurl_media` | `false` | Optional | Pass false to disable unfurling of media content. |
| `icon_url` | `http://lorempixel.com/48/48` | Optional | URL to an image to use as the icon for this message |
| `icon_emoji` | `:chart_with_upwards_trend:` | Optional | emoji to use as the icon for this message. Overrides `icon_url`. |

## Formatting

Messages are formatted as described in the [formatting spec](/docs/formatting). You can specify values for `parse` and `link_names` to change formatting behavior.

The optional `attachments` argument should contain a JSON-encoded array of attachments. For more information, see the [attachments spec](/docs/attachments).

By default links to media are unfurled, but links to text content are not. For more information on the differences and how to control this, see the [the unfurling documentation](/docs/unfurling).

By default messages are posted as [bot\_messages](/events/message/bot_message). If `as_user` is true the message is instead posted as the authenticated user. Using the `as_user` argument requires the [client scope](/docs/oauth#auth_scopes). The `username`, `icon_url` and`icon_emoji` arguments are ignored if `as_user` is true.

## Response

```
{
    "ok": true,
    "ts": "1405895017.000506",
    "channel": "C024BE91L",
    "message": {
        …
    }
}
```

The response includes the TS and channel for the posted message. It also includes the complete message object, as it was parsed by our servers. This may differ from the provided arguments as our servers sanitize links, attachments and other properties.

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

