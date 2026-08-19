# Extension Bypass

Tests filename-based upload validation.

## Categories

### double-extension
Tests whether validation checks the first or final extension.

### multiple-dot
Tests filename parsing and normalization around multiple dots.

### case-variation
Tests case-sensitive extension validation.

### trailing-dot
Tests handling of trailing dots and filename normalization.

### trailing-space
Tests whitespace normalization.

### null-byte
Legacy null-byte test cases represented using `%00`.
Modern applications normally reject these.

### extension-confusion
Tests alternate/common extensions and allowlist consistency.

## Expected secure behavior

The application should determine the actual file type using
content/signature validation rather than trusting the filename.

Record:
- HTTP status
- Upload response
- Stored filename
- Whether the upload is accepted/rejected
- Filename normalization
- Retrieved Content-Type
- Whether the original content is preserved
