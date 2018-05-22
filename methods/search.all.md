Searches for messages and files matching a query.

## Facts

| Method URL: | `https://slack.com/api/search.all` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`search:read`](/scopes/search:read) [`read`](/scopes/read) |

 |

* * *

This method allows users and applications to search both messages and files in a single call.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `query` | `pickleface` | Required | Search query. May contains booleans, etc. |
| `count` | `20` | Optional, default=20 | Number of items to return per page. |
| `highlight` | `true` | Optional | Pass a value of `true` to enable query highlight markers (see below). |
| `page` | `2` | Optional, default=1 | Page number of results to return. |
| `sort` | `timestamp` | Optional, default=score | Return matches sorted by either `score` or `timestamp`. |
| `sort_dir` | `asc` | Optional, default=desc | Change sort direction to ascending (`asc`) or descending (`desc`). |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The response returns matches broken down by their type of content, similar to the facebook/gmail auto-completed search widgets.

Typical success response

```
{
    "files": {
        "matches": [
            {
                "channels": [],
                "comments_count": 1,
                "created": 1508804330,
                "display_as_bot": false,
                "editable": false,
                "external_type": "",
                "filetype": "png",
                "groups": [],
                "id": "F7PKF1NR7",
                "image_exif_rotation": 1,
                "ims": [],
                "initial_comment": {
                    "comment": "Sure! Here's the workflow diagram!",
                    "created": 1508804330,
                    "id": "Fc7NLL52E7",
                    "is_intro": true,
                    "timestamp": 1508804330,
                    "user": "U2U85N1RZ"
                },
                "is_external": false,
                "is_public": true,
                "mimetype": "image/png",
                "mode": "hosted",
                "name": "slack workflow diagram.png",
                "original_h": 117,
                "original_w": 128,
                "permalink": "https://example.slack.com/files/U2U85N1RZ/F7PKF1NR7/slack_workflow_diagram.png",
                "permalink_public": "https://slack-files.com/T2U81E2FZ-F7PKF1NR7-bea9143f18",
                "pretty_type": "PNG",
                "preview": null,
                "public_url_shared": false,
                "score": "0.99982661240974",
                "size": 35705,
                "thumb_160": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_160.png",
                "thumb_360": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_360.png",
                "thumb_360_h": 117,
                "thumb_360_w": 128,
                "thumb_64": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_64.png",
                "thumb_80": "https://files.slack.com/files-tmb/T2U81E2FZ-F7PKF1NR7-19f33fc256/slack_workflow_diagram_80.png",
                "timestamp": 1508804330,
                "title": "slack workflow diagram",
                "top_file": false,
                "url_private": "https://files.slack.com/files-pri/T2U81E2FZ-F7PKF1NR7/slack_workflow_diagram.png",
                "url_private_download": "https://files.slack.com/files-pri/T2U81E2FZ-F7PKF1NR7/download/slack_workflow_diagram.png",
                "user": "U2U85N1RZ",
                "username": "amy"
            }
        ],
        "pagination": {
            "first": 1,
            "last": 1,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 1
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 1
        },
        "total": 1
    },
    "messages": {
        "matches": [
            {
                "channel": {
                    "id": "C2U86NC6M",
                    "is_ext_shared": false,
                    "is_mpim": false,
                    "is_org_shared": false,
                    "is_pending_ext_shared": false,
                    "is_private": false,
                    "is_shared": false,
                    "name": "general",
                    "pending_shared": []
                },
                "iid": "35692677-e60e-43d9-ac45-1987cea88975",
                "next": {
                    "iid": "6f510ea1-e1d3-4f3f-bdb9-f9c6f6e9d609",
                    "text": "Thanks!",
                    "ts": "1508804378.000219",
                    "type": "message",
                    "user": "U2U85HJ7R",
                    "username": "john"
                },
                "permalink": "https://example.slack.com/archives/C2U86NC6M/p1508804330000296",
                "previous": {
                    "iid": "aba8603c-0543-4fb2-9118-a5ac85f3d138",
                    "text": "Can you send me the Slack workflow diagram?",
                    "ts": "1508804301.000026",
                    "type": "message",
                    "user": "U2U85HJ7R",
                    "username": "john"
                },
                "team": "T2U81E2FZ",
                "text": "uploaded a file: <https://example.slack.com/files/U2U85N1RZ/F7PKF1NR7/slack_workflow_diagram.png|slack workflow diagram> and commented: Sure! Here's the workflow diagram!",
                "ts": "1508804330.000296",
                "type": "message",
                "user": "U2U85N1RZ",
                "username": "amy"
            }
        ],
        "pagination": {
            "first": 1,
            "last": 1,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 1
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 1
        },
        "total": 1
    },
    "ok": true,
    "posts": {
        "matches": [],
        "total": 0
    },
    "query": "diagram"
}
```

Typical error response

```
{
    "error": "missing_scope",
    "needed": "search:read",
    "ok": false,
    "provided": "identify,bot:basic"
}
```

Within each content group, data is returned in the following format:

```
{
    "matches": [],
    "paging": {
        "count": 100,
        "total": 15,
        "page": 1,
        "pages": 1
    }
}
```

- `count` - number of records per page
- `total` - total records matching query
- `page` - page of records returned
- `pages` - total pages matching query

This block gives the (estimated) total number of matches of this type, then has an array containing the specified page of the top matches. The format of matches depends on the match type, as described in the documentation for [search.messages](/methods/search.messages) and [search.files](/methods/search.files). These methods can be used to fetch further pages of messages or files.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `user_is_bot` | This method cannot be called by a bot user. |
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

