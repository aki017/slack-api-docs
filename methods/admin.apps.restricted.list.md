List restricted apps for an org or workspace.

## Facts

| Method URL: | `https://slack.com/api/admin.apps.restricted.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`admin.apps:read`](/scopes/admin.apps:read)&nbsp; |

 |

* * *

This [App Management API](/admins/managing) method lists apps restricted from installation on a workspace or org.

This method requires an `admin.*` scope. It's obtained through the normal [OAuth process](/docs/oauth), but there are a few additional requirements. The scope must be requested by an Enterprise Grid admin or owner, and the OAuth install must take place on the entire Grid org, not an individual workspace. See the [`admin.apps:read` page](/scopes/admin.apps:read) for more detailed instructions.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `5c3e53d5` | Optional | Set `cursor` to `next_cursor` returned by the previous call to list items in the next page |
| `enterprise_id` | `E0AS553RN` | Optional | |
| `limit` | `100` | Optional, default=100 | The maximum number of items to return. Must be between 1 - 1000 both inclusive. |
| `team_id` | `T0HFE6EBT` | Optional | |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

**Note:** `enterprise_id` and `team_id` cannot be used together.

- Passing `enterprise_id` will return the list of org-wide restricted apps.
- Passing `team_id` will return the apps restricted for that specific workspace.

## Response

Typical success response

```
{
    "ok": true,
    "restricted_apps": [
        {
            "app": {
                "id": "A0FDLP8M2L",
                "name": "My Test App",
                "description": "A fun test app for Slack",
                "help_url": "https://example.com",
                "privacy_policy_url": "https://example.com",
                "app_homepage_url": "https://example.com",
                "app_directory_url": "https://myteam.enterprise.slack.com/apps/A0FDLP8M2L-my-test-app",
                "is_app_directory_approved": true,
                "is_internal": false,
                "icons": {
                    "image_32": "https://143326534038rl8788_eb57dbc818daa4ba15d6_32.png",
                    "image_36": "https://143326534038rl8788_eb57dbc818daa4ba15d6_36.png",
                    "image_48": "https://143326534038rl8788_eb57dbc818daa4ba15d6_48.png",
                    "image_64": "https://143326534038rl8788_eb57dbc818daa4ba15d6_64.png",
                    "image_72": "https://143326534038rl8788_eb57dbc818daa4ba15d6_72.png",
                    "image_96": "https://143326534038rl8788_eb57dbc818daa4ba15d6_96.png",
                    "image_128": "https://4332653438rl87808_eb57dbc818daa4ba15d6_128.png",
                    "image_192": "https://4332653438rl87808_eb57dbc818daa4ba15d6_192.png",
                    "image_512": "https://4332653438rl87808_eb57dbc818daa4ba15d6_512.png",
                    "image_1024": "https://1433265338rl878408_eb57dbc818daa4ba15d6_1024.png",
                    "image_original": "https://143338rl8782653408_eb57dbc818daa4ba15d6_original.png"
                },
                "additional_info": ""
            },
            "scopes": [
                {
                    "name": "files:write:user",
                    "description": "Upload, edit, and delete files on the user\u201fs behalf",
                    "is_sensitive": true,
                    "token_type": "user"
                }
            ],
            "date_updated": 1574296721,
            "last_resolved_by": {
                "actor_id": "W0G82LMFD",
                "actor_type": "user"
            }
        }
    ],
    "response_metadata": {
        "next_cursor": ""
    }
}
```

Typical error response

```
{
    "ok": false,
    "error": "invalid_auth"
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `feature_not_enabled` | Returned when the Admin APIs feature is not enabled for this team |
| `not_an_admin` | This method is only accessible by org/workspace owners and admins |
| `team_not_found` | Returned when team id is not found. |
| `too_many_teams_provided` | Please provide only `team_id` OR `enterprise_id`. |
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

