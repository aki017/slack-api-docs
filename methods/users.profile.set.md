This method is used to set the profile information for a user.

## Arguments

This method has the URL `https://slack.com/api/users.profile.set` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `users.profile:write`) |
| `user` | `U1234567890` | Optional | ID of user to change. This argument may only be specified by team admins on paid teams. |
| `profile` | `{ first_name: "John", ... }` | Optional | Collection of key:value pairs presented as a URL-encoded JSON hash. |
| `name` | `first_name` | Optional | Name of a single key to set. Usable only if `profile` is not passed. |
| `value` | `John` | Optional | Value to set a single key to. Usable only if `profile` is not passed. |

Individual fields can be updated by passing the pair of arguments `name` and`value`; or multiple fields can be updated at once by passing the argument`profile`.

The `profile` argument accepts a URL-encoded JSON object containing all the fields to be updated as so:

```
{
    "first_name": "John",
    "last_name": "Smith",
    "email": "john@smith.com",
    "fields": {
        "Xf06054BBB": {
            "value": "Barista",
            "alt": "I make the coffee & the tea!",
        }
    }
}
```

You would send the `profile` parameter value URL-encoded as:

```
%7B%22first_name%22%3A%22John%22%2C%22last_name%22%3A%22Smith%22%2C%22email
%22%3A%22john%40smith.com%22%2C%22fields%22%3A%7B%22Xf06054BBB%22%3A%7B
%22value%22%3A%22Barista%22%2C%22alt%22%3A%22I%20make%20the%20coffee
%20%26%20the%20tea%21%22%7D%7D
```

The `first_name` and `last_name` fields can be up to 35 characters each. The name `slackbot` cannot be used for either of these fields.

The `email` field must be a valid email address. It cannot have spaces, and it must have an `@` and a domain. It cannot be in use by another member of the same team. Changing a user's email address will send an email to both the old and new addresses, and also post a slackbot to the user informing them of the change. This field can only be changed for users on paid teams.

The `skype` field can be up to 254 characters.

The recommended way to set the profile image fields is to call [`users.setPhoto`](/methods/users.setPhoto). To clear them, call [`users.deletePhoto`](/methods/users.deletePhoto).

The `fields` key is an array of key:value pairs holding the values for the user's custom profile fields. The key is the ID of the definition for that field, which is per-team. This ID can also be used in the `name` field of this method. The `value` of a field is what should be displayed, unless the `alt` key is also present, in which case that is displayed instead. The `value` can be up to 256 characters for fields of type `text` and `link`. For fields of type `options_list`, the `value` must be one of the `possible_values` in the field definition. For fields of type `date`, the `value` must be a valid date. The `alt` field can be up to 256 characters for all field types. The `profile` argument must be used in order to set the `alt` field.

Use [`team.profile.get`](/methods/team.profile.get) to retrieve the profile fields used by a team.

## Response

After making this call, the complete user's profile will be returned in the same format as above.

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

This method will generate a [`user_change`](/events/user_change) event on success, containing the complete user.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `reserved_name` | First or last name are reserved. |
| `invalid_profile` | Profile object passed in is not valid JSON (make sure it is URL encoded!). |
| `profile_set_failed` | Failed to set user profile. |
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

