# 10 — DateTime and Text in Pandas

Learn to parse, inspect, transform, and analyze dates, times, and text columns using Pandas.

## Learning objectives

By the end of this module, you should be able to:
- Parse strings into datetime values with `pd.to_datetime()`.
- Extract date components with `.dt`.
- Filter rows by dates and create time-based features.
- Work with timedeltas, date ranges, and time zones.
- Clean text with the Pandas `.str` accessor.
- Search, replace, split, extract, and validate text.
- Combine datetime and text transformations in a data-cleaning workflow.

## 1. Setup

```python
import pandas as pd
```

Pandas provides the `.dt` accessor for datetime-like Series and `.str` for string-like Series.

## 2. Parsing dates and times

```python
dates = pd.Series(["2026-01-05", "2026-02-17", "not-a-date"])
parsed = pd.to_datetime(dates, errors="coerce")
```

- `errors="raise"`: raise an error when a value cannot be parsed.
- `errors="coerce"`: convert invalid values to `NaT`.
- `errors="ignore"`: deprecated in newer Pandas versions; prefer explicit parsing and validation.
- Use `format=` when the input format is known, such as `format="%d/%m/%Y"`.

Common format codes:
- `%Y`: four-digit year
- `%y`: two-digit year
- `%m`: month
- `%d`: day
- `%H`: hour (24-hour clock)
- `%M`: minute
- `%S`: second
- `%b`: abbreviated month name
- `%B`: full month name

```python
pd.to_datetime("14/10/2026", format="%d/%m/%Y")
```

## 3. Datetime properties with `.dt`

For a datetime Series:

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day"] = df["date"].dt.day
df["day_name"] = df["date"].dt.day_name()
df["quarter"] = df["date"].dt.quarter
df["is_month_end"] = df["date"].dt.is_month_end
```

Other useful properties include `.dt.hour`, `.dt.minute`, `.dt.dayofweek` (Monday is 0), `.dt.is_month_start`, and `.dt.days_in_month`.

## 4. Filtering by date

```python
df["date"] = pd.to_datetime(df["date"])
recent = df.loc[df["date"] >= "2026-01-01"]
window = df.loc[df["date"].between("2026-01-01", "2026-03-31")]
```

For timestamps, a half-open interval is often safer for a whole month:
```python
january = df.loc[
    (df["timestamp"] >= "2026-01-01") &
    (df["timestamp"] < "2026-02-01")
]
```
This includes every time on January 31 without needing to specify `23:59:59`.

## 5. Timedeltas and date ranges

```python
df["delivery_days"] = (
    pd.to_datetime(df["delivered_at"]) -
    pd.to_datetime(df["ordered_at"])
).dt.days

calendar = pd.date_range("2026-01-01", periods=7, freq="D")
```

A `Timedelta` represents a duration. `pd.date_range()` generates regular timestamps. Common frequencies include `"D"` (day), `"h"` (hour), `"W"` (week), `"MS"` (month start), and `"ME"` (month end; availability depends on Pandas version).

## 6. Time zones

```python
utc = pd.to_datetime(df["timestamp"], utc=True)
india_time = utc.dt.tz_convert("Asia/Kolkata")
```

Use `utc=True` when input timestamps include offsets or should be normalized to UTC. To convert a timezone-naive local time, first localize it to its source zone, then convert:
```python
local = pd.to_datetime(df["local_time"]).dt.tz_localize("Asia/Kolkata")
```
Ambiguous and nonexistent local times can occur around daylight-saving transitions in some time zones; handle them explicitly where relevant.

## 7. Cleaning text with `.str`

```python
s = df["name"].astype("string")
df["name_clean"] = s.str.strip().str.title()
```

Useful methods:
- `.str.lower()`, `.str.upper()`, `.str.title()`, `.str.capitalize()`
- `.str.strip()`, `.str.lstrip()`, `.str.rstrip()`
- `.str.contains(pattern, case=False, na=False)`
- `.str.startswith(prefix, na=False)`, `.str.endswith(suffix, na=False)`
- `.str.replace(old, new, regex=False)`
- `.str.split(delimiter, expand=True)`
- `.str.len()`, `.str.slice(start, stop)`
- `.str.extract(regex)` for capturing groups
- `.str.fullmatch(regex, na=False)` to validate an entire string

Use `astype("string")` when missing values should remain as Pandas `<NA>` rather than the text `"nan"`.

## 8. Literal versus regular-expression replacement

```python
s.str.replace(".", "-", regex=False)  # replace literal periods
s.str.replace(r"\d+", "#", regex=True)  # replace digit sequences
```

In regular expressions, characters such as `.`, `+`, `?`, `(`, and `[` have special meanings. Set `regex=False` for literal replacements.

## 9. Splitting and extracting

```python
parts = df["full_name"].astype("string").str.split(" ", n=1, expand=True)
df[["first_name", "remaining_name"]] = parts

emails = df["email"].astype("string").str.extract(
    r"(?P<username>[^@]+)@(?P<domain>[^@]+)"
)
```

Regex extraction is useful for structured strings, but simple patterns are not a complete substitute for formal email validation.

## 10. Best practices

1. Preserve the raw column before transforming it.
2. Parse dates early and inspect invalid values that became `NaT`.
3. Specify `format=` when the format is known and consistent.
4. Use consistent timezone conventions for event timestamps.
5. Prefer vectorized `.dt` and `.str` methods over row-by-row Python loops.
6. Use `na=False` when missing text should not match a filter.
7. Be deliberate about regex versus literal replacement.
8. Validate outputs and inspect representative rows after transformations.

## 11. Mini workflow

```python
df = pd.DataFrame({
    "created": ["2026-01-02", "bad-date", "2026-01-20"],
    "customer": ["  ALICE ", "Bob", "carol  "],
    "ticket": ["BUG-102", "REQ-204", "BUG-305"],
})
df["created"] = pd.to_datetime(df["created"], errors="coerce")
df["customer"] = df["customer"].astype("string").str.strip().str.title()
df["ticket_type"] = df["ticket"].str.extract(r"^([A-Z]+)-")
df["created_month"] = df["created"].dt.month
```

## Further practice

Complete [`practice.md`](practice.md), check [`solutions.md`](solutions.md), and run the examples in [`examples.ipynb`](examples.ipynb).
