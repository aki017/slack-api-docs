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

This method returns member and conversation analytics data presented as new-line delimited JSON files, compressed with _gzip_.

Each response contains a file with analytics data for a single day.

This [API method for admins](/enterprise/managing) may only be used on [Enterprise Grid](/enterprise).

Analytics data is available dating back to January 1st, 2020.

Historical data is not recomputed when a workspace and its accompanying member engagement data is added to or removed from an organization. Similarly, if a member is removed from a workspace, the data returned by the API will not update historical engagement data to reflect only the workspaces the member is currently a part of. This behavior **differs** from the _Analytics Dashboard_ in Org and Team settings, which does consider historical changes in its calculation of 30 day and all time metrics.

**Coming end of April 2021** : We are adding six new metrics to to our member analytics dashboards! The six new metrics will be available for in the daily member metrics files begining with January 1, 2020. Check the member analytics fields for more details.

## Arguments

`token`Required
Authentication token bearing required scopes. Tokens should be passed as an HTTP Authorization header or alternatively, as a POST parameter.
Example`xxxx-xxxxxxxxx-xxxx`

`type`Required
The type of analytics to retrieve. The options are currently limited to `member` (for grid member analytics) and `public_channel` (for public channel analytics).
Example`member`

`date`Optional
Date to retrieve the analytics data for, expressed as `YYYY-MM-DD` in UTC.
Example`2020-09-01`

`metadata_only`Optional
Retrieve metadata for the `type` of analytics indicated. Can be used only with `type` set to `public_channel` analytics. See detail below. Omit the `date` parameter when using this argument.

Default: `false`
Example`true`

<ts-icon class="ts_icon_code"></ts-icon>Present arguments as parameters in `application/x-www-form-urlencoded` querystring or POST body. This method does not currently accept `application/json`.

The `date` argument is **required** when requesting daily analytics data. It represents the date in UTC corresponding to the analytics data you're requesting. Omit this argument when setting `only_metadata` to `true`, as metadata cannot be filtered by date.

The `type` argument is **required**. It represents type of analytics data you are requesting. Currently, the API supports `member` (what users do in workspaces) or `public_channel` (what happens in conversations) analytics.

When setting `type` to `public_channel`, you may also use the `only_metadata` boolean argument, which changes the response entirely to give you metadata about the public channels appearing in your conversation analytics. Omit the `date` parameter when using this mode.

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

After decompression, the file returned in the response will look something like the following, depending on the type of analytics requested.

#### Member analytics response 

Here are 3 example lines of JSON you would find after decompressing a member analytics file. Each line provides information about a different user.

```
{"enterprise_id":"E5UBAR8CH","date":"2020-10-05","user_id":"W0POSID23ID","email_address":"rbrautigan@example.org","is_guest":false,"is_billable_seat":false,"is_active":false,"is_active_ios":false,"is_active_android":false,"is_active_desktop":false,"reactions_added_count":0,"messages_posted_count":0,"channel_messages_posted_count":0,"files_added_count":0,"date_claimed": 1584810430,"slack_calls_count": 12,"is_active_slack_connect": true,"is_active_workflows": true,"is_active_apps": true,"search_count": 223}
{"enterprise_id":"E5UBAR8CH","date":"2020-10-05","user_id":"W1ZOSID3ZI2","email_address":"gstein@example.org","is_guest":false,"is_billable_seat":true,"is_active":true,"is_active_ios":false,"is_active_android":false,"is_active_desktop":true,"reactions_added_count":23,"messages_posted_count":123,"channel_messages_posted_count":23,"files_added_count":3,"date_claimed": 1584810520,"slack_calls_count": 4,"is_active_slack_connect": false,"is_active_workflows": true,"is_active_apps": false,"search_count": 1001}
{"enterprise_id":"E5UBAR8CH","date":"2020-10-05","user_id":"W3DOSZD23IP","email_address":"obutler@example.org","is_guest":false,"is_billable_seat":true,"is_active":true,"is_active_ios":true,"is_active_android":false,"is_active_desktop":false,"reactions_added_count":521,"messages_posted_count":5,"channel_messages_posted_count":5,"files_added_count":0,"date_claimed": 1584810510,"slack_calls_count": 3,"is_active_slack_connect": false,"is_active_workflows": true,"is_active_apps": true,"search_count": 32}
```

