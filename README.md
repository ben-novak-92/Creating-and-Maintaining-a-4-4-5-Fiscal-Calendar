# 4-4-5 Fiscal Calendar (Databricks)

## Overview

This project generates a **4-4-5 fiscal calendar** at daily grain and stores it as a Delta table in Databricks.

The calendar:

- Follows a 4-4-5 weekly structure per quarter  
- Produces 52-week (364-day) standard years  
- Automatically generates 53-week (371-day) fiscal years  
- Supports manual fiscal year shifting via `SHIFT_DAYS`

---

## Configuration

Key parameters:

```python
START_DATE = date(2000, 1, 1)   # FY1 Day 1
NUM_YEARS  = 100                # Number of fiscal years
SHIFT_DAYS = 0                  # Manual global shift (+/- days)
TARGET_TABLE = "main.shared.fiscal_calendar_445"

## Configuration

- **`START_DATE`** defines FY1 start exactly.
- **`SHIFT_DAYS`** shifts the entire fiscal calendar forward or backward.
- Leap years are computed **algorithmically** (no hardcoding).

---

## Output Columns

- `calendar_date`
- `fiscal_year`
- `fiscal_quarter`
- `fiscal_month`
- `fiscal_period`
- `fiscal_week_of_year`
- `fiscal_week_of_quarter`
- `fiscal_day_of_year`
- `is_53_week_year`
- `fiscal_year_label`
- `fiscal_quarter_label`
- `fiscal_month_label`

---

## Leap Year Logic

Fiscal years are built using a rolling **364-day structure**.

When accumulated solar drift exceeds one full week, a **53rd week (371 days total)** is inserted automatically.

No manual maintenance required.
