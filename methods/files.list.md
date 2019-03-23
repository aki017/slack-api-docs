## Facts

| Method URL: | `https://slack.com/api/files.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`files:read`](/scopes/files:read)&nbsp; |

 |

* * *

<ts-icon class="ts_icon_all_files"></ts-icon> **File threads are here**  

A new file commenting experience arrived on July 23, 2018. [**Learn more**](/changelog/2018-05-file-threads-soon-tread) about what's new and the migration path for apps already working with files and file comments.

This method returns a list of files within the team. It can be filtered and sliced in various ways.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel` | `C1234567890` | Optional | Filter files appearing in a specific channel, indicated by its ID. |
| `count` | `20` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |
| `ts_from` | `123456789` | Optional, default=0 | Filter files created after this timestamp (inclusive). |
| `ts_to` | `123456789` | Optional, default=now | Filter files created before this timestamp (inclusive). |
| `types` | `images` | Optional, default=all | Filter files by type (see below). You can pass multiple values in the types argument, like `types=spaces,snippets`.The default value is `all`, which does not filter the list. |
| `user` | `W1234567890` | Optional | Filter files created by a single user. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Typical success response

```
{
    "ok": true,
    "files": [
        {
            "id": "F0S43P1CZ",
            "created": 1531763254,
            "timestamp": 1531763254,
            "name": "billair.gif",
            "title": "billair.gif",
            "mimetype": "image/gif",
            "filetype": "gif",
            "pretty_type": "GIF",
            "user": "U061F7AUR",
            "editable": false,
            "size": 144538,
            "mode": "hosted",
            "is_external": false,
            "external_type": "",
            "is_public": true,
            "public_url_shared": false,
            "display_as_bot": false,
            "username": "",
            "url_private": "https://.../billair.gif",
            "url_private_download": "https://.../billair.gif",
            "thumb_64": "https://.../billair_64.png",
            "thumb_80": "https://.../billair_80.png",
            "thumb_360": "https://.../billair_360.png",
            "thumb_360_w": 176,
            "thumb_360_h": 226,
            "thumb_160": "https://.../billair_=_160.png",
            "thumb_360_gif": "https://.../billair_360.gif",
            "image_exif_rotation": 1,
            "original_w": 176,
            "original_h": 226,
            "deanimate_gif": "https://.../billair_deanimate_gif.png",
            "pjpeg": "https://.../billair_pjpeg.jpg",
            "permalink": "https://https://.../billair.gif",
            "permalink_public": "https://.../...",
            "channels": [
                "C0T8SE4AU"
            ],
            "groups": [],
            "ims": [],
            "comments_count": 0
        },
        {
            "id": "F0S43PZDF",
            "created": 1531763342,
            "timestamp": 1531763342,
            "name": "tedair.gif",
            "title": "tedair.gif",
            "mimetype": "image/gif",
            "filetype": "gif",
            "pretty_type": "GIF",
            "user": "U061F7AUR",
            "editable": false,
            "size": 137531,
            "mode": "hosted",
            "is_external": false,
            "external_type": "",
            "is_public": true,
            "public_url_shared": false,
            "display_as_bot": false,
            "username": "",
            "url_private": "https://.../tedair.gif",
            "url_private_download": "https://.../tedair.gif",
            "thumb_64": "https://.../tedair_64.png",
            "thumb_80": "https://.../tedair_80.png",
            "thumb_360": "https://.../tedair_360.png",
            "thumb_360_w": 176,
            "thumb_360_h": 226,
            "thumb_160": "https://.../tedair_=_160.png",
            "thumb_360_gif": "https://.../tedair_360.gif",
            "image_exif_rotation": 1,
            "original_w": 176,
            "original_h": 226,
            "deanimate_gif": "https://.../tedair_deanimate_gif.png",
            "pjpeg": "https://.../tedair_pjpeg.jpg",
            "permalink": "https://https://.../tedair.gif",
            "permalink_public": "https://.../...",
            "channels": [
                "C0T8SE4AU"
            ],
            "groups": [],
            "ims": [],
            "comments_count": 0
        }
    ],
    "paging": {
        "count": 100,
        "total": 2,
        "page": 1,
        "pages": 1
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

The response contains a list of [file objects](/types/file), followed by paging information.

In order to gather information on [tombstoned files](/changelog/2019-03-wild-west-for-files-no-more) in Free workspaces, so that you can delete or revoke them, pass the `show_files_hidden_by_limit` parameter. While the yielded files will still be redacted, you'll gain the `id` of the files so that you can delete or revoke them.

The paging information contains the `count` of files returned, the `total` number of files matching the filter (if any was supplied), the `page` of results returned in this response and the total number of `pages` available.

If cursor paging is used, a `nextCursor` argument will be returned instead of the normal paging information such as `count`, `total`, `page`, and `pages`. It will be up to the client to keep track of that information.

### Types

The file types you may encounter include (but are not limited to):

- `all` - All files
- `spaces` - Posts
- `snippets` - Snippets
- `images` - Image files
- `gdocs` - Google docs
- `zips` - Zip files
- `pdfs` - PDF files

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid |
| `unknown_type` | Value passed for `types` was invalid |
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

