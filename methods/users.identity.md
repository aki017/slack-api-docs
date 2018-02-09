Get a user's identity.

## Facts

| Method URL: | `https://slack.com/api/users.identity` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`identity.basic`](/scopes/identity.basic) |
| [user](/docs/token-types#user) | [`identity.basic`](/scopes/identity.basic) |

 |

* * *

After your [Slack app](/slack-apps) is awarded an identity token through [Sign in with Slack](/docs/sign-in-with-slack), use this method to retrieve a user's identity.

The returned fields depend on any additional authorization scopes you've requested.

This method may only be used by tokens with the `identity.basic` scope, as provided in the [Sign in with Slack](/docs/sign-in-with-slack) process.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

## Response

You will receive at a minimum the following information:

```
{
    "ok": true,
    "user": {
        "name": "Sonny Whether",
        "id": "U0G9QF9C6"
    },
    "team": {
        "id": "T0G9PQBBK"
    }
}
```

The `identity.email` scope provides the member's email address, if available:

```
{
    "ok": true,
    "user": {
        "name": "Sonny Whether",
        "id": "U0G9QF9C6",
        "email": "bobby@example.com"
    },
    "team": {
        "id": "T0G9PQBBK"
    }
}
```

Using with the `identity.avatar` scope yields the member's avatar images. _Available sizes may vary in the future._

```
{
    "ok": true,
    "user": {
        "name": "Sonny Whether",
        "id": "U0G9QF9C6",
        "image_24": "https:\/\/cdn.example.com\/sonny_24.jpg",
        "image_32": "https:\/\/cdn.example.com\/sonny_32.jpg",
        "image_48": "https:\/\/cdn.example.com\/sonny_48.jpg",
        "image_72": "https:\/\/cdn.example.com\/sonny_72.jpg",
        "image_192": "https:\/\/cdn.example.com\/sonny_192.jpg"
    },
    "team": {
        "id": "T0G9PQBBK"
    }
}
```

Use with the `identity.team` scope to retrieve the user's workspace name:

```
{
    "ok": true,
    "user": {
        "name": "Sonny Whether",
        "id": "U0G9QF9C6"
    },
    "team": {
        "name": "Captain Fabian's Naval Supply",
        "id": "T0G9PQBBK"
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "account_inactive"
}
```

User IDs are not guaranteed to be globally unique across all Slack users. The combination of user ID and team ID, on the other hand, is guaranteed to be globally unique.

See the [Sign in with Slack](/docs/sign-in-with-slack) docs for even more information on these responses.

In addition, you can request access to additional profile fields by adding the following [authorization scopes](/docs/oauth-scopes) to your OAuth request, detailed in the above examples.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. |
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

