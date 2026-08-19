# 07 — Path & Filename Validation

## Objective

Test whether user-controlled filenames are safely handled during file upload,
storage, and path construction.

## Payloads

`filename-wordlist.txt` contains test cases for:

- Path traversal (`../`, encoded separators)
- Path separator variations
- Dot and path normalization
- Trailing dots/spaces
- Special and reserved filenames
- Hidden filenames
- Case variations

## Methodology

Use the wordlist with Burp Intruder against the upload request, replacing only
the `filename` value.

Example:

    Content-Disposition: form-data; name="file"; filename="§image.jpg§"

Record the response for each payload.

## Successful Exploitation

A vulnerability is confirmed when a filename causes the application to:

- Write a file outside the intended upload directory.
- Access or overwrite an unintended file.
- Resolve the uploaded file to an unexpected filesystem/resource path.
- Bypass intended filename/path restrictions.

An unusual filename being accepted by itself does not confirm a vulnerability.
