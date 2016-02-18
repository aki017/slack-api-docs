This method updates a message in a channel.

## Arguments

This method has the URL `https://slack.com/api/chat.update` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `chat:write:user`) |
| `ts` | `1405894322.002768` | Required | Timestamp of the message to be updated. |
| `channel` | `C1234567890` | Required | Channel containing the message to be updated. |
| `text` | `Hello world` | Required | New text for the message, using the [default formatting rules](/docs/formatting). |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | Structured message attachments. |
| `parse` | `none` | Optional | Change how messages are treated. See below. |
| `link_names` | `1` | Optional | Find and link channel names and usernames. |

## Formatting

The default value for parse will attempt to discover links in text but does not support URL markup. To update messages with URL markup, you must specify `parse=none`. For more information, refer to the [formatting spec](/docs/formatting).

The optional `attachments` argument should contain a JSON-encoded array of attachments. If you do not include an attachments property, a message's previous attachments will remain visible. To remove a previous attachment, include an empty `attachments` array with your request. For more information, see the [attachments spec](/docs/attachments).

## Valid message types

Only messages posted by the authenticated user are able to be updated using this method. This includes regular chat messages, as well as messages containing the `me_message` subtype.

Attempting to update other message types will return a `cant_update_message` error.

## Response

```
{
    "ok": true,
    "channel": "C024BE91L",
    "ts": "1401383885.000061",
    "text": "Updated Text"
}
```

The response includes the `text`, `channel` and `timestamp` properties of the updated message so clients can keep their local copies of the message in sync.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `message_not_found` | No message exists with the requested timestamp. |
| `cant_update_message` | Authenticated user does not have permission to update this message. |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `edit_window_closed` | The message cannot be edited due to the team message edit settings |
| `msg_too_long` | Message text is too long |
| `no_text` | No message text provided |
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

