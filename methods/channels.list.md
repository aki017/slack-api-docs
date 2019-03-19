## Facts

| Method URL: | `https://slack.com/api/channels.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`channels:read`](/scopes/channels:read)&nbsp; |

 |

* * *

Don't use this method. Use [`conversations.list`](/methods/conversations.list) instead.

This legacy method returns a list of all channels in the team. This includes channels the caller is in, channels they are not currently in, and archived channels but does not include private channels. The number of (non-deactivated) members in each channel is also returned.

To retrieve a list of private channels, use [`conversations.list`](/methods/conversations.list).

[Shared channels](/shared-channels) and channels that have converted from public to private or back again are not returned by this method. Use the [Conversations API](/docs/conversations-api) methods instead, like [`conversations.list`](/methods/conversations.list).

_Having trouble getting a HTTP 200 response from this method?_ Try excluding the `members` list from each channel object using the `exclude_members` parameter.

The `members` array found in this and other methods will begin automatically truncating at 1,500 and eventually fewer results beginning December 1, 2017. As of March, 2018 the cap is 500. Please Use [`conversations.members`](/methods/conversations.members) to manage memberships instead. [Read on to learn more.](/changelog/2017-10-members-array-truncating)

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `dXNlcjpVMDYxTkZUVDI=` | Optional | Paginate through collections of data by setting the `cursor` parameter to a `next_cursor` attribute returned by a previous request's `response_metadata`. Default value fetches the first "page" of the collection. See [pagination](/docs/pagination) for more detail. |
| `exclude_archived` | `true` | Optional, default=false | Exclude archived channels from the list |
| `exclude_members` | `true` | Optional, default=false | Exclude the `members` collection from each `channel` |
| `limit` | `20` | Optional, default=0 | The maximum number of items to return. Fewer than the requested number of items may be returned, even if the end of the users list hasn't been reached. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a list of limited [channel objects](/types/channel).

Typical cursored success response

```
{
    "ok": true,
    "channels": [
        {
            "id": "C0G9QF9GW",
            "name": "random",
            "is_channel": true,
            "created": 1449709280,
            "creator": "U0G9QF9C6",
            "is_archived": false,
            "is_general": false,
            "name_normalized": "random",
            "is_shared": false,
            "is_org_shared": false,
            "is_member": true,
            "is_private": false,
            "is_mpim": false,
            "members": [
                "U0G9QF9C6",
                "U0G9WFXNZ"
            ],
            "topic": {
                "value": "Other stuff",
                "creator": "U0G9QF9C6",
                "last_set": 1449709352
            },
            "purpose": {
                "value": "A place for non-work-related flimflam, faffing, hodge-podge or jibber-jabber you'd prefer to keep out of more focused work-related channels.",
                "creator": "",
                "last_set": 0
            },
            "previous_names": [],
            "num_members": 2
        },
        {
            "id": "C0G9QKBBL",
            "name": "general",
            "is_channel": true,
            "created": 1449709280,
            "creator": "U0G9QF9C6",
            "is_archived": false,
            "is_general": true,
            "name_normalized": "general",
            "is_shared": false,
            "is_org_shared": false,
            "is_member": true,
            "is_private": false,
            "is_mpim": false,
            "members": [
                "U0G9QF9C6",
                "U0G9WFXNZ"
            ],
            "topic": {
                "value": "Talk about anything!",
                "creator": "U0G9QF9C6",
                "last_set": 1449709364
            },
            "purpose": {
                "value": "To talk about anything!",
                "creator": "U0G9QF9C6",
                "last_set": 1449709334
            },
            "previous_names": [],
            "num_members": 2
        }
    ],
    "response_metadata": {
        "next_cursor": "dGVhbTpDMUg5UkVTR0w="
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

To get a full [channel object](/types/channel), call the [`channels.info`](/methods/channels.info) method.

Use the `exclude_members` parameter to exclude the `members` collection from each listed channel. This improves performance, especially with larger teams. Use [`channels.info`](/methods/channels.info) to retrieve `members` on a channel-by-channel basis instead.

An `is_org_shared` attribute may appear set to `true` on channels that are shared channel between multiple teams of an [enterprise grid](/enterprise-grid). See the [enterprise grid shared channels documentation](/enterprise-grid#shared_channels) for more detail.

## Pagination
This method uses cursor-based pagination to make it easier to incrementally collect information. To begin pagination, specify a `limit` value under `1000`. We recommend no more than `200` results at a time.  
  
Responses will include a top-level `response_metadata` attribute containing a `next_cursor` value. By using this value as a `cursor` parameter in a subsequent request, along with `limit`, you may navigate through the collection page by virtual page.  
  
 For apps created after August 7, 2018, this method defaults to cursor-based pagination. Use the `limit` and `cursor` parameters to guarantee your passage into the paradise of cursored pagination.  
  
 See [pagination](/docs/pagination) for more information.  
  

Please note that the argument for the method, `exclude_members` may not work well with pagination at this time. Channels with very large memberships and teams with many channels may cause the method to throw a HTTP 500 error. Please exclude memberships with `exclude_members` and look them up atomically with `channels.info` instead.

* * *

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
| `member_list_truncated` | A `members` array included in a channel object was truncated at 1500 results. Use `conversations.members` to retrieve all results. |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

