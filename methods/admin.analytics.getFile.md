Retrieve analytics data for a given date, presented as a compressed JSON file

## Facts

| Method URL: | `https://slack.com/api/admin.analytics.getFile` |
| Preferred HTTP method: | `GET` |
| Accepted content types: | `application/x-www-form-urlencoded` |
| Rate limiting: | [Tier 2](/docs/rate-limits#tier_t2) |
| Works with: | 

| Token type | Required scope(s) |
| --- | --- |
| [user](/docs/token-types#user) | [`admin.analytics:read`](/scopes/admin.analytics:read)&nbsp; |

 |

* * *

This method returns a new-line delimited JSON file, compressed with _gzip_. The file contains analytics data for a single day.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

Analytics data is available dating back to January 1st, 2020.

Historical data is not recomputed when a workspace and its accompanying member engagement data is added to or removed from an organization. Similarly, if a member is removed from a workspace, the data returned by the API will not update historical engagement data to reflect only the workspaces the member is currently a part of. This behavior **differs** from the _Analytics Dashboard_ in Org and Team settings, which does consider historical changes in its calculation of 30 day and all time metrics.

## Arguments

`token`Required
Authentication token bearing required scopes.
Example`xxxx-xxxxxxxxx-xxxx`

`date`Required
Date to retrieve the analytics data for, expressed as `YYYY-MM-DD` in UTC.
Example`2020-09-01`

`type`Required
The type of analytics to retrieve. The options are currently limited to `member`.
Example`member`

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

The `date` argument is **required**. It represents the date in UTC corresponding to the analytics data you're requesting.

The `type` argument is **required**. It represents type of analytics data you are requesting. Currently, the API supports `member` analytics.

## Response

This method is almost completely unlike other [Web API](/web) methods you encounter. It doesn't return `application/json` with the traditional `"ok": true` response on success, though you'll find `"ok": false` on failure.

Instead, it returns a single file, often very large, containing JSON objects that are separated by newlines and then compressed with `application/gzip`.

To open the successful response and utilize the data, you must first store and decompress the response. Some tools may parse new line-separated JSON for you with ease, while other tools will require that you first split each new line separated "row" into its own separate JSON object before being deserialized.

##### Common successful response

A gzipped new-line delimited JSON file presented with a `Content-type: application/gzip` HTTP header. An example file name is `Slack Corp Member Analytics 2020-08-01.json.gz`

##### Common error response

Typical Web API error response is a JSON response with a `Content-type: application/json` HTTP header.

```
{
    "ok": false,
    "error": "org_level_email_display_disabled"
}
```

After decompression, the file will look something like:

```
{"enterprise_id":"E5UBAR8CH","date":"2020-10-05","user_id":"W0POSID23ID","email_address":"rbrautigan@example.org","is_guest":false,"is_billable_seat":false,"is_active":false,"is_active_ios":false,"is_active_android":false,"is_active_desktop":false,"reactions_added_count":0,"messages_posted_count":0,"channel_messages_posted_count":0,"files_added_count":0}
{"enterprise_id":"E5UBAR8CH","date":"2020-10-05","user_id":"W1ZOSID3ZI2","email_address":"rbrautigan@example.org","is_guest":false,"is_billable_seat":true,"is_active":true,"is_active_ios":false,"is_active_android":false,"is_active_desktop":true,"reactions_added_count":23,"messages_posted_count":123,"channel_messages_posted_count":23,"files_added_count":3}
{"enterprise_id":"E5UBAR8CH","date":"2020-10-05","user_id":"W3DOSZD23IP","email_address":"rbrautigan@example.org","is_guest":false,"is_billable_seat":true,"is_active":true,"is_active_ios":true,"is_active_android":false,"is_active_desktop":false,"reactions_added_count":521,"messages_posted_count":5,"channel_messages_posted_count":5,"files_added_count":0}
```

### Field guide 

Each row of JSON data may include the following fields.

| Field | Example | About |
| --- | --- | --- |
| `date` | `2020-09-13` | The date this row of data is representative of |
| `enterprise_id` | `E2AB3A10F` | Unique ID of the involved Enterprise organization |
| `enterprise_user_id` | `W1F83A9F9` | The canonical, organization-wide user ID this row concerns |
| `email_address` | `person@acme.com` | The email address of record for the same user |
| `enterprise_employee_number` | `273849373` | This field is pulled from data synced via a [SCIM API custom attribute](/scim#user-attributes) |
| `is_guest` | `false` | User is classified as a _guest_ (not a full workspace member) on the date in the API request |
| `is_billable_seat` | `true` | User is classified as a billable user ( [included in the bill](https://slack.com/help/articles/218915077-Fair-Billing-Policy)) on the the date in the API request |
| `is_active` | `true` | User has posted a message or read at least one channel or direct message on the date in the API request |
| `is_active_ios` | `true` | User has posted a message or read at least one channel or direct message on the date in the API request via the Slack iOS App |
| `is_active_android` | `false` | User has posted a message or read at least one channel or direct message on the date in the API request via the Slack Android App |
| `is_active_desktop` | `true` | User has posted a message or read at least one channel or direct message on the date in the API request via the Slack Desktop App |
| `reactions_added_count` | `20` | Total reactions added to any message type in any conversation type by the user on the date in the API request. Removing reactions is not included. This metric is not de-duplicated by message—if a user adds 3 different reactions to a single message, we will report `3` reactions |
| `messages_posted_count` | `40` | Total messages posted by the user on the date in the API request to all message and conversation types, whether public, private, multi-person direct message, etc. |
| `channel_messages_posted_count` | `30` | Total messages posted by the user in private channels and public channels on the date in the API request, not including direct messages |
| `files_added_count` | `5` | Total files uploaded by the user on the date in the API request |

## Practical usage example 

To quickly decompress the file and view the JSON directly, use a cURL command like:

```
curl "https://slack.com/api/admin.analytics.getFile" -d "date=2020-11-03&type=member" | gunzip -c > member_analytics_2020_11_03.json
```

...and you'll have a `member_analytics_2020_11_03.json` file in the current operating directory. In some organizations the decompressed size can be quite large. Take care that you have the disk or memory space you need to store and process these files.

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `data_not_available` | The `date` was before the API became available. |
| `file_not_found` | The analytics data for the `date` specified weren't found. |
| `file_not_yet_available` | The analytics data for the `date` isn't available yet. |
| `invalid_type` | The analytics data for the `type` specified weren't found. |
| `invalid_arguments` | The method was either called with invalid arguments or some detail about the arguments passed are invalid, which is more likely when using complex arguments like blocks or attachments. |
| `invalid_date` | The `date` argument was invalid. |
| `missing_scope` | The token used is not granted the specific scope permissions required to complete this request. |
| `not_an_enterprise` | The user token does not belong to an enterprise. |
| `not_an_admin` | The user token does not have admin privileges. |
| `feature_not_enabled` | This feature is not enabled on your workspace. |
| `member_analytics_disabled` | Member analytics are disabled for your org. |
| `org_level_email_display_disabled` | This API is unavailable for orgs with a 'Hide email addresses.' policy. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed. |
| `no_permission` | The workspace token used in this request does not have the permissions necessary to complete the request. Make sure your app is a member of the conversation it's attempting to post a message to. |
| `org_login_required` | The workspace is undergoing an enterprise migration and will not be available until migration is complete. |
| `ekm_access_denied` | Administrators have suspended the ability to post a message. |
| `not_allowed_token_type` | The token type used in this request is not allowed. |
| `method_deprecated` | The method has been deprecated. |
| `deprecated_endpoint` | The endpoint has been deprecated. |
| `two_factor_setup_required` | Two factor setup is required. |
| `enterprise_is_restricted` | The method cannot be called from an Enterprise. |
| `is_bot` | This method cannot be called by a bot user. |
| `invalid_arg_name` | The method was passed an argument whose name falls outside the bounds of accepted or expected values. This includes very long names and names with non-alphanumeric characters other than `_`. If you get this error, it is typically an indication that you have made a _very_ malformed API call. |
| `invalid_array_arg` | The method was passed an array as an argument. Please only input valid strings. |
| `invalid_charset` | The method was called via a `POST` request, but the `charset` specified in the `Content-Type` header was invalid. Valid charset names are: `utf-8` `iso-8859-1`. |
| `invalid_form_data` | The method was called via a `POST` request with `Content-Type` `application/x-www-form-urlencoded` or `multipart/form-data`, but the form data was either missing or syntactically invalid. |
| `invalid_post_type` | The method was called via a `POST` request, but the specified `Content-Type` was invalid. Valid types are: `application/json` `application/x-www-form-urlencoded` `multipart/form-data` `text/plain`. |
| `missing_post_type` | The method was called via a `POST` request and included a data payload, but the request did not include a `Content-Type` header. |
| `team_added_to_org` | The workspace associated with your request is currently undergoing migration to an Enterprise Organization. Web API and other platform operations will be intermittently unavailable until the transition is complete. |
| `ratelimited` | The request has been ratelimited. Refer to the `Retry-After` header for when to retry the request. |
| `accesslimited` | Access to this method is limited on the current network |
| `request_timeout` | The method was called via a `POST` request, but the `POST` data was either missing or truncated. |
| `service_unavailable` | The service is temporarily unavailable |
| `fatal_error` | The server could not complete your operation(s) without encountering a catastrophic error. It's possible some aspect of the operation succeeded before the error was raised. |
| `internal_error` | The server could not complete your operation(s) without encountering an error, likely due to a transient issue on our end. It's possible some aspect of the operation succeeded before the error was raised. |

## Warnings

This table lists the expected warnings that this method will return. However, other warnings can be returned in the case where the service is experiencing unexpected trouble.

| Warning | Description |
| --- | --- |
| `missing_charset` | The method was called via a `POST` request, and recommended practice for the specified `Content-Type` is to include a `charset` parameter. However, no `charset` was present. Specifically, non-form-data content types (e.g. `text/plain`) are the ones for which `charset` is recommended. |
| `superfluous_charset` | The method was called via a `POST` request, and the specified `Content-Type` is not defined to understand the `charset` parameter. However, `charset` was in fact present. Specifically, form-data content types (e.g. `multipart/form-data`) are the ones for which `charset` is superfluous. |

