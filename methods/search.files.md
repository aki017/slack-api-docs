This method returns files matching a search query.

## Arguments

This method has the URL `https://slack.com/api/search.files` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `read`) |
| `query` | `pickleface` | Required | Search query. May contain booleans, etc. |
| `sort` | `timestamp` | Optional, default=score | Return matches sorted by either `score` or `timestamp`. |
| `sort_dir` | `asc` | Optional, default=desc | Change sort direction to ascending (`asc`) or descending (`desc`). |
| `highlight` | `1` | Optional | Pass a value of `1` to enable query highlight markers (see below). |
| `count` | `100` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

The response envelope contains paging and result information:

```
{
    "ok": true,
    "query": "test",
    "files": {
        "total": 829,
        "paging": {
            "count": 20,
            "total": 829,
            "page": 1,
            "pages": 42
        },
        "matches": [
            {...},
            {...},
            {...}
        ]
    }
}
```

Matches contains a list of [file objects](/types/file).

All search methods support the `highlight` parameter. If specified, the matching query terms will be marked up in the results so that clients may replace them with appropriate highlighting markers (e.g. `<span class="highlight"></span>`). The UTF-8 markers we use are:

```
start: "\xEE\x80\x80"; # U+E000 (private-use)
end : "\xEE\x80\x81"; # U+E001 (private-use)
```

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

