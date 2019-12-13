Invite a user to a workspace.

## Facts

| Method URL: | `https://slack.com/api/admin.users.invite` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`admin.users:write`](/scopes/admin.users:write)&nbsp; |

 |

* * *

This Admin API invites a user to a workspace.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `channel_ids` | `C1A2B3C4D,C26Z25Y24` | Required | A comma-separated list of `channel_id`s for this user to join. At least one channel is required. |
| `email` | `joe@email.com` | Required | The email address of the person to invite. |
| `team_id` | &nbsp; | Required | The ID (`T1234`) of the workspace. |
| `custom_message` | `Come and join our team!` | Optional | An optional message to send to the user in the invite email. |
| `guest_expiration_ts` | `0123456789.012345` | Optional | Timestamp when guest account should be disabled. Only include this timestamp if you are inviting a guest user and you want their account to expire on a certain date. |
| `is_restricted` | `true` | Optional | Is this user a multi-channel guest user? (default: false) |
| `is_ultra_restricted` | `true` | Optional | Is this user a single channel guest user? (default: false) |
| `real_name` | `{"full_name":"Joe Smith"}` | Optional | Full name of the user. |
| `resend` | `true` | Optional | Allow this invite to be resent in the future if a user has not signed up yet. (default: false) |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

## Response

Typical success response

```
{
    "ok": true
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `team_not_found` | `team_id` was not found. |
| `feature_not_enabled` | The Admin APIs feature is not enabled for this team. |
| `invalid_email` | Email address was not valid. |
| `already_in_team` | The user is already on the team. |
| `failed_looking_up_user` | We couldn't find the requested user. |
| `failed_to_validate_caller` | The token calling this method doesn't have permission to invite a user. |
| `failed_to_validate_team` | The team calling this method was invalid. |
| `failed_to_validate_channels` | `channel_ids` was invalid. |
| `failed_to_validate_expiration` | `expiration_ts` was invalid. |
| `failed_to_validate_custom_message` | `extra_message` was invalid. |
| `failed_to_send_invite` | The invite failed to send. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `is_bot` | This method cannot be called by a bot user. |
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

