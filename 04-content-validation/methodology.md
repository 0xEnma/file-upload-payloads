# 04 — Content Validation

## Objective

Test whether uploaded images are actually parsed and validated rather than
only checking the filename, MIME type, or magic bytes.

## Payloads

### `malformed-image/`

Contains JPEG files with valid JPEG characteristics but intentionally damaged
or incomplete image structures.

```text
corrupted-data.jpg
invalid-structure.jpg
minimal-invalid.jpg
missing-eoi.jpg
truncated.jpg
````

**Purpose:** Test whether the application can detect images that cannot be
properly decoded.

### `metadata/`

Contains images with controlled EXIF metadata.

```text
exif-basic.jpg
exif-gps.jpg
exif-long-value.jpg
metadata-rich.jpg
```

**Purpose:** Determine whether metadata is stripped, preserved, safely
processed, or exposed elsewhere.

### `polyglot/`

Contains valid images with controlled trailing data.

```text
gif-with-trailing-data.gif
jpeg-with-trailing-data.jpg
png-with-trailing-data.png
```

**Purpose:** Determine whether additional data is removed, preserved, or
processed unexpectedly.

---

## Methodology

For each payload:

1. Upload it through the normal file-upload functionality.
2. Record the HTTP response and generated filename/URL.
3. If accepted, retrieve the uploaded file.
4. Compare the original and retrieved files.
5. Determine whether the application rejected, modified, re-encoded, or
   preserved the payload.
6. Check whether any subsequent image-processing functionality handles the
   payload differently.

Useful local checks:

```bash
file downloaded-file
xxd -l 32 downloaded-file
exiftool downloaded-file
```

For polyglot files:

```bash
grep -a "VAPT-POLYGLOT-TEST" downloaded-file
```

## Successful Exploitation / Validation Bypass

Do **not** consider `200 OK` or `201 Created` alone sufficient.

A meaningful result is when malformed or specially constructed content is
accepted and:

```text
Upload
  ↓
Accepted
  ↓
Stored
  ↓
Original malformed/additional content preserved
  ↓
Application processes or exposes it unexpectedly
```

For metadata, look for controlled metadata being unexpectedly exposed or
rendered in an unsafe context.

For malformed images, look for the application treating an undecodable file
as a valid image or passing it to downstream processing.

For polyglots, look for different application components interpreting the
same file differently in a security-relevant way.

Document the exact observed behavior and demonstrated impact rather than
assuming that acceptance alone constitutes a vulnerability.

```
