Uploads or creates a file.

## Facts

| Method URL: | `https://slack.com/api/files.upload` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | [`application/x-www-form-urlencoded`](/web#post_bodies "Learn more about sending requests") |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [workspace](/docs/token-types#workspace) | [`files:write:user`](/scopes/files:write:user) |
| [user](/docs/token-types#user) | [`files:write:user`](/scopes/files:write:user) [`post`](/scopes/post) |

 |

* * *

This method allows you to create or upload an existing file.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channels` | `C1234567890,C2345678901,C3456789012` | Optional | Comma-separated list of channel names or IDs where the file will be shared. |
| `content` | `...` | Optional | File contents via a POST variable. If omitting this parameter, you must provide a `file`. |
| `file` | `...` | Optional | File contents via `multipart/form-data`. If omitting this parameter, you must submit `content`. |
| `filename` | `foo.txt` | Optional | Filename of file. |
| `filetype` | `php` | Optional | A [file type](/types/file#file_types) identifier. |
| `initial_comment` | `Best!` | Optional | Initial comment to add to file. |
| `title` | `My File` | Optional | Title of file. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

**You must provide either a `file` or `content` parameter.**

The content of the file can either be posted using an `enctype` of `multipart/form-data` (with the file parameter named `file`), in the usual way that files are uploaded via the browser, or the content of the file can be sent as a POST var called `content`. The latter should be used for creating a "file" from a long message/paste and forces "editable" mode.

In both cases, the type of data in the file will be intuited from the filename and the magic bytes in the file, for supported formats.

Sending a valid `filetype` parameter will override this behavior. Possible `filetype` values can be found in the [`file` object definition](/types/file#file_types).

Files uploaded via `multipart/form-data` will be stored either in hosted or editable mode, based on certain heuristics (determined type, file size).

The file can also be shared directly into channels on upload, by specifying an optional argument `channels`. If there's more than one channel name or ID in the `channels` string, they should be comma-separated.

There is a 1 megabyte file size limit for files uploaded as snippets.

## Response

If successful, the response will include a [file object](/types/file).

```
{
    "ok": true,
    "file": {...}
}
```

By default all newly-uploaded files are private and only visible to the owner. They become public once they are shared into a public channel (which can happen at upload time via the `channels` argument).

## Examples

Upload "dramacat.gif" and share it in channel, using `multipart/form-data`:

```
curl -F file=@dramacat.gif -F channels=C024BE91L,#general -F token=xxxx-xxxxxxxxx-xxxx https://slack.com/api/files.upload
```

Create an editable file containing the text "Hello":

```
curl -F content="Hello" -F token=xxxx-xxxxxxxxx-xxxx https://slack.com/api/files.upload
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `posting_to_general_channel_denied` | An admin has restricted posting to the #general channel. |
| `invalid_channel` | One or more channels supplied are invalid |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
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

