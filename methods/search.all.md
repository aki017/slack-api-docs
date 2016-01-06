This method allows to to search both messages and files in a single call.

## Arguments

This method has the URL `https://slack.com/api/search.all` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `search:read`) |
| `query` | `pickleface` | Required | Search query. May contains booleans, etc. |
| `sort` | `timestamp` | Optional, default=score | Return matches sorted by either `score` or `timestamp`. |
| `sort_dir` | `asc` | Optional, default=desc | Change sort direction to ascending (`asc`) or descending (`desc`). |
| `highlight` | `1` | Optional | Pass a value of `1` to enable query highlight markers (see below). |
| `count` | `20` | Optional, default=20 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

The response returns matches broken down by their type of content, similar to the facebook/gmail auto-completed search widgets.

```
{
    "ok": true,
    "query": "Best Pickles",
    "messages": {...},
    "files": {...}
}
```

Within each content group, data is returned in the following format:

```
{
    "matches": [],
    "paging": {
        "count": 100, - number of records per page
        "total": 15, - total records matching query
        "page": 1, - page of records returned
        "pages": 1 - total pages matching query
    }
}
```

This block gives the (estimated) total number of matches of this type, then has an array containing the specified page of the top matches. The format of matches depends on the match type, as described in the documentation for [search.messages](/methods/search.messages) and [search.files](/methods/search.files). These methods can be used to fetch further pages of messages or files.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

