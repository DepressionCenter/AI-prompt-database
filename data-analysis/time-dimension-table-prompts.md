![Depression Center Logo](https://github.com/DepressionCenter/.github/blob/main/images/EFDCLogo_375w.png "depressioncenter.org")


# **AI Prompts Database**
#### *__Real-world examples of AI prompts from the University of Michigan.__*

<br />

## Time Dimension Table Prompts

### Contributors:
+ [Gabriel Mongefranco](https://gabriel.mongefranco.com) ([@gabrielmongefranco](https://github.com/gabrielmongefranco)), Eisenberg Family Depression Center, University of Michigan

### Description
Use this prompt to generate code for date and time dimensions (Calendar and Time Slots tables) to use in dashboards.

This example consits of multiple prompts. The first is a base prompt with the logic to be implemented. The others are specific targets (such as SQL, JavaScript, Lua, Pyhon, R, PowerShell, Bash). To use it, you must combine the base prompt with the target-specific prompt.

### Base Prompt Template
**For: GPT, Gemini, Claude, Others**  <br />
**Prompt:**<br />
<pre><code>
Build two time dimension tables according to the following specifications. The target environment and output format are defined in the appended block below — follow those instructions exactly for implementation.

---

## DATE RANGE

Determine the date range dynamically at runtime using the current date:

- Start date: January 1st of (current year minus 10)
- End date: December 31st of (current year plus 5)
- Do not hardcode any specific years. The range must always be calculated fresh each time the script runs.

---

## `Calendar` TABLE

### Natural Key
- `Date`: string, format `"YYYY-MM-DD"` (e.g., `"2025-01-01"`)
  This is the primary/join key. No separate ID column needed.

### Columns
| Column | Type | Description |
|---|---|---|
| `Date` | string `"YYYY-MM-DD"` | Natural key, join key |
| `Year` | integer | Calendar year (e.g., 2025) |
| `AcademicYear` | string `"YYYY-YY"` | Sep 1 of year X through Aug 31 of year X+1. Label is `"YYYY-YY"` of that range (e.g., `"2024-25"`) |
| `FiscalYear` | string `"FYyyyy"` | Jul 1 of year X through Jun 30 of year X+1. Label is `"FY"` + the year in which Jun 30 falls (e.g., `"FY2025"`) |
| `Quarter` | integer 1–4 | Calendar quarter (Q1 = Jan–Mar) |
| `CalendarWeek` | integer 1–53 | ISO 8601 week number |
| `AcademicWeek` | integer 1–52 (or 53) | Week offset from Sep 1 of the current academic year |
| `FiscalWeek` | integer 1–52 (or 53) | Week offset from Jul 1 of the current fiscal year |

### Calculation Rules

**ISO 8601 `CalendarWeek`:** The week containing the first Thursday of the year is Week 1. Monday is the first day of the week.

**`AcademicWeek` and `FiscalWeek`** are simple day-offset weeks (not ISO):
```
week = floor(daysSinceYearStart / 7) + 1
```
where `daysSinceYearStart` is the number of days since Sep 1 (academic) or Jul 1 (fiscal) of the current cycle.

**Timezone safety:** Use UTC-based date methods throughout to prevent off-by-one errors from local timezone offsets.

---

## `TimeSlots` TABLE

### Natural Key
- `FiveMinuteIntervalStart`: string, format `"HH:MM"` (e.g., `"00:00"`, `"13:35"`)
  This is the primary/join key. No separate ID column needed.

### Columns
| Column | Type | Description |
|---|---|---|
| `FiveMinuteIntervalStart` | string `"HH:MM"` | Natural key, join key |
| `FiveMinuteIntervalEnd` | string `"HH:MM"` | 4 minutes after start |
| `FiveMinuteInterval` | integer 1–288 | Sequential daily counter |
| `ThirtyMinuteIntervalStart` | string `"HH:MM"` | Start of the containing 30-min block |
| `ThirtyMinuteIntervalEnd` | string `"HH:MM"` | 29 minutes after the 30-min block start |
| `ThirtyMinuteInterval` | integer 1–48 | Sequential daily counter |
| `Hour` | integer 0–23 | Hour in 24-hour format |

The table contains exactly **288 rows** (24 hours × 12 five-minute intervals per hour), ordered `"00:00"` through `"23:55"`. All intervals reset daily starting at midnight.

### Calculation Rules

- Every 6 consecutive five-minute rows share the same `ThirtyMinuteInterval` value.
- All `HH` and `MM` values must be zero-padded to two digits.

**`FiveMinuteIntervalEnd`** is always 4 minutes after `FiveMinuteIntervalStart`:
```
"00:00" → "00:04"
"13:35" → "13:39"
"23:55" → "23:59"
```

**`ThirtyMinuteIntervalEnd`** is always 29 minutes after `ThirtyMinuteIntervalStart`:
```
"00:00" → "00:29"
"00:30" → "00:59"
"23:30" → "23:59"
```

### CRITICAL: Interval Boundary Rule

End values must be the **last minute inside the block**, not the first minute of the next block.

- `FiveMinuteIntervalEnd` for `"13:35"` must be `"13:39"`, **NOT** `"13:40"`.
- `ThirtyMinuteIntervalEnd` for `"13:30"` must be `"13:59"`, **NOT** `"14:00"`.

This is essential for correct `BETWEEN` joins. If end values overlap with the next block's start value, a fact record timestamped at exactly `"13:40"` would match two rows instead of one.

**Validation:** Confirm that no `FiveMinuteIntervalEnd` value equals any `FiveMinuteIntervalStart` value other than its own row. Report PASS or FAIL for this check.

---

## FACT TABLE JOIN PATTERN

The fact table (not built here) contains a `DateTime` field as `"YYYY-MM-DD HH:MM:SS"` or `"YYYY-MM-DD HH:MM"`. Join by extracting the date and time substrings and using `BETWEEN` for the time portion. Example in generic SQL:

```sql
SELECT
  f.*,
  d.Year, d.AcademicYear, d.FiscalYear,
  d.Quarter, d.CalendarWeek, d.AcademicWeek, d.FiscalWeek,
  t.Hour,
  t.ThirtyMinuteInterval, t.ThirtyMinuteIntervalStart, t.ThirtyMinuteIntervalEnd,
  t.FiveMinuteInterval,   t.FiveMinuteIntervalStart,   t.FiveMinuteIntervalEnd
FROM FactTable f
JOIN Calendar d
  ON SUBSTRING(f.DateTime, 1, 10) = d.Date
JOIN TimeSlots t
  ON SUBSTRING(f.DateTime, 12, 5)
     BETWEEN t.FiveMinuteIntervalStart
         AND t.FiveMinuteIntervalEnd
```

---
</code></pre>
<br />


### Taret-Specific Prompts
Copy the prmopt for your desired target, and paste at the end of the base prompt. <br />

#### **Target: Pure Generic SQL (recursive CTE)**
<pre><code>
## Target: Pure Generic SQL (recursive CTE)

Implement using only standard SQL with no procedural extensions. Generate both tables as CTEs or CREATE TABLE AS SELECT using recursive CTEs for date and time series generation.

- The Calendar CTE must generate one row per day for the full date range using recursive date arithmetic.
- The TimeSlots CTE must generate all 288 rows using recursive integer arithmetic.
- Do not use procedural loops, stored procedures, or any vendor-specific functions beyond basic string formatting and date arithmetic.
- All derived columns must be computed inline within the CTE.
- The final output should be two standalone `CREATE TABLE AS` or `WITH` + `SELECT` statements that can be run on any SQL engine supporting recursive CTEs (e.g., PostgreSQL, SQLite, DuckDB).
- Use `strftime` or equivalent portable string formatting for zero-padded time values.
- Note any assumptions made about the target SQL dialect.

---
</code></pre>


#### **Target: Pure JavaScript → AlaSQL (browser, array assignment)**
<pre><code>
## Target: Pure JavaScript → AlaSQL (browser, array assignment)

Implement in pure vanilla JavaScript (ES6+) for use with AlaSQL, intended to run in a browser (potentially embedded in a webpage).

**Environment constraints:**
- Zero external dependencies beyond AlaSQL itself.
- Do not use Node.js-specific APIs (`require()`, `fs`, `process`, etc.).

**Date range — use exactly this pattern:**
```js
const currentYear = new Date().getFullYear()
const startDate = `${currentYear - 10}-01-01`
const endDate   = `${currentYear + 5}-12-31`
```

**Performance — bulk assignment only:**
Do not insert rows one at a time using AlaSQL `INSERT` statements. Generate the full array in JavaScript first, then assign directly:
```js
alasql("CREATE TABLE Calendar")
alasql.tables.Calendar.data = generateCalendar(startDate, endDate)
alasql("CREATE TABLE TimeSlots")
alasql.tables.TimeSlots.data = generateTimeSlots()
```

**Function signatures:**
- `generateCalendar(startDate, endDate)` — accepts ISO date strings, returns a plain JS array of objects, one per calendar day.
- `generateTimeSlots()` — no parameters, returns a plain JS array of exactly 288 objects.

Minimize intermediate object creation and string operations inside loops wherever possible. Use UTC-based `Date` methods (`getUTCFullYear`, `getUTCMonth`, `getUTCDate`, etc.) throughout.

---

</code></pre>

#### **Target: Pure JavaScript (to JSON / array)**
<pre><code>
## Target: Pure JavaScript (to JSON / array)

Implement in pure vanilla JavaScript (ES6+), suitable for Node.js or a browser console.

- No external dependencies.
- `generateCalendar(startDate, endDate)` returns a plain JS array of objects.
- `generateTimeSlots()` returns a plain JS array of exactly 288 objects.
- After generating both arrays, serialize each to a JSON file (Node.js: use `fs.writeFileSync`) or log to console (browser).
- Output files: `Calendar.json` and `TimeSlots.json`.
- Print PASS/FAIL for the boundary validation.

---
</code></pre>


#### **Target: PowerShell (to CSV)**
<pre><code>
## Target: PowerShell (to CSV)

Implement in PowerShell. Generate both tables as arrays of `[PSCustomObject]` and export each to a CSV file:
- `Calendar.csv`
- `TimeSlots.csv`

- Use .NET `[DateTime]` for date arithmetic. Use UTC methods where needed to avoid timezone issues.
- Do not use any external modules.
- Use `Export-Csv -NoTypeInformation`.
- Print PASS/FAIL for the boundary validation to the console.

---
</code></pre>


#### **Target: Python (to CSV and pandas DataFrame)**
<pre><code>
## Target: Python (to CSV and pandas DataFrame)

Implement in Python 3. Use only the standard library plus `pandas`.

- Generate both tables as `pandas.DataFrame` objects.
- Export to CSV: `Calendar.csv` and `TimeSlots.csv` (no index column).
- Use Python's `datetime` module for all date arithmetic. Do not use `dateutil` or other third-party date libraries.
- The Calendar generation must not iterate row-by-row with append; build via list comprehension or vectorized operations where possible.
- Print PASS/FAIL for the boundary validation.

---
</code></pre>


#### **Target: R (to data frame)**
<pre><code>
## Target: R (to data frame)

Implement in R. Use only base R — no external packages required (though `data.table` or `tibble` may be used optionally if available).

- Generate both tables as base R `data.frame` objects: `calendar_df` and `timeslots_df`.
- Optionally write to CSV using `write.csv()`: `Calendar.csv` and `TimeSlots.csv`.
- Use `as.Date()` and `format()` for date arithmetic and string formatting.
- Avoid row-by-row `rbind()` in a loop; use vectorized operations (`seq.Date()`, `ifelse()`, etc.) wherever possible.
- Print PASS/FAIL for the boundary validation using `cat()` or `message()`.

---

</code></pre>


#### **Target: Power Query (M language)**
<pre><code>
## Target: Power Query (M language)

Implement in Power Query M, suitable for use in Power BI Desktop or Excel's Power Query Editor.

- Define both tables as separate queries named `Calendar` and `TimeSlots`.
- Use `Date.Year(DateTime.LocalNow())` to determine the current year dynamically at runtime. Do not hardcode any years.
- Generate the Calendar date series using `List.Dates(startDate, numberOfDays, #duration(1,0,0,0))` converted to a table via `Table.FromList`, then expand columns using `Table.AddColumn`. Do not generate rows one at a time in a recursive pattern.
- Generate the TimeSlots rows using `List.Generate` or `{0..287}` as the row index source, converted to a table, with all columns derived from the index via `Table.AddColumn`.
- Use M's native date and time functions (`Date.WeekOfYear`, `Date.Month`, `Date.Year`, `Time.Hours`, etc.) where available. Implement ISO 8601 week numbering manually if `Date.WeekOfYear` does not conform to ISO 8601 in the target host (note: Power BI's `Date.WeekOfYear` is not ISO 8601 — provide a correct manual implementation).
- All string formatting (zero-padding, `"YYYY-MM-DD"`, `"HH:MM"`, `"FYyyyy"`, `"YYYY-YY"`) must use `Text.PadStart` and `Text.From` — do not rely on locale-sensitive formatting.
- Each query should end with the table typed using `Table.TransformColumnTypes` so column types are explicit.
- Include the PASS/FAIL boundary validation as a separate diagnostic query named `TimeSlots_BoundaryCheck` that returns a one-row, one-column table containing `"PASS"` or `"FAIL"`.

---
</code></pre>


#### **Target: Bash shell (to CSV)**
<pre><code>
## Target: Bash shell (to CSV)

Implement as a Bash script (bash 4+). Write both tables to CSV files:
- `Calendar.csv`
- `TimeSlots.csv`

- Use only POSIX tools available in a standard Linux environment: `date`, `awk`, `printf`, `seq`, etc.
- The `date` command must use UTC (`TZ=UTC date`) to avoid timezone issues.
- Generate the header row first, then append data rows.
- The TimeSlots table may be generated with arithmetic only (no `date` command needed).
- Print PASS/FAIL for the boundary validation to stdout.
- Include a brief comment at the top of the script noting any assumed `date` command flavor (GNU vs BSD).
----

</code></pre>


#### **Target: Lua (in-memory table)**
<pre><code>
## Target: Lua (in-memory table)

Implement in pure Lua. Store both tables as Lua arrays of tables (i.e., `table[i]` is a row, each row is a key-value table of column names to values).

- No external libraries.
- Use Lua's `os.date` or manual date arithmetic for date calculations. Note which approach you use and why.
- Expose both tables as module-level variables: `Calendar` and `TimeSlots`.
- Print a summary row count for each table and the PASS/FAIL boundary validation result to stdout.

---
</code></pre>

#### **Target: Lua (to CSV and SQLite)**
<pre><code>
## Target: Lua (to CSV and SQLite)

Implement in Lua. Write both tables to:
1. Separate CSV files: `Calendar.csv` and `TimeSlots.csv`, with a header row.
2. A SQLite database file `time_dimensions.db` with two tables: `Calendar` and `TimeSlots`.

Use the `lsqlite3` binding for SQLite. Use plain Lua `io` for CSV output. Wrap all SQLite inserts in a single transaction per table for performance. Print PASS/FAIL for the boundary validation to stdout.

---
</code></pre>



Copyright © 2024-2026 The Regents of the University of Michigan
