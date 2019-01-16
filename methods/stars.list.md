Lists stars for a user.

## Facts

| Method URL: | `https://slack.com/api/stars.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`stars:read`](/scopes/stars:read) |
| [user](/docs/token-types#user) | [`stars:read`](/scopes/stars:read) |

 |

* * *

This method lists the items starred by the authed user.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `count` | `20` | Optional, default=100 | Number of items to return per page. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Parameter for pagination. Set `cursor` equal to the `next_cursor` attribute returned by the previous request's `response_metadata`. This parameter is optional, but pagination is mandatory: the default value simply fetches the first "page" of the collection. See [pagination](/docs/pagination) for more details. |
| `limit` | `20` | Optional, default=0 | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the list hasn't been reached. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

The response contains a list of starred items followed by pagination information.

```
{
    "ok": true,
    "items": [
        {
            "type": "message",
            "channel": "C2147483705",
            "message": {...}
        },
        {
            "type": "file",
            "file": { ... }
        }
        {
            "type": "file_comment",
            "file": { ... },
            "comment": { ... }
        }
        {
            "type": "channel",
            "channel": "C2147483705"
        },

    ],
    "response_metadata": {
        "next_cursor": "dGVhbTpDMUg5UkVTR0w="
    }
}
```

Different item types can be starred. Every item in the list has a `type` property, the other property depend on the type of item. The possible types are:

- **`message`** : the item will have a `message` property containing a [message object](/docs/messages)
- **`file`** : this item will have a `file` property containing a [file object](/types/file).
- **`file_comment`** : the item will have a `file` property containing the [file object](/types/file) and a `comment` property containing the file comment.
- **`channel`** : the item will have a `channel` property containing the channel ID.
- **`im`** : the item will have a `channel` property containing the channel ID for this direct message.
- **`group`** : the item will have a `group` property containing the channel ID for the private group.

## Pagination
This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.  
  
Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.  
  
 For apps created after August 7, 2018, this method defaults to cursor-based pagination. Use the `limit` and `cursor` parameters to guarantee your passage into the paradise of cursored pagination.  
  
 See [pagination](/docs/pagination) for more information.  
  

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
| `is_bot` | This method cannot be called by a bot user. |
| `invalid_arguments` | The method was called with invalid arguments. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
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

