This method returns a list of files within the team. It can be filtered and sliced in various ways.

## Arguments

This method has the URL `https://slack.com/api/files.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `files:read` |
| `user` | `U1234567890` | Optional | Filter files created by a single user. |
| `channel` | `C1234567890` | Optional | Filter files appearing in a specific channel, indicated by its ID. |
| `ts_from` | `123456789` | Optional, default=0 | Filter files created after this timestamp (inclusive). |
| `ts_to` | `123456789` | Optional, default=now | Filter files created before this timestamp (inclusive). |
| `types` | `images` | Optional, default=all | Filter files by type:
- `all` - All files
- `spaces` - Posts
- `snippets` - Snippets
- `images` - Image files
- `gdocs` - Google docs
- `zips` - Zip files
- `pdfs` - PDF files
You can pass multiple values in the types argument, like `types=spaces,snippets`.The default value is `all`, which does not filter the list. |
| `count` | `20` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

```
{
    "ok": true,
    "files": [
        {...},
        {...},
        {...},
        ...
    ],
    "paging": {
        "count": 100,
        "total": 295,
        "page": 1,
        "pages": 3
    }
}
```

The response contains a list of [file objects](/types/file), followed by paging information. Files are always returned with the most recent first.

The paging information contains the `count` of files returned, the `total` number of files matching the filter (if any was supplied), the `page` of results returned in this response and the total number of `pages` available.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid |
| `unknown_type` | Value passed for `types` was invalid |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
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

