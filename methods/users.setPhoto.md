This method allows the user to set their profile image. The caller can pass image data via `image`.

Providing a "crop box" with `crop_x`, `crop_y`, and `crop_w` is optional. Otherwise, the whole image will be used. If cropping instructions are not specified and the source image is not _square_, the image will be letterboxed, just like your favorite old laserdiscs.

Please limit your images to a maximum size of 1024 by 1024 pixels. 512x512 pixels is the minimum.

To remove a profile image, use the companion method [`users.deletePhoto`](/methods/users.deletePhoto).

## Arguments

This method has the URL `https://slack.com/api/users.setPhoto` and follows the [Slack Web API calling conventions](/web#basics).

| Argument | Example | Required | Description |
| --- | --- | --- | --- |
| `token` | `xxxx-xxxxxxxxx-xxxx` | Required | Authentication token (Requires scope: `users.profile:write`) |
| `image` | `...` | Required | File contents via `multipart/form-data`. |
| `crop_x` | `10` | Optional | X coordinate of top-left corner of crop box |
| `crop_y` | `15` | Optional | Y coordinate of top-left corner of crop box |
| `crop_w` | `100` | Optional | Width/height of crop box (always square) |

Use this method with a HTTP POST and provide your HTTP `Content-type` as `multipart/form-data`.

When providing the `image` parameter, provide your image data directly but present its correct `Content-type`, such as `image/gif`, `image/jpeg`, `image/png`, etc.

## Response

```
{
    "ok": true
}
```

## Errors

This table lists the expected errors that this method could return. However, other errors can be returned in the case where the service is down or other unexpected factors affect processing. Callers should _always_ check the value of the `ok` params in the response.

| Error | Description |
| --- | --- |
| `bad_image` | The uploaded image could not be processed - try passing a JPEG, GIF or PNG |
| `too_large` | The uploaded image had excessive dimensions |
| `too_many_frames` | An animated GIF with too many frames was uploaded |
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