To be extra helpful, here's one of those lines formatted a little prettier:

```
{
	"enterprise_id": "E5UBAR8CH",
	"date": "2020-10-05",
	"user_id": "W3DOSZD23IP",
	"email_address": "obutler@example.org",
	"is_guest": false,
	"is_billable_seat": true,
	"is_active": true,
	"is_active_ios": true,
	"is_active_android": false,
	"is_active_desktop": false,
	"reactions_added_count": 521,
	"messages_posted_count": 5,
	"channel_messages_posted_count": 5,
	"files_added_count": 0,
	"date_claimed": 1584810510,
	"slack_calls_count": 3,
	"is_active_slack_connect": false,
	"is_active_workflows": true,
	"is_active_apps": true,
	"search_count": 32
}
```

#### Public channel analytics response 

This example shows 3 lines of decompressed public channel analytics. Each line provides information about activity in a single public channel.

```
{"enterprise_id":"EJB3MZFLM","originating_team":{"team_id":"T5J3Q04QZ","name":"postmodernity"},"channel_id":"CNGL0KGG1","date_created":1555111593,"date_last_active":1684820530,"total_members_count":7,"full_members_count":6,"guest_member_count":1,"messages_posted_count":223,"messages_posted_by_members_count":80,"members_who_viewed_count":225,"members_who_posted_count":3,"reactions_added_count":23,"visibility":"public","channel_type":"single_workspace_channel","is_shared_externally":false,"shared_with":[],"externally_shared_with_organizations":[],"date":"2020-11-14"}
{"enterprise_id":"EJB3MZFLM","originating_team":{"team_id":"T3J3A04QB","name":"modernity"},"channel_id":"CNGG2KB92","date_created":1358111593,"date_last_active":1452719593,"total_members_count":227,"full_members_count":227,"guest_member_count":0,"messages_posted_count":1138,"messages_posted_by_members_count":1137,"members_who_viewed_count":226,"members_who_posted_count":7,"reactions_added_count":7212,"visibility":"public","channel_type":"single_workspace_channel","is_shared_externally":true,"shared_with":[],"externally_shared_with_organizations":[],"date":"2020-11-14"}
{"enterprise_id":"EJB3MZFLM","originating_team":{"team_id":"EJB3MZFLM","name":"arcane-enterprise"},"channel_id":"CNGZ5K595","date_created":1355111593,"date_last_active":1452719593,"total_members_count":5,"full_members_count":4,"guest_member_count":1,"messages_posted_count":1,"messages_posted_by_members_count":1,"members_who_viewed_count":5,"members_who_posted_count":1,"reactions_added_count":1,"visibility":"public","channel_type":"multi_workspace_channel","is_shared_externally":true,"shared_with":[{"team_id":"T5J3Q04QA","name":"scifi"},{"team_id":"EJB3MZFLM","name":"arcane-enterprise"}],"externally_shared_with_organizations":[{"name":"Away Org","domain":"away.enterprise.slack.com"}],"date":"2020-11-14"}
```

And here's just one line of that formatted in a friendly fashion:

```
{
	"enterprise_id": "EJB3MZFLM",
	"originating_team": {
		"team_id": "EJB3MZFLM",
		"name": "arcane-enterprise"
	},
	"channel_id": "CNGZ5K595",
	"date_created": 1355111593,
	"date_last_active": 1452719593,
	"total_members_count": 5,
	"full_members_count": 4,
	"guest_member_count": 1,
	"messages_posted_count": 1,
	"messages_posted_by_members_count": 1,
	"members_who_viewed_count": 5,
	"members_who_posted_count": 1,
	"reactions_added_count": 1,
	"visibility": "public",
	"channel_type": "multi_workspace_channel",
	"is_shared_externally": false,
	"shared_with": [
		{
			"team_id": "T5J3Q04QA",
			"name": "scifi"
		},
		{
			"team_id": "EJB3MZFLM",
			"name": "arcane-enterprise"
		}
	],
	"externally_shared_with_organizations": [
		{
			"name": "Away Org",
			"domain": "away.enterprise.slack.com"
		}
	],
	"date": "2020-11-14"
}
```

