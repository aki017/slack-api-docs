Find a user with an email address.

## Facts

| Method URL: | `https://slack.com/api/users.lookupByEmail` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | [`application/x-www-form-urlencoded`](/web#post_bodies "Learn more about sending requests") |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | [`users:read.email`](/scopes/users:read.email) |

 |

* * *

Retrieve a single user by looking them up by their registered email address. Requires [`users:read.email`](/scopes/users:read.email).

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `email` | `spengler@ghostbusters.example.com` | Required | An email address belonging to a user in the workspace |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

Returns a [user object](/types/user).

Typical success response

```
{
    "ok": true,
    "user": {
        "id": "W012A3CDE",
        "team_id": "T012AB3C4",
        "name": "spengler",
        "deleted": false,
        "color": "9f69e7",
        "real_name": "episod",
        "tz": "America\/Los_Angeles",
        "tz_label": "Pacific Daylight Time",
        "tz_offset": -25200,
        "profile": {
            "avatar_hash": "ge3b51ca72de",
            "status_text": "Print is dead",
            "status_emoji": ":books:",
            "real_name": "Egon Spengler",
            "display_name": "spengler",
            "real_name_normalized": "Egon Spengler",
            "display_name_normalized": "spengler",
            "email": "spengler@ghostbusters.example.com",
            "image_24": "https:\/\/...\/avatar\/e3b51ca72dee4ef87916ae2b9240df50.jpg",
            "image_32": "https:\/\/...\/avatar\/e3b51ca72dee4ef87916ae2b9240df50.jpg",
            "image_48": "https:\/\/...\/avatar\/e3b51ca72dee4ef87916ae2b9240df50.jpg",
            "image_72": "https:\/\/...\/avatar\/e3b51ca72dee4ef87916ae2b9240df50.jpg",
            "image_192": "https:\/\/...\/avatar\/e3b51ca72dee4ef87916ae2b9240df50.jpg",
            "image_512": "https:\/\/...\/avatar\/e3b51ca72dee4ef87916ae2b9240df50.jpg",
            "team": "T012AB3C4"
        },
        "is_admin": true,
        "is_owner": false,
        "is_primary_owner": false,
        "is_restricted": false,
        "is_ultra_restricted": false,
        "is_bot": false,
        "updated": 1502138686,
        "is_app_user": false,
        "has_2fa": false
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "users_not_found"
}
```

### Profile

The profile hash contains as much information as the user has supplied in the default profile fields: `first_name`, `last_name`, `real_name`, `display_name`, `skype`, and the `image_*` fields. Only the `image_*` fields are guaranteed to be included. Data that has not been supplied may not be present at all, may be null or may contain the empty string ("").

A user's custom profile fields may be discovered using [`users.profile.get`](/methods/users.profile.get).

### Email addresses

Apps created after January 4th, 2017 must explicitly request the `users:read.email` [OAuth permission scope](/docs/oauth-scopes) when using the [OAuth app installation flow](/docs/oauth) to enable access to this method.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `users_not_found` | Value passed for `user` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
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
