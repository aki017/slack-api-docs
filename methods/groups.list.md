Lists private channels that the calling user has access to.

## Facts

| Method URL: | `https://slack.com/api/groups.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 3](/docs/rate-limits#tier_t3) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#granular_bot) | [`groups:read`](/scopes/groups:read)&nbsp; |
| [user](/docs/token-types#user) | [`groups:read`](/scopes/groups:read)&nbsp; |
| [classic&nbsp;bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |

 |

* * *

<ts-icon class="ts_icon_warning"></ts-icon>
**This method is deprecated.** [&nbsp;Learn more](/changelog/2020-01-deprecating-antecedents-to-the-conversations-api).

Please use these methods instead:

- [`conversations.list`](/methods/conversations.list)
- [`users.conversations`](/methods/users.conversations)

This method is replaced by [`conversations.list`](/methods/conversations.list) and should not be used. **Some private channels will not work with this method.**

This legacy method returns a list of private channels in the team that the caller is in and archived groups that the caller was in.

The list of (non-deactivated) members in each private channel is also returned.

Does not include private shared channels or channels that have been converted from public to private. Use the [Conversations API](/docs/conversations-api) instead.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Parameter for pagination. Set `cursor` equal to the `next_cursor` attribute returned by the previous request's `response_metadata`. This parameter is optional, but pagination is mandatory: the default value simply fetches the first "page" of the collection. See [pagination](/docs/pagination) for more details. |
| `exclude_archived` | `true` | Optional, default=0 | Don't return archived private channels. |
| `exclude_members` | `true` | Optional, default=false | Exclude the `members` from each `group` |
| `limit` | `20` | Optional, default=0 | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the list hasn't been reached. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a list of [group objects](/types/group) (also known as "private channel objects"):

```
{
    "ok": true,
    "groups": [
        {
            "id": "G024BE91L",
            "name": "secretplans",
            "created": 1360782804,
            "creator": "U024BE7LH",
            "is_archived": false,
            "members": [
                "U024BE7LH"
            ],
            "topic": {
                "value": "Secret plans on hold",
                "creator": "U024BE7LV",
                "last_set": 1369677212
            },
            "purpose": {
                "value": "Discuss secret plans that no-one else should know",
                "creator": "U024BE7LH",
                "last_set": 1360782804
            }
        },
        ....
    ],
    "response_metadata": {
        "next_cursor": "dGVhbTpDMUg5UkVTR0w="
    }
}
```

## Pagination
This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.  
  
Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.  
  
 See [pagination](/docs/pagination) for more information.  
  

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
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

