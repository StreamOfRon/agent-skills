# Click: Command Line Interface Creation

Click is a Python package for creating command line interfaces with minimal code.

## Basic Usage

```python
import click

@click.command()
@click.option("--name", default="World", help="Name to greet")
def hello(name):
    """Simple program that greets NAME."""
    click.echo(f"Hello, {name}!")

if __name__ == "__main__":
    hello()
```

## Key Features

- **Decorators**: `@click.command()`, `@click.option()`, `@click.argument()`
- **Automatic help**: Generated from docstrings and option help text
- **Type conversion**: Automatic conversion of CLI arguments to Python types
- **Nested commands**: Support for subcommands and command groups
- **Progress bars**: Built-in progress bar support
- **Terminal colors**: ANSI color support via `click.secho()`

## Common Patterns

### Options with flags
```python
@click.option("--verbose", is_flag=True, help="Enable verbose mode")
```

### Required options
```python
@click.option("--name", required=True, help="Your name")
```

### Multiple values
```python
@click.option("--item", multiple=True, help="Items to process")
```

### Choice validation
```python
@click.option("--format", type=click.Choice(["json", "yaml", "text"]))
```

## Reference

Full documentation: https://click.palletsprojects.com/
