Sends an ephemeral message to a user in a channel.

## Facts

| Method URL: | `https://slack.com/api/chat.postEphemeral` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 4](/docs/rate-limits#tier_t4) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [workspace](/docs/token-types#workspace) | [`chat:write`](/scopes/chat:write) |
| [user](/docs/token-types#user) | [`chat:write:user`](/scopes/chat:write:user) [`chat:write:bot`](/scopes/chat:write:bot) [`post`](/scopes/post) |

 |

* * *

This method posts an ephemeral message, which is visible only to the assigned user in a specific public channel, private channel, or private conversation.

Ephemeral message delivery is not guaranteed — the user must be currently active in Slack and a member of the specified `channel`. By nature, ephemeral messages do not persist across reloads, desktop and mobile apps, or sessions.

Use ephemeral messages to send users context-sensitive messages, relevant to the channel they're detectably participating in. Avoid sending unexpected or unsolicited ephemeral messages.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel, private group, or IM channel to send message to. Can be an encoded ID, or a name. |
| `text` | `Hello world` | Required | Text of the message to send. See below for an explanation of formatting. This field is usually required, unless you're providing only `attachments` instead. |
| `user` | `U0BPQUNTA` | Required | `id` of the user who will receive the ephemeral message. The user should be in the channel specified by the `channel` argument. |
| `as_user` | `true` | Optional | Pass true to post the message as the authed bot. Defaults to false. |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | A JSON-based array of structured attachments, presented as a URL-encoded string. |
| `link_names` | `true` | Optional | Find and link channel names and usernames. |
| `parse` | `full` | Optional | Change how messages are treated. Defaults to `none`. See below. |

<ts-icon class="ts_icon_code"></ts-icon> This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

A message must have either `text` or `attachments` or both. The `text` parameter is required unless you provide `attachments`. Use both parameters in conjunction with each other to create awesome messages.

## Formatting

Messages are formatted as described in the [formatting spec](/docs/message-formatting). You can specify values for `parse` and `link_names` to change formatting behavior.

The optional `attachments` argument should contain a JSON-encoded array of attachments.

For more information, see the [attachments spec](/docs/message-attachments). If you're using a [Slack app](/slack-apps), you can also use this method to attach [message buttons](/docs/message-buttons).

Use the [**Message Builder**](/docs/messages/builder) to preview your message formatting and attachments in real time! It's easy to translate your JSON examples to the parameters understood by `chat.postEphemeral`.

For best results, limit the number of characters in the `text` field to a few thousand bytes at most. Ideally, messages should be short and human-readable, if you need to post longer messages, please consider [uploading a snippet instead](/methods/files.upload). (A single message should be no larger than 4,000 bytes.)

Consider reviewing our [message guidelines](/docs/message-guidelines), especially if you're using attachments or message buttons.

## Authorship

How message authorship is attributed varies by a few factors, with some behaviors varying depending on the kinds of tokens you're using to post a message.

The best way to realize your intended result is to be explicit with the `as_user` parameter.

`chat.postEphemeral` wants your message posting to succeed and may attempt to guess the most appropriate `as_user` interpretation based on the kind of token you're using.

If `as_user` is not provided at all, then the value is inferred, based on the scopes granted to the caller: If the caller _could_ post with `as_user` passed as `false`, then that is how the method behaves; otherwise, the method behaves as if `as_user` were passed as `true`.

### When `as_user` is false

When the `as_user` parameter is set to `false`, messages are posted as " [`bot_messages`](/events/message/bot_message)", with message authorship attributed to the default user name and icons associated with the [Custom Integration](/custom-integrations) or [Slack App](/slack-apps).

### When `as_user` is true

Set `as_user` to `true` and the authenticated user will appear as the author of the message. Posting as the authenticated user **requires** the`client` or the more preferred `chat:write:user` [scopes](/docs/oauth#auth_scopes).

## Channels

You **must** specify a public channel, private channel, or an IM channel with the `channel` argument. Each one behaves slightly differently based on the authenticated user's permissions and additional arguments. If the user specified as the intended recipient is not in the given channel, the ephemeral message will not be delivered, and a `user_not_in_channel` error will be returned. Note that the `user` parameter expects a user's `id`, and not a screen name.

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

To send a direct message to the user _owning_ the token used in the request, provide the `channel` field with the a conversation/IM ID value found in a method like [`im.list`](/methods/im.list).

## Response

Typical success response

```
{
    "ok": true,
    "message_ts": "1502210682.580145"
}
```

Typical error response

```
{
    "ok": false,
    "error": "user_not_in_channel"
}
```

The `message_ts` included with the response _cannot_ be used with [`chat.update`](/methods/chat.update), as it does not represent an actual message written to the database like it does with other api methods.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `is_archived` | Channel has been archived. |
| `msg_too_long` | Message text is too long |
| `no_text` | No message text provided |
| `restricted_action` | A workspace preference prevents the authenticated user from posting. |
| `too_many_attachments` | Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message. |
| `user_not_in_channel` | Intended recipient is not in the specified channel. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
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

