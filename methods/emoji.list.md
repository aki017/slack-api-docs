This method lists the custom emoji for a team.

## Arguments

This method has the URL `https://slack.com/api/emoji.list` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `read`) |

## Response

```
{
    "ok": true,
    "emoji": {
        "bowtie": "https:\/\/my.slack.com\/emoji\/bowtie\/46ec6f2bb0.png",
        "squirrel": "https:\/\/my.slack.com\/emoji\/squirrel\/f35f40c0e0.png",
        "shipit": "alias:squirrel",
        …
    }
}
```

The `emoji` property contains a map of name/url pairs, one for each custom emoji used by the team. The `alias:` pseudo-protocol will be used where the emoji is an alias, the string following the colon is the name of the other emoji this emoji is an alias to.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |

