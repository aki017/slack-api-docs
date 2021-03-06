List emoji for an Enterprise Grid organization.

## Facts

| Method URL: | `https://slack.com/api/admin.emoji.list` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`admin.teams:read`](/scopes/admin.teams:read)&nbsp; |

 |

* * *

This [Admin API method](/enterprise/managing) lists the emoji across an Enterprise Grid organization.

If you're looking to list emoji in a single workspace, use the regular [`emoji.list`](/methods/emoji.list) method. Also, this Admin method only lists _custom_ emoji. Use the regular `emoji.list` method with the `include_categories` boolean to see standard emoji included with Slack.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

## Arguments

 | Argument | Example | Required | Description |
| --- | --- | --- | --- |
 | `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token bearing required scopes. |
| `cursor` | `5c3e53d5` | Optional | Set `cursor` to `next_cursor` returned by the previous call to list items in the next page |
| `limit` | `100` | Optional, default=100 | The maximum number of items to return. Must be between 1 - 1000 both inclusive. |

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

This `admin` scope is obtained through the [OAuth flow](/docs/oauth), but there are a few additional requirements. The app requesting this scope **must** be [installed](/start/overview#installing_distributing) by an _ **admin or Owner** _ of an Enterprise Grid organization. Also, the app must be installed on the **entire** org, not on an individual workspace. See below for more details.

Admin API endpoints reach across **an entire Enterprise Grid organization** , not individual workspaces.

For a token to be imbued with Admin scopes, it must be obtained from installing an app on the **entire Grid org** , not just a workspace within the organization.

To configure and install an app supporting Admin API endpoints on your Enterprise Grid organization:

1. [Create a new Slack app](/apps). Your app will need to be able to handle a standard [OAuth 2 flow](/docs/oauth#flow).
2. In the app's settings, select **OAuth & Permissions** from the left navigation. Scroll down to the section titled **Scopes** and add the `admin.*` scope you want. Click the green **Save Changes** button.
3. In the app's settings, select **Manage Distribution** from the left navigation. Under the section titled **Share Your App with Other Workspaces** , make sure all four sections have the green check. Then click the green **Activate Public Distribution** button.
4. Under the **Share Your App with Your Workspace** section, copy the **Sharable URL** and paste it into a browser to initiate the OAuth handshake that will install the app on your organization. You must be logged in as an **admin or Owner** of your Enterprise Grid organization to install the app.
5. Check the dropdown in the upper right of the installation screen to make sure you are installing the app on the organization, not an individual workspace within the organization. See the image below for a visual.
6. Once your app completes the OAuth flow, you will be granted an OAuth token that can be used for calling Admin API methods for your organization.
 ![](https://a.slack-edge.com/80588/img/api/workspace-v-org-audit.png)
_When installing an app to use an Admin API endpoint, be sure to install it on your Grid organization, not a workspace within the organization._

## Response

Typical success response

```
{
    "ok": true,
    "emoji": {
        "bowtie": "https://emoji.slack-edge.com/T9TK3CUKW/bowtie/f3ec6f2bb0.png",
        "squirrel": "https://emoji.slack-edge.com/T9TK3CUKW/squirrel/465f40c0e0.png",
        "glitch_crab": "https://emoji.slack-edge.com/T9TK3CUKW/glitch_crab/db049f1f9c.png",
        "piggy": "https://emoji.slack-edge.com/T9TK3CUKW/piggy/b7762ee8cd.png",
        "cubimal_chick": "https://emoji.slack-edge.com/T9TK3CUKW/cubimal_chick/85961c43d7.png",
        "dusty_stick": "https://emoji.slack-edge.com/T9TK3CUKW/dusty_stick/6177a62312.png",
        "slack": "https://emoji.slack-edge.com/T9TK3CUKW/slack/7d462d2443.png",
        "pride": "https://emoji.slack-edge.com/T9TK3CUKW/pride/56b1bd3388.png",
        "thumbsup_all": "https://emoji.slack-edge.com/T9TK3CUKW/thumbsup_all/50096a1020.gif",
        "slack_call": "https://emoji.slack-edge.com/T9TK3CUKW/slack_call/b81fffd6dd.png",
        "shipit": "alias:squirrel",
        "white_square": "alias:white_large_square",
        "black_square": "alias:black_large_square",
        "simple_smile": {
            "apple": "https://a.slack-edge.com/80588/img/emoji_2017_12_06/apple/simple_smile.png",
            "google": "https://a.slack-edge.com/80588/img/emoji_2017_12_06/google/simple_smile.png"
        }
    },
    "cache_ts": "1575283387.000000",
    "categories_version": "5",
    "categories": [
        {
            "name": "Smileys & People",
            "emoji_names": [
                "grinning",
                "grin",
                "joy",
                "etc etc ..."
            ]
        }
    ]
}
```

The `cache_ts` field in the response indicates the last time the emoji set was updated. You can use it to cache this list.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `invalid_cursor` | Value passed for `cursor` was not valid or is no longer valid. |
| `feature_not_enabled` | The Admin APIs feature is not enabled for this team. |
| `not_an_admin` | This method is only accessible by org owners and Admins. |
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

