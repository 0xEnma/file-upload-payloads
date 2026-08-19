# 02 — MIME Type Bypass

## Objective

Test whether the application trusts the client-supplied `Content-Type` header
or independently verifies that the declared MIME type matches the actual
uploaded file.

## Content-Type Values

    image/jpeg
    image/png
    image/gif
    image/webp
    image/svg+xml
    application/octet-stream
    text/plain
    text/html
    application/json
    application/pdf

## Baseline Files

Use the files from `00-valid-baselines/`:

    valid.jpg
    valid.png
    valid.gif
    valid.webp

## Test Combinations

| # | Baseline | Content-Type |
|---|---|---|
| 01 | `valid.jpg` | `image/jpeg` |
| 02 | `valid.jpg` | `image/png` |
| 03 | `valid.jpg` | `image/gif` |
| 04 | `valid.jpg` | `image/webp` |
| 05 | `valid.jpg` | `image/svg+xml` |
| 06 | `valid.jpg` | `application/octet-stream` |
| 07 | `valid.jpg` | `text/plain` |
| 08 | `valid.jpg` | `text/html` |
| 09 | `valid.png` | `image/png` |
| 10 | `valid.png` | `image/jpeg` |
| 11 | `valid.png` | `image/gif` |
| 12 | `valid.png` | `image/webp` |
| 13 | `valid.png` | `application/octet-stream` |
| 14 | `valid.gif` | `image/gif` |
| 15 | `valid.gif` | `image/jpeg` |
| 16 | `valid.gif` | `image/png` |
| 17 | `valid.webp` | `image/webp` |
| 18 | `valid.webp` | `image/jpeg` |
| 19 | `valid.webp` | `image/png` |

## What to Look For

For each combination, record:

- Upload accepted or rejected
- HTTP status code
- Error/success response
- Generated filename
- Whether the file is stored
- Whether the file can be retrieved
- Response `Content-Type`
- Actual file type after retrieval
- Whether the file was modified or re-encoded

## Successful Bypass Indicators

An interesting result is when the declared MIME type differs from the actual
file type but the application still accepts and stores the file.

Example:

    File: valid.jpg
    Content-Type: image/png
    Actual content: JPEG
    Result: Upload accepted

Also note cases where a valid image is accepted with a non-image MIME type,
such as:

    File: valid.jpg
    Content-Type: application/octet-stream
    Result: Upload accepted

Acceptance alone does not establish a high-impact vulnerability. Check what
happens to the uploaded file after acceptance:

    Upload
      ↓
    Stored?
      ↓
    Retrievable?
      ↓
    Content modified/re-encoded?
      ↓
    Response Content-Type?
      ↓
    Processed by another component?
      ↓
    Security impact?

Do not claim code execution or other impact unless it is actually demonstrated.
