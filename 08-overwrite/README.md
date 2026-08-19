# 08 — File Overwrite / Filename Collision

## Objective

Test whether uploaded files can overwrite existing files because of predictable
or insufficiently unique filename generation.

## Methodology

1. Upload a baseline file and record its generated filename/path.
2. Upload a different file using the same filename or a predictable filename.
3. Retrieve the original resource.
4. Compare the content before and after the second upload.
5. Repeat where applicable with predictable generated filenames or IDs.
6. Test concurrent uploads if filename generation appears collision-prone.

## Successful Exploitation

A vulnerability is confirmed when an upload causes an existing file to be
replaced or modified unintentionally.

Example:

    Upload A
        ↓
    image.jpg stored
        ↓
    Upload B using the same/colliding name
        ↓
    image.jpg retrieved
        ↓
    Content is now B

### Additional Impact

Assess whether the overwritten file:

- Belongs to another user.
- Is referenced by another application function.
- Is an application-controlled resource.
- Can affect content displayed to other users.

An identical generated filename or a successful upload alone does not confirm
an overwrite vulnerability. The existing file's content must be demonstrated
to have been replaced or modified.
