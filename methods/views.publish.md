Publish a static view for a User.

## Facts

| Method URL: | `https://slack.com/api/views.publish` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 4](/docs/rate-limits#tier_t4) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [bot](/docs/token-types#bot) | [`bot`](/scopes/bot) |
| [user](/docs/token-types#user) | _No scope required_ |

 |

* * *

Create or update the view that comprises [an app's Home tab](/surfaces/tabs) for a specific user.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `user_id` | `U0BPQUNTA` | Required | `id` of the user you want publish a view to. |
| `view` | &nbsp; | Required | A [view payload](/reference/surfaces/views). This must be a JSON-encoded string. |
| `hash` | `156772938.1827394` | Optional | A string that represents view state to protect against possible race conditions. |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

Assuming your view payload was properly formatted, valid, and the `user_id` was viable, you will receive a success response.

Typical success response includes the published view payload.

```
{
    "ok": true,
    "view": {
        "id": "VMHU10V25",
        "team_id": "T8N4K1JN",
        "type": "home",
        "close": null,
        "submit": null,
        "blocks": [
            {
                "type": "section",
                "block_id": "2WGp9",
                "text": {
                    "type": "mrkdwn",
                    "text": "A simple section with some sample sentence.",
                    "verbatim": false
                }
            }
        ],
        "private_metadata": "Shh it is a secret",
        "callback_id": "identify_your_home_tab",
        "state": {
            "values": []
        },
        "hash": "156772938.1827394",
        "clear_on_close": false,
        "notify_on_close": false,
        "root_view_id": "VMHU10V25",
        "previous_view_id": null,
        "app_id": "AA4928AQ",
        "external_id": "",
        "bot_id": "BA13894H"
    }
}
```

Typical error response, before getting to any possible validation errors.

```
{
    "ok": false,
    "error": "invalid_arguments",
    "response_metadata": {
        "messages": [
            "invalid `user_id`"
        ]
    }
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `hash_conflict` | Error returned when the provided `hash` doesn't match the current stored value. |
| `duplicate_external_id` | Error returned when the given `external_id` has already be used. |
| `view_too_large` | Error returned if the provided view is greater than 250kb. |
| `not_enabled` | Error returned if a `home` view is published but the Home tab isn't enabled for the app. |
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

