# Causemos Compliant Format

DOJO will convert model data into the causemos compliant format used by [Uncharted](https://www.uncharted.software/).

## Contents

Data registration is discussed in the [data registration document](https://github.com/jataware/dojo-docs/blob/feature/causemos/data-registration.md). This document is meant to further describe less common data registration scenarios.

1. [Multi-dates](#multi-dates)
2. [Non-standard calendars](#non-standard%20calendars)
3. [Reserved column names](#reserved%20column%20names)



## Multi-dates
In some instances a model may have date data that represents a range of dates for example:
|    Date   | Country  | Crop Index |
|-----------|----------|------------|
| 2015/2016 | Djibouti | 0.7        |
| 2016/2017 | Djibouti | 0.8        |
| 2017/2018 | Djibouti | 0.9        |

 where `2017/2018` represents start and end dates. Causemos format supports only a single date field with the column name `timestamp`, therefore a multi-date should be divided into separate columns. Using the above example, this would correspond to:
| Start Date | End Date  | Country  | Crop Index |
|------------|-----------|----------|------------|
|    2015    |    2016   | Djibouti | 0.7        |
|    2016    |    2017   | Djibouti | 0.8        |
|    2017    |    2018   | Djibouti | 0.9        |
where one date would be marked as the `primary_date = True` and another would become a `feature` column, as described in the [data registration document](https://github.com/jataware/dojo-docs/blob/feature/causemos/data-registration.md).


## Non-standard calendars

Causemos dates are standardized according to the [Gregorion calendar](https://en.wikipedia.org/wiki/Gregorian_calendar). An example of a non-standard calendar is the [Ethiopian calendar](https://en.wikipedia.org/wiki/Ethiopian_calendar). Dates in a non-standard calendar should be converted to Gregorion datetime.


## Reserved column names
The Causemos format reserves the following column names: `timestamp`, `country`, `admin1`, `admin2`, `admin3`, `lat`, `lng`, `feature`, and `value`. If data is submitted with these column names and not used to represent that entity, then the submitted column name will be appended with the suffix `_non_primary`.