#### Public channel metadata response 

Uncompressed, you'll find each public channel's metadata on a single line like so:

```
{"channel_id":"CNGL0K091","name":"tomorrow","topic":"I'd gladly pay you Tuesday for a hamburger today","description":"What do you want to do tomorrow?","date":"2020-11-14"}
{"channel_id":"CNGG2KB92","name":"announcements","topic":"What's new with what you do","description":"Company announcements, edicts, and mandates","date":"2020-11-14"}
{"channel_id":"CNGZ5K595","name":"teds","topic":"'No you meant the other ted' - @ted","description":"A channel just for teds, by teds.","date":"2020-11-14"}
```

And here's that last one printed pretty for you:

```
{
	"channel_id": "CNGZ5K595",
	"name": "teds",
	"topic": "'No you meant the other ted' - @ted",
	"description": "A channel just for teds, by teds.",
	"date": "2020-11-14"
}
```

* * *

Follow along below to learn what all these fields mean.

### Field guide 

The response format changes depending on which type of analytics you're retrieving.

- Member analytics
- Public channel analytics
  - Public channel analytics metadata

#### Member analytics 

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
| `date_claimed` | `1584810530` | **Coming soon:** The date of the very first time the member signed in to any workspace within the grid organization, presented in seconds since the epoch (UNIX time) |
| `slack_calls_count` | `10` | **Coming soon:** Total number of Slack calls made or joined on a given day, excluding any calls using third party software besides Slack calls |
| `is_active_slack_connect` | `true` | **Coming soon:** Returns `true` when the member is considered active on _Slack Connect_, in that they've read or posted a message to a channel or direct message shared with at least one external workspace |
| `is_active_workflows` | `false` | **Coming soon:** Returns `true` when this member has interacted with at least one workflow on a given day |
| `is_active_apps` | `true` | **Coming soon:** Returns `true` when this member has interacted with a Slack app or custom integration on the given day, or if such an app or integration has performed an action on the user's behalf, such as updating their custom status |
| `search_count` | `3` | **Coming soon:** The number of searches this user performed in Slack on a given day |

#### Conversation analytics fields 

At this time, these analytics are only available for **public channels**. Archived and deleted channels are **not** included.

The account associated with the token making the request must have the _ **Public Channel Management permission** _. By default, the only account with this permission is an organization's primary owner.

If you need more metadata about these public channels, you can quickly fetch it by setting `metadata_only` to `true`. Learn more about the information available.

