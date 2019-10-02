Approve an app for installation on a workspace.

## Facts

| Method URL: | `https://slack.com/api/admin.apps.approve` |
| Preferred HTTP method: | `POST` |
| Accepted content types: | `application/x-www-form-urlencoded`, [`application/json`](/web#posting_json "Learn more about sending HTTP POST with JSON") |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | _No scope required_ |

 |

* * *

This [App Management API](/admins/managing) method approves an app install request in a specific workspace.

This method requires an `admin.*` scope. It's obtained through the normal [OAuth process](/docs/oauth), but there are a few additional requirements. The scope must be requested by an Enterprise Grid admin or owner, and the OAuth install must take place on the entire Grid org, not an individual workspace. See the [`admin.apps:write` page](/scopes/admin.apps:write) for more detailed instructions.

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `app_id` | `A12345` | Optional | The id of the app to approve. |
| `request_id` | `Ar12345` | Optional | The id of the request to approve. |
| `team_id` | &nbsp; | Optional | |

<ts-icon class="ts_icon_code"></ts-icon>This method supports `application/json` via HTTP POST. Present your `token` in your request's `Authorization` header. [Learn more](/web#posting_json).

Either `app_id` or `request_id` is required. These IDs can be obtained either directly via the [`app_requested` event](/events/app_requested), or by the [`admin.apps.requests.list` method](/methods/admin.apps.requests.list).

`team_id` is **required** if your Enterprise Grid org contains more than one workspace.

## Response

```
{
        "ok": true
    }
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_request_id` | The `request_id` passed is invalid. |
| `invalid_app_id` | The `app_id` passed is invalid. |
| `request_id_required_for_custom_integrations` | A `request_id` is required for custom integrations |
| `feature_not_enabled` | Returned when the Admin APIs feature is not enabled for this team |
| `not_an_admin` | This method is only accessible by org owners and admins |
| `request_id_or_app_id_is_required` | Must include a `request_id` or `app_id` |
| `too_many_ids_provided` | Please provide only `app_id` OR `request_id` |
| `too_many_teams_provided` | Please provide only `team_id` OR `enterprise_id` |
| `request_already_resolved` | The app request has already been resolved |
| `team_not_found` | Returned when team id is not found. |
| `app_management_app_not_installed_on_org` | The app management app must be installed on the org. |
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

