# Date

# weekofyear 跨年的问题

1. spark

   spark.sql.functions.weekofyear 遵循 ISO 8601

2. pandas

   pd.Timestamp('2019-10-06').isocalendar(), 遵循 ISO 8601

3. Python 内置 date 库
   date.isocalendar()

## ISO 8601

> Week date representations are in the formats as shown in the adjacent box. [YYYY] indicates the ISO week-numbering year which is slightly different from the traditional Gregorian calendar year (see below). [Www] is the week number prefixed by the letter W, from W01 through W53. [D] is the weekday number, from 1 through 7, beginning with Monday and ending with Sunday.
>
> There are several mutually equivalent and compatible descriptions of week 01:
>
> the week with the year's first Thursday in it (the formal ISO definition),
> the week with 4 January in it,
> the first week with the majority (four or more) of its days in the starting year, and
> the week starting with the Monday in the period 29 December – 4 January.
> As a consequence, if 1 January is on a Monday, Tuesday, Wednesday or Thursday, it is in week 01. If 1 January is on a Friday, Saturday or Sunday, it is in week 52 or 53 of the previous year (there is no week 00). 28 December is always in the last week of its year.
>
> The week number can be described by counting the Thursdays: week 12 contains the 12th Thursday of the year.
>
> The ISO week-numbering year starts at the first day (Monday) of week 01 and ends at the Sunday before the new ISO year (hence without overlap or gap). It consists of 52 or 53 full weeks. The first ISO week of a year may have up to three days that are actually in the Gregorian calendar year that is ending; if three, they are Monday, Tuesday and Wednesday. Similarly, the last ISO week of a year may have up to three days that are actually in the Gregorian calendar year that is starting; if three, they are Friday, Saturday, and Sunday. The Thursday of each ISO week is always in the Gregorian calendar year denoted by the ISO week-numbering year.
>
> Examples:
>
> Monday 29 December 2008 is written "2009-W01-1"
> Sunday 3 January 2010 is written "2009-W53-7"

参考：
https://en.wikipedia.org/wiki/ISO_8601
