# uv Script Support

uv provides native support for running Python scripts with inline dependencies (PEP 723).

## Running Scripts

```bash
# Run a script with inline dependencies
uv run script.py

# Explicitly use script mode
uv run --script script.py
```

## Shebang Line

Use this shebang for direct execution:

```python
#!/usr/bin/env -S uv run --script
```

This allows:
```bash
chmod +x script.py
./script.py
```

## Dependency Resolution

uv automatically:
1. Reads the PEP 723 metadata block
2. Creates an isolated environment
3. Installs dependencies
4. Runs the script

## Caching

uv caches environments, so subsequent runs are fast. The cache is keyed by:
- Script path
- Metadata block content
- Python version

## Reference

Full documentation: https://docs.astral.sh/uv/guides/scripts/
