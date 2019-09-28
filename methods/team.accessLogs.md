Gets the access logs for the current team.

## Facts

| Method URL: | `https://slack.com/api/team.accessLogs` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | _No scope required_ |

 |

* * *

This method is used to retrieve the "access logs" for users on a workspace.

Each access log entry represents a user accessing Slack from a specific user, IP address, and user agent combination.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `before` | `1457989166` | Optional, default=now | End of time range of logs to include in results (inclusive). |
| `count` | `20` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The method's paginated response contains a list of access log entries. Each item in the `logins` array represents a number of possible user interactions, collated by the user, IP address, and user agent combination. These include actual logins as well as other API calls that are typically made when accessing Slack.

This response demonstrates pagination and two access log entries.

```
{
    "ok": true,
    "logins": {
        "0": {
            "user_id": "U45678",
            "username": "alice",
            "date_first": 1422922864,
            "date_last": 1422922864,
            "count": 1,
            "ip": "127.0.0.1",
            "user_agent": "SlackWeb Mozilla/5.0 (Macintosh; Intel Mac OS X 10_10_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2272.35 Safari/537.36",
            "isp": "BigCo ISP",
            "country": "US",
            "region": "CA"
        },
        "1": {
            "user_id": "U12345",
            "username": "white_rabbit",
            "date_first": 1422922493,
            "date_last": 1422922493,
            "count": 1,
            "ip": "127.0.0.1",
            "user_agent": "SlackWeb Mozilla/5.0 (iPhone; CPU iPhone OS 8_1_3 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Version/8.0 Mobile/12B466 Safari/600.1.4",
            "isp": "BigCo ISP",
            "country": "US",
            "region": "CA"
        }
    },
    "paging": {
        "count": 100,
        "total": 2,
        "page": 1,
        "pages": 1
    }
}
```

A workspace must be on a paid plan to use this method, otherwise the `paid_only` error is thrown:

```
{
    "ok": false,
    "error": "paid_only"
}
```

Each access log entry contains the user `id` and `username` that accessed Slack.

`date_first` is a unix timestamp of the first access log entry for this user, IP address, and user agent combination.

`date_last` is the most recent for that combination.

`count` is the total number of access log entries for that combination.

`ip` is the IP address of the device used.

`user_agent` is the reported user agent string from the browser or client application.

`isp` is our _best guess_ at the internet service provider owning the IP address.

`country` and `region` are also our best guesses on where the access originated, based on the IP address.

### Pagination

The paging information contains the `count` of items returned, the `total` number of items reacted to, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

This method does not yet support cursored pagination.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `over_pagination_limit` | It is not possible to request more than 1000 items per page or more than 100 pages. |
| `paid_only` | This is only available to paid teams. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `is_bot` | This method cannot be called by a bot user. |
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
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

