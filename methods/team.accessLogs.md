This method is used to get the access logs for users on a team.

## Arguments

This method has the URL `https://slack.com/api/team.accessLogs` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `admin`) |
| `count` | `100` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

The response contains a list of logins followed by pagination information.

```
{
    "ok": true,
    "logins": [
        {
            "user_id": "U12345",
            "username": "bob",
            "date_first": 1422922864,
            "date_last": 1422922864,
            "count": 1,
            "ip": "127.0.0.1",
            "user_agent": "SlackWeb Mozilla\/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit\/537.36 (KHTML, like Gecko) Chrome\/41.0.2272.35 Safari\/537.36",
            "isp": "BigCo ISP",
            "country": "US",
            "region": "CA"
        },
        {
            "user_id": "U45678",
            "username": "alice",
            "date_first": 1422922493,
            "date_last": 1422922493,
            "count": 1,
            "ip": "127.0.0.1",
            "user_agent": "SlackWeb Mozilla\/5.0 (iPhone; CPU iPhone OS 8_1_3 like Mac OS X) AppleWebKit\/600.1.4 (KHTML, like Gecko) Version\/8.0 Mobile\/12B466 Safari\/600.1.4",
            "isp": "BigCo ISP",
            "country": "US",
            "region": "CA"
        },
    ],
    "paging": {
        "count": 100,
        "total": 2,
        "page": 1,
        "pages": 1
    }
}
```

Each login contains the user id and username that logged in. `date_first` is a unix timestamp of the first login for this user/ip/user\_agent combination.`date_last` is the most recent for that combination. `count` is the total number of logins for that combination. `ip` is the ip address of the device used to login. `user_agent` is the reported user agent string from the browser or client application. `isp` is our best guess at the internet service provider who owns the ip address. `country` and `region` are similarly where we think that login came from, based on the ip address.

The paging information contains the `count` of items returned, the `total` number of items reacted to, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `paid_only` | This is only available to paid teams. |
| `over_pagination_limit` | It is not possible to request more than 1000 items per page or more than 100 pages. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

