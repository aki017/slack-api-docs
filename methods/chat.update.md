Updates a message.

## Facts

| Method URL: | `https://slack.com/api/chat.update` |
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

This method updates a message in a channel. Though related to [`chat.postMessage`](/methods/chat.postMessage), some parameters of `chat.update` are handled differently.

Ephemeral messages created by [`chat.postEphemeral`](/methods/chat.postEphemeral) or otherwise cannot be updated with this method.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Required | Channel containing the message to be updated. |
| `text` | `Hello world` | Required | New text for the message, using the [default formatting rules](/docs/formatting). It's not required when presenting `attachments`. |
| `ts` | `1405894322.002768` | Required | Timestamp of the message to be updated. |
| `as_user` | `true` | Optional | Pass true to update the message as the authed user. [Bot users](/bot-users) in this context are considered authed users. |
| `attachments` | `[{"pretext": "pre-hello", "text": "text-world"}]` | Optional | A JSON-based array of structured attachments, presented as a URL-encoded string. This field is required when not presenting `text`. |
| `blocks` | `[{"type": "section", "text": {"type": "plain_text", "text": "Hello world"}}]` | Optional | A JSON-based array of structured blocks, presented as a URL-encoded string. |
| `link_names` | `true` | Optional | Find and link channel names and usernames. Defaults to `none`. See below. |
| `parse` | `none` | Optional | Change how messages are treated. Defaults to `client`, unlike `chat.postMessage`. See below. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Formatting

By default, Slack will attempt to discover links in text but does not support URL markup. To update messages with URL markup, you must specify `link_names=1` or `parse=none`. Names and channels will not be linkified in the `@username` or `#channel` format, unless you pass `link_names=1` or `parse=full`. For more information, refer to the [formatting spec](/docs/formatting).

If you used `link_names` or `parse` arguments in the original message, you should specify them when calling `chat.update` as well. Otherwise, the values you passed initially will be overwritten with this method’s defaults (`link_names=none`, `parse=client`).

The optional `attachments` argument should contain a JSON-encoded array of attachments. If you do not include an attachments property, a message's previous attachments will remain visible. To remove a previous attachment, include an empty `attachments` array with your request. For more information, see the [attachments spec](/docs/attachments).

## Valid message types

Only messages posted by the authenticated user are able to be updated using this method. This includes regular chat messages, as well as messages containing the `me_message` subtype. [Bot users](/bot-users) may also update the messages they post.

Attempting to update other message types will return a `cant_update_message` error.

## Response

Typical success response

```
{
    "ok": true,
    "channel": "C024BE91L",
    "ts": "1401383885.000061",
    "text": "Updated text you carefully authored"
}
```

Typical error response

```
{
    "ok": false,
    "error": "cant_update_message"
}
```

The response includes the `text`, `channel` and `timestamp` properties of the updated message so clients can keep their local copies of the message in sync.

## Bot users

To use `chat.update` with a [bot user](/bot-users) token, you'll need to _think of your bot user as a user_, and pass `as_user` set to `true` while editing a message created by that same bot user.

### Interactive messages with buttons

If you're posting [message with buttons](/docs/message-buttons), you may use `chat.update` to continue updating ongoing state changes around a message. Provide the `ts` field the message you're updating and follow the bot user instructions above to update message text, remove or add attachments and actions.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `message_not_found` | No message exists with the requested timestamp. |
| `cant_update_message` | Authenticated user does not have permission to update this message. |
| `channel_not_found` | Value passed for `channel` was invalid. |
| `edit_window_closed` | The message cannot be edited due to the team message edit settings |
| `msg_too_long` | Message text is too long |
| `too_many_attachments` | Too many attachments were provided with this message. A maximum of 100 attachments are allowed on a message. |
| `no_text` | No message text provided |
| `as_user_not_supported` | The `as_user` parameter does not function with workspace apps. |
| `is_inactive` | The message cannot be edited within a frozen, archived or deleted channel. |
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

