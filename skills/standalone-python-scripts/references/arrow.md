# Arrow: Better Dates & Times for Python

Arrow provides a sensible, human-friendly approach to creating, manipulating, formatting and converting dates and times.

## Basic Usage

```python
import arrow

# Get current time
now = arrow.now()

# Parse dates
date = arrow.get("2023-12-25")

# Format output
now.format("YYYY-MM-DD HH:mm:ss")  # "2023-12-25 14:30:00"
now.format("dddd, MMM Do")  # "Monday, Dec 25th"
```

## Key Features

- **Human-friendly**: Intuitive API for common operations
- **Timezone support**: Built-in timezone handling and conversion
- **Relative time**: "2 hours ago", "in 3 days"
- **Span support**: Floor/ceil to hour, day, month, etc.
- **Localization**: Support for multiple languages

## Common Patterns

### Timezone handling
```python
utc = arrow.utcnow()
local = utc.to("US/Pacific")
```

### Relative time
```python
past = arrow.get("2023-01-01")
past.humanize()  # "X years ago"
```

### Date arithmetic
```python
future = arrow.now().shift(days=7, hours=3)
past = arrow.now().shift(months=-2)
```

### Date ranges
```python
start = arrow.get("2023-01-01")
end = arrow.get("2023-12-31")
for r in arrow.Arrow.span_range("month", start, end):
    print(r[0].format("MMMM YYYY"))
```

## Reference

Full documentation: https://arrow.readthedocs.io/
