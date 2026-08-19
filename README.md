# File Upload VAPT Payloads

A general-purpose collection of payloads and test cases for assessing file
upload functionality during authorized web application security testing.

## Structure

```text
file-upload-payloads/
├── 00-valid-baselines/
├── 01-extension-bypass/
├── 02-mime-type-bypass/
├── 03-magic-byte-bypass/
├── 04-content-validation/
├── 05-executable-content/
├── 06-xss/
├── 07-path-filename/
├── 08-overwrite/
└── 09-size-resource/
```

## Categories

| # | Category | Purpose |
|---|---|---|
| `00` | Valid Baselines | Known-good image files used as references |
| `01` | Extension Bypass | Test filename and extension validation |
| `02` | MIME Type Bypass | Test client-controlled `Content-Type` validation |
| `03` | Magic Byte Bypass | Test file-signature validation |
| `04` | Content Validation | Test image structure, metadata, and content processing |
| `05` | Executable Content | Test whether uploaded files can be executed server-side |
| `06` | XSS | Test browser-side execution through uploaded content |
| `07` | Path & Filename | Test filename normalization and path handling |
| `08` | Overwrite | Test filename collisions and unintended file replacement |
| `09` | Size & Resource | Test upload-size and resource-handling limits |

## Usage

Start with the valid baselines in `00-valid-baselines/` and progress through
the relevant test categories.

Each category contains its own `README.md` describing the payloads and the
methodology for validating the corresponding weakness.

Payloads should be tested individually and the application's response should
be verified rather than treating successful upload as proof of a
vulnerability.

## Evidence

For confirmed issues, retain relevant:

- Upload requests
- Upload responses
- Retrieved files
- HTTP responses
- Local file-analysis results
- Screenshots demonstrating the security impact

Use only against systems for which you have explicit authorization to perform
security testing.
