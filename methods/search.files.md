Searches for files matching a query.

## Facts

| Method URL: | `https://slack.com/api/search.files` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`search:read`](/scopes/search:read)&nbsp; |

 |

* * *

This method returns files matching a search query.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `query` | `pickleface` | Required | Search query. |
| `count` | `20` | Optional, default=20 | Number of items to return per page. |
| `highlight` | `true` | Optional | Pass a value of `true` to enable query highlight markers (see below). |
| `page` | `2` | Optional, default=1 | Page number of results to return. |
| `sort` | `timestamp` | Optional, default=score | Return matches sorted by either `score` or `timestamp`. |
| `sort_dir` | `asc` | Optional, default=desc | Change sort direction to ascending (`asc`) or descending (`desc`). |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The response envelope contains paging and result information:

Typical success response

```
{
    "files": {
        "matches": {
            "0": {
                "channels": {},
                "comments_count": 1,
                "created": 1507850315,
                "deanimate_gif": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_deanimate_gif.png",
                "display_as_bot": false,
                "editable": false,
                "external_type": "",
                "filetype": "gif",
                "groups": {},
                "id": "F7H0D7ZBB",
                "image_exif_rotation": 1,
                "ims": {},
                "is_external": false,
                "is_public": true,
                "mimetype": "image/gif",
                "mode": "hosted",
                "name": "computer.gif",
                "original_h": 313,
                "original_w": 500,
                "permalink": "https://eventsdemo.slack.com/files/U2U85N1RZ/F7H0D7ZBB/computer.gif",
                "permalink_public": "https://slack-files.com/T2U81E2BB-F7H0D7ZBB-85b7f5557e",
                "pretty_type": "GIF",
                "preview": null,
                "public_url_shared": false,
                "reactions": {
                    "0": {
                        "count": 1,
                        "name": "stuck_out_tongue_winking_eye",
                        "users": {
                            "0": "U2U85N1RZ"
                        }
                    }
                },
                "score": "0.38899223746309",
                "size": 1639034,
                "thumb_160": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_160.png",
                "thumb_360": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_360.png",
                "thumb_360_gif": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_360.gif",
                "thumb_360_h": 225,
                "thumb_360_w": 360,
                "thumb_480": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_480.png",
                "thumb_480_gif": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_480.gif",
                "thumb_480_h": 300,
                "thumb_480_w": 480,
                "thumb_64": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_64.png",
                "thumb_80": "https://files.slack.com/files-tmb/T2U81E2BB-F7H0D7ZBB-21624821e6/computer_80.png",
                "timestamp": 1507850315,
                "title": "computer.gif",
                "top_file": false,
                "url_private": "https://files.slack.com/files-pri/T2U81E2BB-F7H0D7ZBB/computer.gif",
                "url_private_download": "https://files.slack.com/files-pri/T2U81E2BB-F7H0D7ZBB/download/computer.gif",
                "user": "U2U85N1RZ",
                "username": ""
            }
        },
        "pagination": {
            "first": 1,
            "last": 3,
            "page": 1,
            "page_count": 1,
            "per_page": 20,
            "total_count": 3
        },
        "paging": {
            "count": 20,
            "page": 1,
            "pages": 1,
            "total": 3
        },
        "total": 3
    },
    "ok": true,
    "query": "computer.gif"
}
```

Typical error response

```
{
    "error": "No query passed",
    "ok": false
}
```

Matches contains a list of [file objects](/types/file).

All search methods support the `highlight` parameter. If specified, the matching query terms will be marked up in the results so that clients may replace them with appropriate highlighting markers (e.g. `<span class="highlight"></span>`). The UTF-8 markers we use are:

```
start: "\xEE\x80\x80"; # U+E000 (private-use)
end : "\xEE\x80\x81"; # U+E001 (private-use)
```

Please note that the max `count` value is `100` and the max `page` value is `100`.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
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

