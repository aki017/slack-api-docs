This method returns a list of files within the team. It can be filtered and sliced in various ways.

## Arguments

This method has the URL `https://slack.com/api/files.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `files:read`) |
| `user` | `U1234567890` | Optional | Filter files created by a single user. |
| `ts_from` | `123456789` | Optional, default=0 | Filter files created after this timestamp (inclusive). |
| `ts_to` | `123456789` | Optional, default=now | Filter files created before this timestamp (inclusive). |
| `types` | `images` | Optional, default=all | Filter files by type:
- `all` - All files
- `posts` - Posts
- `snippets` - Snippets
- `images` - Image files
- `gdocs` - Google docs
- `zips` - Zip files
- `pdfs` - PDF files
You can pass multiple values in the types argument, like `types=posts,snippets`.The default value is `all`, which does not filter the list. |
| `count` | `100` | Optional, default=100 | Number of items to return per page. |
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

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid |
| `unknown_type` | Value passed for `types` was invalid |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

