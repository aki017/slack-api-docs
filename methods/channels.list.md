This method returns a list of all channels in the team. This includes channels the caller is in, channels they are not currently in, and archived channels but does not include private channels. The number of (non-deactivated) members in each channel is also returned.

To retrieve a list of private channels, use [`groups.list`](/methods/groups.list).

_Having trouble getting a HTTP 200 response from this method?_ Try excluding the `members` list from each channel object using the `exclude_members` parameter.

## Arguments

This method has the URL `https://slack.com/api/channels.list` and follows the [Slack Web API calling conventions](/web#basics). <aside class="small">Present these parameters as part of an <code>application/x-www-form-urlencoded</code> querystring or POST body. <code>application/json</code> is not currently accepted.</aside>

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `channels:read` |
| `exclude_archived` | `true` | Optional, default=false | Exclude archived channels from the list |
| `exclude_members` | `true` | Optional, default=false | Exclude the `members` collection from each `channel` |

## Response

Returns a list of limited [channel objects](/types/channel):

```
{
    "ok": true,
    "channels": [
        {
            "id": "C024BE91L",
            "name": "fun",
            "created": 1360782804,
            "creator": "U024BE7LH",
            "is_archived": false,
            "is_member": false,
            "num_members": 6,
            "topic": {
                "value": "Fun times",
                "creator": "U024BE7LV",
                "last_set": 1369677212
            },
            "purpose": {
                "value": "This channel is for fun",
                "creator": "U024BE7LH",
                "last_set": 1360782804
            }
        },
        ....
    ]
}
```

To get a full [channel object](/types/channel), call the [`channels.info`](/methods/channels.info) method.

Use the `exclude_members` parameter to exclude the `members` collection from each listed channel. This improves performance, especially with larger teams. Use [`channels.info`](/methods/channels.info) to retrieve `members` on a channel-by-channel basis instead.

An `is_org_shared` attribute may appear set to `true` on channels that are shared channel between multiple teams of an [enterprise grid](/enterprise-grid). See the [enterprise grid shared channels documentation](/enterprise-grid#shared_channels) for more detail.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The team associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

