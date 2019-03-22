## Facts

| Method URL: | `https://slack.com/api/apps.permissions.users.request` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |

 |

* * *

<ts-icon class="ts_icon_sparkles"></ts-icon> **Developer preview has ended**  

This feature was exclusive to our [workspace apps developer preview](/legacy-workspace-apps). The preview has now ended, but fan-favorite features such as token rotation and the Conversations API will become available to classic Slack apps over the coming months.

This method is used to request additional permissions from a user on a workspace.

It's part of the [Permissions API](/docs/permissions-api) that is available only to workspace apps.

To list currently awarded user-centric permissions, use [`apps.permissions.users.list`](/methods/apps.permissions.users.list).

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `scopes` | &nbsp; | Required | A comma separated list of user scopes to request for |
| `trigger_id` | &nbsp; | Required | Token used to trigger the request |
| `user` | &nbsp; | Required | The user this scope is being requested for |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

The `trigger_id` parameter is required and can be found attached to some [Events API](/events-api) events, interactive framework action URL invocations, dialog submissions, and other ways we send your app timely, user-initiated data. See these docs about the [Permissions API](/docs/permissions-api) and [triggers](/docs/triggers) to learn more.

## Response

Standard success response when used with a user token

```
{
    "ok": true
}
```

Standard failure response when trigger\_id is invalid

```
{
    "ok": false,
    "error": "invalid_trigger_id"
}
```

A response other than `"ok": true` indicates the permission request was not presented to the user, perhaps due to an invalid or expired `trigger_id`.

You'll receive related [app events](/events-api#app_events) if the user approves or rejects your permissions request. If the user does nothing, no event will emit.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_trigger` | The provided `trigger_id` is invalid. |
| `trigger_exchanged` | The provided `trigger_id` has already been exchanged. |
| `invalid_scope` | Scopes are invalid. |
| `invalid_user` | There isn't a valid user to request permissions. |
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

