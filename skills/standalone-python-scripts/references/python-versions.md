# Python Version Support

When setting `requires-python` in the PEP 723 metadata, ensure the target version is still supported.

## Support Policy

Only target Python versions that meet **both** criteria:
1. **Not end-of-life** — the version has not reached its EOL date
2. **At least one year until EOL** — the version has more than 12 months of support remaining

**Exception:** Older versions may be targeted if explicitly requested by the user.

## Checking Version Status

The official Python Developer's Guide maintains the current version support table:

**URL:** https://devguide.python.org/versions/

This page shows:
- Which versions are currently supported
- End-of-life dates for each version
- Release managers and status (feature/bugfix/security/EOL)

## Example

As of 2024-2025:
- Python 3.12: Supported until October 2028 ✓
- Python 3.11: Supported until October 2027 ✓
- Python 3.10: Supported until October 2026 ✓
- Python 3.9: End-of-life October 2025 ✗
- Python 3.8: End-of-life October 2024 ✗

Always check the current status before setting `requires-python`.

## Setting requires-python

```python
# /// script
# requires-python = ">=3.12"
# dependencies = [...]
# ///
```

Use a version that has long-term support remaining to avoid premature obsolescence.
