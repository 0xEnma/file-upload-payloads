# 05 — Executable Content

## Payloads

```text
05-executable-content/
├── php/
│   └── test.php
├── jsp/
│   └── test.jsp
├── asp/
│   └── test.asp
├── aspx/
│   └── test.aspx
├── cgi/
│   └── test.cgi
└── script/
    └── test.sh
````

The payloads contain harmless execution markers:

```text
VAPT-PHP-EXECUTION-TEST-001
VAPT-JSP-EXECUTION-TEST-001
VAPT-ASP-EXECUTION-TEST-001
VAPT-ASPX-EXECUTION-TEST-001
VAPT-CGI-EXECUTION-TEST-001
VAPT-SCRIPT-EXECUTION-TEST-001
```

Use the payload corresponding to the server-side technology being tested.

## Methodology

1. Upload the appropriate executable-content payload.
2. Record the upload response and generated file path/URL.
3. Request the uploaded file.
4. Examine the response body and `Content-Type`.
5. Determine whether the payload was:

   * Rejected.
   * Stored and served as static content.
   * Returned as source code.
   * Executed by the server-side runtime.

## Successful Execution

Execution is confirmed when the response contains the **output of the
execution marker** rather than the payload's source code.

Example:

```text
VAPT-PHP-EXECUTION-TEST-001
```

indicates execution.

Whereas receiving:

```text
<?php
echo "VAPT-PHP-EXECUTION-TEST-001";
?>
```

indicates that the source was returned and the payload was **not executed**.

A successful upload (`200`, `201`, etc.) alone does not confirm server-side
execution.

```
