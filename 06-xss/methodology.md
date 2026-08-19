# 06 — XSS

## Payloads

```text
06-xss/
├── svg/
│   ├── basic.svg
│   └── script.svg
├── html-content/
│   └── test.html
├── filename/
│   └── <img src=x onerror=alert(document.domain)>.jpg
└── metadata/
    └── exif-xss.jpg
````

### Payload Categories

* `svg/` — Tests XSS through uploaded SVG content.
* `html-content/` — Tests whether uploaded HTML content is rendered as HTML.
* `filename/` — Tests whether an uploaded filename is reflected into HTML without encoding.
* `metadata/` — Tests whether attacker-controlled image metadata is extracted and rendered unsafely.

## Methodology

1. Upload the appropriate payload through the application's file-upload functionality.
2. Record the upload response and generated file URL/path.
3. Locate every place where the uploaded file, filename, or metadata is displayed.
4. Open the relevant page or directly request the uploaded file in a browser.
5. Check whether the payload is interpreted as executable HTML/JavaScript.
6. Use a harmless proof such as `alert(document.domain)` to confirm execution.

## Successful Exploitation

A successful XSS test requires **browser-side execution** of the supplied payload.

For example:

```text
Upload SVG
    ↓
SVG stored
    ↓
SVG served/embedded
    ↓
Browser interprets SVG
    ↓
JavaScript executes
    ↓
alert(document.domain)
```

For filename or metadata testing:

```text
Attacker-controlled value
        ↓
Application displays value
        ↓
Value inserted into HTML without encoding
        ↓
Browser interprets markup
        ↓
JavaScript executes
```

### Not Vulnerable / Not Confirmed

The following alone do **not** confirm XSS:

* File upload succeeds.
* File is stored.
* File can be downloaded.
* Payload appears as plain text.
* Payload source is returned instead of being interpreted.
* Browser downloads the file instead of rendering it.
* `Content-Type: image/jpeg` is returned and the payload is not executed.

Document the exact rendering context and execution result when reporting a
confirmed finding.
<<<<<<< HEAD

=======
>>>>>>> 39c813b (Fixed whitespaces in the methodology.md)
