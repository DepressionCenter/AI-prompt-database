![Depression Center Logo](https://github.com/DepressionCenter/.github/blob/main/images/EFDCLogo_375w.png "depressioncenter.org")


# **AI Prompts Database**
#### *__Real-world examples of UMGPT, Maizey and other AI prompts from the University of Michigan.__*

<br />

## PowerQuery Calendar Table

### Contributors:
+ [Gabriel Mongefranco](https://gabriel.mongefranco.com) ([@gabrielmongefranco](https://github.com/gabrielmongefranco)), Eisenberg Family Depression Center, University of Michigan

### Description
Use this prompt to generate M-Query code for a calendar or time table. This is useful in Power Query, Excel, Power BI, etc. for creating a time dimension in dashboards. For example, it could be used to create 5-minute blocks to standardize CGM or temperature readings to the same time scale, or 30-second blocks for sleep and HRV. Another example is creating fiscal year, calendar year, quarters, and week numbers to facilitate reporting.

### Template
**For: GPT**  <br />
**Prompt:**
<pre><code>
Generate a PowerQuery formula to give me a calendar table with the following columns: year, academic year (sept 1 thru aug 31, formatted as yyyy-yyyy), fiscal year (jul 1 thru jun 30, formatted as yyyy-yyyy), quarter, calendar week #, academic week #, fiscal week #, date (without time), hour (0-23), 5-minute interval # (resets at midnight, starts with zero), 5-minute interval start datetime, 5-minute interval end datetime, 1-minute interval # (resets at midnight, starts with zero), 1-minute interval start datetime, 1-minute interval end datetime, 30-second interval # (resets at midnight, starts with zero), 30-second interval start datetime, 30-second interval end datetime. There should also be a unique sequential numeric index for each row.
The calendar should span <var>[START_DATE]</var> at midnight thru <var>[END_DATE]</var> at 12:59:59PM.
</code></pre>
<br />


### Examples

Generate a date calendar with fiscal and academic years fro 2020-2030:<br />
<kbd>
Generate a PowerQuery formula to give me a calendar table with the following columns: year, academic year (sept 1 thru aug 31, formatted as yyyy-yyyy), fiscal year (jul 1 thru jun 30, formatted as yyyy-yyyy), quarter, calendar week #, academic week #, fiscal week #, date (without time), StartDateTime (same as date but at midnight), EndDateTime (same as date but at 12:59:59 PM). There should also be a unique sequential numeric index for each row.
The calendar should span <var>1/1/2020</var> at midnight thru <var>12/31/2030</var> at 12:59:59PM.
</kbd><br /><br />
<samp>
let
    // 1. Set boundaries
    StartDate = #date(2020, 1, 1),
    EndDate = #date(2030, 12, 31),
    NumberOfDays = Duration.Days(EndDate - StartDate) + 1,
    
    // 2. Build list of dates
    DateList = List.Dates(StartDate, NumberOfDays, #duration(1,0,0,0)),
    
    // 3. Turn into table with sequential index
    TableWithIndex = Table.FromList(DateList, Splitter.SplitByNothing(), {"Date"}),
    AddIndex = Table.AddIndexColumn(TableWithIndex, "Index", 0, 1, Int64.Type),

    // 4. Add StartDateTime & EndDateTime columns
    AddStartDT = Table.AddColumn(AddIndex, "StartDateTime", each DateTime.From([Date])),
    AddEndDT = Table.AddColumn(AddStartDT, "EndDateTime", each DateTime.From([Date]) + #duration(0,12,59,59)),

    // 5. Add Year
    AddYear = Table.AddColumn(AddEndDT, "Year", each Date.Year([Date])),
    
    // 6. Academic Year (Sept 1–Aug 31, yyyy-yyyy)
    AddAcademicYear = Table.AddColumn(AddYear, "AcademicYear", each 
        let
            y = Date.Year([Date]),
            m = Date.Month([Date])
        in 
            if m >= 9 then Text.From(y) & "-" & Text.From(y + 1)
            else Text.From(y - 1) & "-" & Text.From(y)
    ),

    // 7. Fiscal Year (Jul 1–Jun 30, yyyy-yyyy)
    AddFiscalYear = Table.AddColumn(AddAcademicYear, "FiscalYear", each 
        let
            y = Date.Year([Date]),
            m = Date.Month([Date])
        in 
            if m >= 7 then Text.From(y) & "-" & Text.From(y + 1)
            else Text.From(y - 1) & "-" & Text.From(y)
    ),

    // 8. Quarter (calendar quarter)
    AddQuarter = Table.AddColumn(AddFiscalYear, "Quarter", each Date.QuarterOfYear([Date])),
    
    // 9. Calendar Week # (ISO, Monday start)
    AddCalendarWeek = Table.AddColumn(AddQuarter, "CalendarWeek", each Date.WeekOfYear([Date], Day.Monday)),
    
    // 10. Academic Week # (since prior Sept 1, Monday start)
    AddAcademicWeek = Table.AddColumn(AddCalendarWeek, "AcademicWeek", each 
        let
            baseYear = if Date.Month([Date]) >= 9 then Date.Year([Date]) else Date.Year([Date]) - 1,
            academicStart = #date(baseYear, 9, 1),
            weekNum = Number.RoundDown(Duration.Days([Date] - academicStart) / 7) + 1
        in weekNum
    ),
    
    // 11. Fiscal Week # (since prior July 1, Monday start)
    AddFiscalWeek = Table.AddColumn(AddAcademicWeek, "FiscalWeek", each 
        let
            baseYear = if Date.Month([Date]) >= 7 then Date.Year([Date]) else Date.Year([Date]) - 1,
            fiscalStart = #date(baseYear, 7, 1),
            weekNum = Number.RoundDown(Duration.Days([Date] - fiscalStart) / 7) + 1
        in weekNum
    ),

    // 12. Select and reorder columns
    FinalTable = Table.SelectColumns(AddFiscalWeek, {
        "Index", "Year", "AcademicYear", "FiscalYear", "Quarter", "CalendarWeek",
        "AcademicWeek", "FiscalWeek", "Date", "StartDateTime", "EndDateTime"
    })
in
    FinalTable
</samp><br /><br />


<br />


Generate a full calendar table with 5-minute and 30-second intervals for 2024-2026:<br />
<kbd>
Generate a PowerQuery formula to give me a calendar table with the following columns: year, academic year (sept 1 thru aug 31, formatted as yyyy-yyyy), fiscal year (jul 1 thru jun 30, formatted as yyyy-yyyy), quarter, calendar week #, academic week #, fiscal week #, date (without time), hour (0-23), 5-minute interval # (resets at midnight, starts with zero), 5-minute interval start datetime, 5-minute interval end datetime, 1-minute interval # (resets at midnight, starts with zero), 1-minute interval start datetime, 1-minute interval end datetime, 30-second interval # (resets at midnight, starts with zero), 30-second interval start datetime, 30-second interval end datetime. There should also be a unique sequential numeric index for each row.
The calendar should span <var>1/1/2024</var> at midnight thru <var>12/31/2026</var> at 12:59:59PM.
</kbd><br /><br />
<samp>
let
    // 1. Set Start and End DateTime
    StartDateTime = #datetime(2024, 1, 1, 0, 0, 0),
    EndDateTime = #datetime(2026, 12, 31, 12, 59, 59),
    
    // 2. Calculate total number of 30-second intervals
    TotalSeconds = Duration.TotalSeconds(EndDateTime - StartDateTime),
    IntervalCount = Number.RoundDown(TotalSeconds / 30) + 1,
    
    // 3. Generate list of interval indexes
    IntervalIndexes = List.Numbers(0, IntervalCount),
    
    // 4. Create table from intervals
    CalendarTable = Table.FromList(IntervalIndexes, Splitter.SplitByNothing(), {"Index"}),
    
    // 5. Add start and end datetimes for each interval
    AddDateTimes = Table.AddColumn(CalendarTable, "IntervalStart", each StartDateTime + #duration(0,0,0,[Index] * 30)),
    AddEndDateTimes = Table.AddColumn(AddDateTimes, "IntervalEnd", each [IntervalStart] + #duration(0,0,0,30) - #duration(0,0,0,1)), // ends at xx:xx:29
    
    // 6. Extract Date, Hour, and other parts
    AddDate = Table.AddColumn(AddEndDateTimes, "Date", each Date.From([IntervalStart])),
    AddYear = Table.AddColumn(AddDate, "Year", each Date.Year([Date])),
    AddMonth = Table.AddColumn(AddYear, "Month", each Date.Month([Date])),
    AddDay = Table.AddColumn(AddMonth, "Day", each Date.Day([Date])),
    AddHour = Table.AddColumn(AddDay, "Hour", each Time.Hour(Time.From([IntervalStart]))),
    AddMinute = Table.AddColumn(AddHour, "Minute", each Time.Minute(Time.From([IntervalStart]))),
    AddSecond = Table.AddColumn(AddMinute, "Second", each Time.Second(Time.From([IntervalStart]))),
    
    // 7. Academic year (starts Sep 1, ends Aug 31)
    AddAcademicYear = Table.AddColumn(AddSecond, "AcademicYear", each 
        let
            year = Date.Year([Date]),
            month = Date.Month([Date])
        in
            if month >= 9 then Text.From(year) & "-" & Text.From(year + 1)
            else Text.From(year - 1) & "-" & Text.From(year)
    ),
    // 8. Fiscal year (starts Jul 1, ends Jun 30)
    AddFiscalYear = Table.AddColumn(AddAcademicYear, "FiscalYear", each 
        let
            year = Date.Year([Date]),
            month = Date.Month([Date])
        in
            if month >= 7 then Text.From(year) & "-" & Text.From(year + 1)
            else Text.From(year - 1) & "-" & Text.From(year)
    ),
    
    // 9. Quarter (standard calendar quarter)
    AddQuarter = Table.AddColumn(AddFiscalYear, "Quarter", each Date.QuarterOfYear([Date])),
    
    // 10. Calendar week (ISO 8601 week)
    AddWeek = Table.AddColumn(AddQuarter, "CalendarWeek", each Date.WeekOfYear([Date], Day.Monday)),
    
    // 11. Academic week (week from Sep 1, Monday is week start)
    AddAcademicWeek = Table.AddColumn(AddWeek, "AcademicWeek", each 
        let
            baseYear = if Date.Month([Date]) >= 9 then Date.Year([Date]) else Date.Year([Date]) - 1,
            academicYearStart = #date(baseYear, 9, 1),
            weekNo = Number.RoundDown(Duration.Days([Date] - academicYearStart) / 7) + 1
        in weekNo
    ),
    
    // 12. Fiscal week (week from Jul 1, Monday is week start)
    AddFiscalWeek = Table.AddColumn(AddAcademicWeek, "FiscalWeek", each 
        let
            baseYear = if Date.Month([Date]) >= 7 then Date.Year([Date]) else Date.Year([Date]) - 1,
            fiscalYearStart = #date(baseYear, 7, 1),
            weekNo = Number.RoundDown(Duration.Days([Date] - fiscalYearStart) / 7) + 1
        in weekNo
    ),
    
    // 13. 5-minute interval # (since midnight)
    AddFiveMinuteIntervalNum = Table.AddColumn(AddFiscalWeek, "FiveMinuteInterval#", each 
        let
            minsSinceMidnight = [Hour] * 60 + [Minute]
        in Number.IntegerDivide(minsSinceMidnight, 5)
    ),
    AddFiveMinuteIntervalStart = Table.AddColumn(AddFiveMinuteIntervalNum, "FiveMinuteIntervalStart", each 
        let
            d = [Date],
            intervalNum = [FiveMinuteInterval#]
        in DateTime.From(d) + #duration(0,0, intervalNum * 5,0)
    ),
    AddFiveMinuteIntervalEnd = Table.AddColumn(AddFiveMinuteIntervalStart, "FiveMinuteIntervalEnd", each 
        [FiveMinuteIntervalStart] + #duration(0,0,5,0) - #duration(0,0,0,1)
    ),
    
    // 14. 1-minute interval # (since midnight)
    AddOneMinuteIntervalNum = Table.AddColumn(AddFiveMinuteIntervalEnd, "OneMinuteInterval#", each 
        [Hour] * 60 + [Minute]
    ),
    AddOneMinuteIntervalStart = Table.AddColumn(AddOneMinuteIntervalNum, "OneMinuteIntervalStart", each 
        DateTime.From([Date]) + #duration(0,0,[OneMinuteInterval#],0)
    ),
    AddOneMinuteIntervalEnd = Table.AddColumn(AddOneMinuteIntervalStart, "OneMinuteIntervalEnd", each 
        [OneMinuteIntervalStart] + #duration(0,0,1,0) - #duration(0,0,0,1)
    ),
    
    // 15. 30-second interval # (since midnight)
    AddThirtySecondIntervalNum = Table.AddColumn(AddOneMinuteIntervalEnd, "ThirtySecondInterval#", each 
        ([Hour] * 3600 + [Minute] * 60 + [Second]) / 30
    ),
    AddThirtySecondIntervalStart = Table.AddColumn(AddThirtySecondIntervalNum, "ThirtySecondIntervalStart", each 
        DateTime.From([Date]) + #duration(0,0,0, Number.RoundDown([ThirtySecondInterval#]) * 30)
    ),
    AddThirtySecondIntervalEnd = Table.AddColumn(AddThirtySecondIntervalStart, "ThirtySecondIntervalEnd", each 
        [ThirtySecondIntervalStart] + #duration(0,0,0,30) - #duration(0,0,0,1)
    ),
    
    // 16. Remove any unnecessary columns
    FinalTable = Table.SelectColumns(AddThirtySecondIntervalEnd, 
        {
            "Index", "Year", "AcademicYear", "FiscalYear", "Quarter", "CalendarWeek", "AcademicWeek", "FiscalWeek",
            "Date", "Hour",
            "FiveMinuteInterval#", "FiveMinuteIntervalStart", "FiveMinuteIntervalEnd",
            "OneMinuteInterval#", "OneMinuteIntervalStart", "OneMinuteIntervalEnd",
            "ThirtySecondInterval#", "ThirtySecondIntervalStart", "ThirtySecondIntervalEnd"
        }
    )
in
    FinalTable
</samp><br /><br />


----

Copyright © 2024 The Regents of the University of Michigan
