# Open Refine and spreadsheets - recap

## Good practices

- Reproducible data
  - Keep track of changes you made to the data set
- Tidy data
  - Make data machine friendly
  - Each variable must have its own column
  - Each observation must have its own row
  - Do not combine multiple pieces of information in a single cell
- Use static, universal file formats
- Do not change the original data set

## Spreadsheets (key ideas)

### Things spreadsheets do well

- Data entry
  - Provide a GUI for data entry
- Data validation
  - Help ensure data entry is less error prone
- Data wrangling
  - Sort
  - Conditional formatting

### Spreadsheet pitfalls

- Reproducible data analysis
- Do not enforce good data habits
- Formatting
  - Dates (split them up)
  - Null values
  - Conveying information through formatting

## OpenRefine (key ideas)

### Things we can use OpenRefine for

- See an overview of a data set
- Resolve inconsistencies
- Help you split data into granular parts
- Match local data with other data sets
- Save a set of data cleaning steps for replay on multiple files

## OpenRefine features

- OpenRefine automatically keeps a log of every change you make
- OpenRefine does not allow you to modify your original file
  - You must export to a new file to save your work
  - OpenRefine keeps your original data pristine
- Any operation can be undone
- OpenRefine can repeat your steps for more than one data set
- OpenRefine provides a user-friendly interface for complex data work
  - Clustering algorithms
  - Mass edits based on a condition
  