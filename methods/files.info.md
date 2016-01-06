This method returns information about a file in your team.

## Arguments

This method has the URL `https://slack.com/api/files.info` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `files:read`) |
| `file` | `F1234567890` | Required | File to fetch info for |
| `count` | `20` | Optional, default=100 | Number of items to return per page. |
| `page` | `2` | Optional, default=1 | Page number of results to return. |

## Response

The response contains a [file object](/types/file), and a list of comment objects followed by paging information.

```
{
    "ok": true,
    "file": {
        "id" : "F2147483862",
        "timestamp" : 1356032811,

        "name" : "file.htm",
        "title" : "My HTML file",
        "mimetype" : "text\/plain",
        "filetype" : "text",
        "pretty_type": "Text",
        "user" : "U2147483697",

        "mode" : "hosted",
        "editable" : true,
        "is_external": false,
        "external_type": "",

        "size" : 12345,

        "url": "https:\/\/slack-files.com\/files-pub\/T024BE7LD-F024BERPE-09acb6\/1.png",
        "url_download": "https:\/\/slack-files.com\/files-pub\/T024BE7LD-F024BERPE-09acb6\/download\/1.png",
        "url_private": "https:\/\/slack.com\/files-pri\/T024BE7LD-F024BERPE\/1.png",
        "url_private_download": "https:\/\/slack.com\/files-pri\/T024BE7LD-F024BERPE\/download\/1.png",

        "thumb_64": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_64.png",
        "thumb_80": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_80.png",
        "thumb_360": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_360.png",
        "thumb_360_gif": "https:\/\/slack-files.com\/files-tmb\/T024BE7LD-F024BERPE-c66246\/1_360.gif",
        "thumb_360_w": 100,
        "thumb_360_h": 100,

        "permalink" : "https:\/\/tinyspeck.slack.com\/files\/cal\/F024BERPE\/1.png",
        "edit_link" : "https:\/\/tinyspeck.slack.com\/files\/cal\/F024BERPE\/1.png/edit",
        "preview" : "&lt;!DOCTYPE html&gt;\n&lt;html&gt;\n&lt;meta charset='utf-8'&gt;",
        "preview_highlight" : &lt;div class=\"sssh-code\"&gt;&lt;div class=\"sssh-line\"&gt;&lt;pre&gt;&lt;!DOCTYPE html...",
        "lines" : 123,
        "lines_more": 118,

        "is_public": true,
        "public_url_shared": false,
        "channels": ["C024BE7LT", ...],
        "groups": ["G12345", ...],
        "initial_comment": {...},
        "num_stars": 7,
        "is_starred": true
    },
    "comments": [
        {
            "id": "Fc027BN9L9",
            "timestamp": 1356032811,
            "user": "U2147483697",
            "comment": "This is a comment"
        },
        ...
    ],
    "paging": {
        "count": 100,
        "total": 2,
        "page": 1,
        "pages": 0
    }
}
```

The [file object](/types/file) contains information about the uploaded file.

Each comment object in the comments array contains details about a single comment. Comments are returned oldest first.

The paging information contains the `count` of comments returned, the `total` number of comments, the `page` of results returned in this response and the total number of `pages` available. Please note that the max `count` value is `1000` and the max `page` value is `100`.

## Errors

This table lists the expected errors that this method will return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `file_not_found` | Value passed for `file` was invalid |
| `file_deleted` | The requested file has been deleted |
| `not_authed` | No authentication token provided. |
| `invalid_auth` | Invalid authentication token. |
| `account_inactive` | Authentication token is for a deleted user or team. |
| `user_is_bot` | This method cannot be called by a bot user. |