| Field | Example | About |
| --- | --- | --- |
| `enterprise_id` | `EJB3MZFLM` | Immutable, unique id for the Grid Organization |
| `originating_team` | `{"team_id":"T4C3G041C","name":"arcane"}` | A JSON object with the `team_id` and `name` of the workspace that created this public channel |
| `channel_id` | `CNGL0K091` | The unique id belonging to this channel. The `metadata_only` mode will give you information like the channel's name |
| `channel_type` | `single_workspace_channel` | Indicates which kind of public channel this is: `single_workspace_channel`, `multi_workspace_channel`, or `org_wide_channel`. |
| `visibility` | `public` | Indicates whether the channel is `public` or `private`. Only public channel analytics is available at this time. |
| `shared_with` | `[{"team_id":"T123456","name":"pentameter"},{"team_id":"E123457","name":"markov corp"}]` | Indicates which, if any, workspaces in the same organization this channel is shared with. Presented as an array of JSON objects containing a `team_id` and a `name`. Only works with `channel_type` set to `multi_channel_workspace`. One of the included `team_id` values corresponds to the organization itself, such as this `E123457` example. |
| `is_shared_externally` | `false` | A boolean value revealing whether the channel is shared with workspaces outside of this organization when set to `true`. |
| `externally_shared_with_organizations` | `[{"name":"Away Org","domain":"away.enterprise.slack.com"}]` | Indicates which, if any, external organizations this channel is shared with. Presented as an array of JSON objects containing an organization `name` and a slack `domain`. Only works with `is_shared_externally` set to `true`. |
| `date_created` | `1452719593` | The date and time the channel was first created, presented in seconds since the epoch (UNIX time). |
| `date_last_active` | `1584820530` | The date and time the channel last had a message posted in it, presented in seconds since the epoch (UNIX time). |
| `total_members_count` | `7` | A count of total full members & guests |
| `full_members_count` | `6` | A count of people in this channel who have a Full Member account. |
| `guest_member_count` | `1` | A count of people in this channel who have a guest account. |
| `messages_posted_count` | `223` | A count of total messages posted, including messages from apps and integration on the given day |
| `messages_posted_by_members_count` | `193` | A count of total messages posted from Slack Members & guests (human users only) on the given day |
| `members_who_posted_count` | `3` | A count of the unique number of human users (guests & full members) who posted a message on the given day |
| `members_who_viewed_count` | `225` | A count of the unique human users (guests & full members) who read a message on the given day |
| `reactions_added_count` | `23` | A count of emoji reactions left on any message in channel on that given day by human users |
| `date` | `2020-11-14` | The day in question for the query; the date requested by user for usage metrics |

#### Public channel metadata 

If you're retrieving conversation analytics but want to know more than just the ID of each channel, you can issue a separate request to retrieve bulk metadata about the public channels in the analytics report.

The response is in line-delimited JSON, similar to all other responses in this method, with each line containing an object with these fields. The same [`admin.analytics:read`](/scopes/admin.analytics:read) scope used typically in this method is all you need to retrieve this data.

| Field | Example | About |
| --- | --- | --- |
| `channel_id` | `CAGL0K091` | The channel's unique identifier—the very same used in the analytics above |
| `name` | `ama` | The latest name of the channel |
| `topic` | `Are Jack Kerouac's paragraphs too long?` | The latest channel topic |
| `description` | `Ask our editors all about their favorite literature` | A longer description about the purpose of the channel |
| `date` | `2020-11-14` | These details are current as of this date, which is also when you're making this API call |

## Practical usage example 

To quickly decompress a file and view the JSON directly, use a cURL command like:

```
curl "https://slack.com/api/admin.analytics.getFile" -d "date=2020-11-03&type=member" | gunzip -c > member_analytics_2020_11_03.json
```

...and you'll have a `member_analytics_2020_11_03.json` file in the current operating directory. In some organizations the decompressed size can be quite large. Take care that you have the disk or memory space you need to store and process these files.

Here's an example for conversation analytics:

```
curl "https://slack.com/api/admin.analytics.getFile" -d "date=2021-01-04&type=public_channel" | gunzip -c > public_channel_analytics_2021_01_04.json
```

And another for channel metadata to accompany that conversation analytics request—note that we omit `date`.

```
curl "https://slack.com/api/admin.analytics.getFile" -d "type=public_channel&metadata_only=true" | gunzip -c > public_channel_metadata_2021_01_04.json
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should always check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `metadata_not_available` | Metadata not available for the analytics `type` you provided. |
| `metadata_only_does_not_support_date` | The `metadata_only` field gets the latest metadata file. The `date` field is not supported. |
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
| `member_analytics_disabled` | Member analytics are disabled for your organization. |
| `org_level_email_display_disabled` | This API is unavailable for organizations with a _'Hide email addresses'_ policy. |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Some aspect of authentication cannot be validated. Either the provided token is invalid or the request originates from an IP address disallowed from making the request. |
| `account_inactive` | Authentication token is for a deleted user or workspace when using a `bot` token. |
| `token_revoked` | Authentication token is for a deleted user or workspace or the app has been removed when using a `user` token. |
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

