This method allows you to create or upload an existing file.

## Arguments

This method has the URL `https://slack.com/api/files.upload` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `files:write:user`) |
| `file` | `...` | Required | File contents via `multipart/form-data`. |
| `content` | `...` | Optional | File contents via a POST var. |
| `filetype` | `php` | Optional | Slack-internal file type identifier. |
| `filename` | `foo.txt` | Required | Filename of file. |
| `title` | `My File` | Optional | Title of file. |
| `initial_comment` | `Best!` | Optional | Initial comment to add to file. |
| `channels` | `C1234567890` | Optional | Comma-separated list of channel names or IDs where the file will be shared. |

The content of the file can either be posted using an `enctype` of `multipart/form-data` (with the file parameter named `file`), in the usual way that files are uploaded via the browser, or the content of the file can be sent as a POST var called `content`. The latter should be used for creating a "file" from a long message/paste and forces "editable" mode.

In both cases, the type of data in the file will be intuited from the filename and the magic bytes in the file, for supported formats. Sending a `filetype` parameter will override this behavior (if a valid type is sent). Files uploaded via `multipart/form-data` will be stored either in hosted or editable mode, based on certain heuristics (determined type, file size).

The file can also be shared directly into channels on upload, by specifying an optional argument `channels`. If there's more than one channel name or ID in the `channels` string, they should be comma-separated.

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

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `posting_to_general_channel_denied` | An admin has restricted posting to the #general channel. |
| `invalid_channel` | One or more channels supplied are invalid |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

