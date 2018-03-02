Retrieves a user's profile information.

## Facts

| Method URL: | `https://slack.com/api/users.profile.get` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [workspace](/docs/token-types#workspace) | [`users.profile:read`](/scopes/users.profile:read) |
| [user](/docs/token-types#user) | [`users.profile:read`](/scopes/users.profile:read) [`read`](/scopes/read) |

 |

* * *

Use this method to retrieve a user's profile information.

## Arguments

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `include_labels` | `true` | Optional, default=false | Include labels for each ID in custom profile fields |
| `user` | `W1234567890` | Optional | User to retrieve profile info for |

<ts-icon class="ts_icon_code"></ts-icon> Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

If you're frequently calling `users.profile.get` on behalf of a team or user, we recommend caching labels retrieved from [`team.profile.get`](/methods/team.profile.get). Please only use the `include_labels` parameter with `users.profile.get` **sparingly**.

The `include_labels` parameter is **heavily rate-limited**.

## Response

The response contains a `profile` item with an array of key:value pairs.

Typical success response

```
{
    "ok": true,
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
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "user_not_found"
}
```

We hope you find the `first_name`, `last_name`, `email` attributes self-explanatory.

The user's custom-set "current status" can be found in the `status_text` and `status_emoji` attributes. See [custom status](/docs/presence#custom_status) for more.

The `image_` keys hold links to the different sizes we support for the user's profile image from 24x24 to 1024x1024 pixels. A link to the image in its original size is stored in `image_original`.

Bot users may contain an `always_active` profile field, indicating whether the bot user is active in a way that overrides traditional presence rules. The [presence](/docs/presence#bot_presence) docs tell the whole story.

For a description of the `fields` key, see the [users.profile.set](/methods/users.profile.set) method.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
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

