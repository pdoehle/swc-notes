# Databases Using SQL

## Setup (If not Already Done)
* Download and install [Firefox](https://www.mozilla.org/en-US/firefox/new/).
* Install the [SQLite Manager](https://addons.mozilla.org/en-us/firefox/addon/sqlite-manager/) Add-on for Firefox.
* Download the [dataset](https://ndownloader.figshare.com/articles/1314459/versions/6) and associated files and unzip it in a convenient location.

## Motivation
Previously, we used Excel and OpenRefine to organize our data into computer-readable data. Now we will move to the next part of the data workflow, which involves reading in our data and using it for analysis and visualization.

## Dataset
The data we will be using is a time-series for a small mammal community in southern Arizona. This is part of a project studying the effects of rodents and ants on the plant community that has been running for almost 40 years. The rodents are sampled on a series of 24 plots, with different experimental manipulations controlling which rodents are allowed to access which plots.

This is a real dataset that has been used in over 100 publications. It's been simplified just a little bit for the workshop, but you can download the [full dataset](http://esapubs.org/archive/ecol/E090/118/) and work with it using exactly the same tools we’ll learn about today.

## Why Use a Database
* Databases are similar to spreadsheets, but have key differences.
* Spreadsheet
  * Directly edit cells
  * Use formulas based on other cells
* Databases
  * Send commands (called *queries*) to a database manager (we will use sqlite3).
  * The database manager manipulates data for us
  * *manager does lookups, calculations, and queries*
  * Returns results in a tabular form
* So why go through the trouble of using a database?
  * Keep data separate from analysis.
    * Easy to reproduce results.
    * No risk of accidentally changing data while running analysis.
  * Fast (even for large amounts of data)
  * Improves quality control of data entry.
