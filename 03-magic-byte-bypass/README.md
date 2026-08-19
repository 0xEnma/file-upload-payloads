# 03 — Magic Byte / File Signature Validation

## Objective

Determine whether the application verifies the actual file signature (magic
bytes) of an uploaded file instead of relying only on the filename and
client-supplied MIME type.

---

## Payloads

The payloads are stored in:

    signature-mismatch/

Current payloads:

    gif-as-jpeg.jpg
    jpeg-as-png.png
    jpeg-as-webp.webp
    png-as-jpeg.jpg

Each payload has a filename and initial signature representing one image
format while the underlying file content originates from another format.

| Payload | Filename | Declared Format | Actual Content |
|---|---|---|---|
| gif-as-jpeg.jpg | JPEG | JPEG | GIF |
| jpeg-as-png.png | PNG | PNG | JPEG |
| jpeg-as-webp.webp | WebP | WebP | JPEG |
| png-as-jpeg.jpg | JPEG | JPEG | PNG |

These files are intentionally malformed and are used only for upload
validation testing.

---

## Methodology

For each payload:

1. Upload the file through the application's normal file-upload
   functionality.
2. Keep the filename unchanged.
3. Set the multipart `Content-Type` to the MIME type corresponding to the
   filename.
4. Record whether the upload is accepted or rejected.
5. If accepted, record the generated filename or media URL.
6. Retrieve the uploaded file.
7. Compare the retrieved file with the original payload.
8. Determine whether the application:
   - rejects the file,
   - stores the malformed content,
   - validates and re-encodes the image,
   - or otherwise modifies the file.

The purpose is to determine whether the application independently verifies
the actual file format.

---

## Expected Secure Behavior

A secure implementation should not trust the filename or MIME type alone.

For example:

    Filename:       image.png
    Content-Type:   image/png
    Actual content: JPEG

The application should reject the file or safely process it using independent
file-type validation.

A valid implementation may also decode and re-encode accepted images, which
can remove the original malformed content.

---

## Indicators of a Successful Bypass

A potential magic-byte validation bypass is indicated when a mismatched
payload is:

- Accepted by the upload endpoint.
- Successfully stored.
- Retrievable after upload.
- Preserved in its mismatched form.
- Not independently validated or re-encoded.

Example:

    Filename:       image.png
    Content-Type:   image/png
    Actual content: JPEG

            ↓

    Upload accepted
            ↓
    File stored
            ↓
    Original mismatched content preserved

An HTTP `201 Created` response alone is NOT sufficient to confirm a
magic-byte validation bypass.

---

## Verification

### 1. Upload Response

Record:

- HTTP status
- Response body
- Generated filename
- Returned media URL

### 2. Retrieve the File

Request the generated media URL and record:

- HTTP status
- Response `Content-Type`
- Content length
- Whether the file can be downloaded

### 3. Identify the Retrieved File

Use local file identification:

    file downloaded-file

Inspect the file signature:

    xxd -l 32 downloaded-file

Compare the retrieved file with the original payload.

### 4. Check for Re-Encoding

Determine whether the application transformed the malformed file into a valid
image.

Example:

    Malformed file
          ↓
    Application processing
          ↓
      Valid image

If the application safely re-encodes the image, this is different from
simply storing the malformed input.

---

## Result Classification

### Secure

The application:

- Rejects the mismatched file, or
- Independently detects the actual file type, or
- Safely decodes and re-encodes the image.

### Potential Weakness

The application accepts and stores the mismatched file, but:

- The file is stored in a non-executable location.
- It is safely served.
- No additional security impact is demonstrated.

Document the behavior and assess it according to the application's intended
upload restrictions.

### Confirmed Magic-Byte Validation Bypass

The application accepts a file whose filename and MIME type indicate an
allowed image format while its actual signature/content represents another
format, and preserves the mismatched content without independent validation
or safe re-encoding.

---

## Evidence to Capture

For each interesting result, capture:

### Upload Request

    POST /api/media/upload

Record:

    filename="..."
    Content-Type: ...

### Upload Response

Record:

    HTTP status
    Generated filename
    Media URL

### Retrieval Response

Record:

    GET /api/media/...

    HTTP status
    Content-Type
    Content-Length

### Local Analysis

Record the output of:

    file downloaded-file

and:

    xxd -l 32 downloaded-file

Retain the original payload for comparison.

---

## Impact Assessment

A magic-byte validation bypass does not automatically mean Remote Code
Execution.

Determine what happens after the upload:

    Upload accepted
          ↓
    File stored
          ↓
    File retrievable
          ↓
    File processed?
          ↓
    File re-encoded?
          ↓
    File rendered?
          ↓
    File interpreted?
          ↓
    Security impact

Only report the demonstrated impact.

---

## Relationship to Other Upload Tests

    01-extension-bypass/
        Filename validation

    02-mime-type-bypass/
        Client-supplied MIME validation

    03-magic-byte-bypass/
        Actual file signature validation

    04-content-validation/
        Image/content parsing and validation

    05-executable-content/
        Server-side execution

    06-xss/
        Client-side execution through uploaded content

---

## Reporting Checklist

If a bypass is confirmed, document:

- Affected endpoint
- Authentication requirements
- Payload used
- Filename
- Declared MIME type
- Actual file type
- Upload response
- Retrieval response
- Whether content was modified
- Whether content was publicly accessible
- Demonstrated security impact
- Recommended remediation

Do not store live JWTs, API keys, passwords, or other credentials in this
repository.
