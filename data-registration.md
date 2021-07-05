## Data Registration

### Contents

1. [Getting started](#getting-started)
2. [Choosing your file](#choosing-your-file)
3. [Geo and time inference](#geo-and-time-inference)
4. [Annotating your dataset](#annotating-your-dataset)
    - [Date formatting](#date-formatting)
    - [Build a date](#build-a-date)
    - [Coordinate pairs](#coordinate-pairs)
    - [Qualifiers](#qualifiers)

### Getting started

The data registration workflow currently support registering 4 data types: 

* CSV
* Excel
* NetCDF
* GeoTIFF

You may begin by navigating to [data.dojo-test.com](data.dojo-test.com). From there you will be asked for basic metadata about your dataset.

> Please provide as much information as possible throughout the data registration process to ensure that sufficient information is available to end-users of your dataset.

### Choosing your file

When you initially upload a file you may be presented with a set of options depending upon the detected file type. 

Excel files require that you select a worksheet. If your file is large, please wait until you see the detected worksheet names and select the appropriate one. If you proceed without selecting a worksheet name you will not be able to transform the dataset correctly.

If your dataset is a GeoTIFF, you will be asked to provide the data band you wish to process and the name of the feature that resides in that band. You may optionally provide a date for the respective band.

### Geo and time inference

Once you have uploaded your dataset, Dojo analyzes it to determine whether your dataset contains place or time information such as `timestamps`, `latitude`, `longitude`, `ISO` country codes, etc. This analysis process may take a few seconds, but it will ultimately speed up your data annotation.

### Annotating your dataset

Next, you will be shown a sample of 100 rows of your dataset. Columns highlighted in <span style="color:blue">**blue**</span> represent those which had a detected time or location feature.

Click the **Annotate** button at the top of each column to annotate it. 

> Note: you should only annotate columns that you wish to retain in the final, transformed dataset.

Once you've annotated a column it will be highlighted in <span style="color:green">**green**</span>.

![Pre-Annotation](imgs/pre-annotate.png)

You will be asked for a `display name` and `description` for your dataset. Additionally you will be asked whether this column is either `Date`, `Geo`, or a `Feature`. 

In the case of `Date` and `Geo` columns, they may be set to `primary`. It is important to choose only one column to be the primary `Date` and one to be the primary `Geo`. In the case of a [build a date](#build-a-date) or [coordinate pairs](#coordinate-pairs) all relevant columns will be associated as `primary` if the user sets that "grouping" to be primary.

#### Date formatting 

In the below example, the user annotates the "Year" column. 

![Pre-Annotation](imgs/year.png)

Note how the sample table at the left of the page is highlighted <span style="color:green">**green**</span>? That is because we have automatically detected a valid date format of `%Y` for this column. Date formats are defined using the [strftime](https://strftime.org/) reference. Please refer to it for questions about how to correct or update the date format for a column. Generally, our column analysis process can correctly assign a date format, but periodically the user must update or correct this with an appropriate formatter. For example `2020-02-01` would have the date format `%Y-%m-%d` but `Februrary 1, 2020` would be `%B %-d, %Y`.

If the date formatter is incorrect the column preview will turn <span style="color:red">**red**</span> until the user has corrected it.

#### Build a date

Some datasets have year, month and day split out into separate columns. In this case, the user may "build a date" by annotating any of the relevant fields and indicating that it is `part of a multi-column datetime object`. 

![Build a date](imgs/build-a-date.png)

The user can then select the relevant year, month and day columns as well as ensure they have correct date formatters.

#### Coordinate pairs

Generally speaking, if a dataset has latitude and longitude in it we should annotate this and ignore the other geospatial information (unless they are [qualifiers](#qualifiers)) as this is the most granular location information available and can be used to geocode the remainder of the dataset.

However, latitude and longitude are not typically contained in the same column. So, we provide a mechanism for the user to associate a `latitude` with a `longitude` and vice versa. To do this, you indicate that the column `is part of a coordinate pair` and choose it's partner from the dropdown.

![Coordinate pair](imgs/coordinate-pair.png)

#### Qualifiers

Many datasets contain features that _qualify_ other features. For example, in a conflict/event dataset such as ACLED, you may have a category for the type of event. The primary feature associated with the event may be number of fatalities, while the category "qualifies" the number of fatalities.

![Qualifiers](imgs/qualifiers.png)

To set `Event Type` as a _qualifier_ for `fatalities` the user should check the box indicating that `this field qualifies another`. The user should then select the relevant columns that the current feature qualifies.

> Note: you should only _qualify_ other features, not `Geo` or `Date` information since those are inherently dataset qualifiers. This avoids "qualifying a qualifier."

### Transforming the dataset

When you have completed annotating your dataset you should have at least one feature annotated as well as a primary geography and date. If no primary `Date` or `Geo` information was provided, we do our best to identify what _might_ have been `primary` based on the user's annotations.

We then transform the dataset into a ready-to-use format. This process may take some time, depending on what is required. If the dataset is quite large and requires reverse geocoding latitude and longitudes into admin 0 through 3 (using GADM) it could take up to a few minutes.

After the dataset has been transformed a preview will be shown in the ready-to-use format. If the dataset is large, a random sample of 100 rows is taken to allow the user to spot check accuracy. All `features` are "stacked" on top of each other. Qualifiers are added as additional columns to the right.

![Preview](imgs/preview.png)

If everything looks good the user can download this table if they wish. To save their work, the user **must `Submit to Dojo`**. Upon success the user can register another dataset or view the final metadata in Dojo.
