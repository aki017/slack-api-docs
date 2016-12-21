This method is used to get the profile information for a user.

## Arguments

This method has the URL `https://slack.com/api/users.profile.get` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token.  
Requires scope: `users.profile:read` |
| `user` | `U1234567890` | Optional | User to retrieve profile info for |
| `include_labels` | `1` | Optional, default=0 | Include labels for each ID in custom profile fields |

If you call `users.profile.get` frequently on behalf of a team or user, we recommend caching labels retrieved from [`team.profile.get`](/methods/team.profile.get) or from when you **sparingly** use the `include_labels` parameter with `users.profile.get`.

The `include_labels` parameter is **heavily rate-limited**.

## Response

The response contains a `profile` item with an array of key:value pairs.

The `first_name`, `last_name`, `email` and `skype` keys are self-explanatory.

The `image_` keys hold links to the different sizes we support for the user's profile image from 24x24 to 1024x1024 pixels. A link to the image in its original size is stored in `image_original`.

Bot users may contain an `always_active` profile field, indicating whether the bot user is active in a way that overrides traditional presence rules. The [presence](/docs/presence#bot_presence) docs tell the whole story.

For a description of the `fields` key, see the [users.profile.set](/methods/users.profile.set) method.

```
{
    "ok": true,
    "profile": {
        "first_name": "John",
        "last_name": "Smith",
        "email": "john@smith.com",
        "skype": "johnsmith",
        "image_24": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_24.jpg",
        "image_32": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_32.jpg",
        "image_48": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_48.jpg",
        "image_72": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_72.jpg",
        "image_192": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_192.jpg",
        "image_512": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_512.jpg",
        "image_1024": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_1024.jpg",
        "image_original": "https://s3.amazonaws.com/slack-files/avatars/2015-11-16/123456_original.jpg",
        "fields": {
            "Xf06054AAA": {
                "value": "San Francisco",
                "alt": "Giants, yo!",
                "label": "Favorite Baseball Team"
            },
            "Xf06054BBB": {
                "value": "Barista",
                "alt": "I make the coffee & the tea!",
                "label": "Position"
            }
        }
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `user_not_found` | Value passed for `user` was invalid. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of common decency. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed a PHP-style array argument (e.g. with a name like `foo[7]`). These are never valid with the Slack API. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

