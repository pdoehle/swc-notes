---
title: Excel
tags: presentation
slideOptions:
  theme: solarized
  transition: 'fade'
---
### Quality control
**Questions:**
- How can we carry out basic quality control and quality assurance in spreadsheets?

**Objectives:**
- Apply quality control techniques to identify errors in spreadsheets and limit incorrect data entry.

---

### Quality Assurance

![Data validation](https://datacarpentry.org/spreadsheet-ecology-lesson/fig/data_validation.png)

---

### Quality Control

- Don't forget to keep the original data pristine and create a `readme.txt` file
- Helpful resource: Cornell University's [guidance](https://data.research.cornell.edu/content/readme) on creating `readme.txt` files

---

### Sorting

- Invalid values often sort to the bottom or top of a column
- Sort the data on each column one at a time to root out poor data

---

### Conditional Formatting
- Use with caution
- Can be a good strategy for flagging outliers

---

### Exporting data
**Questions:**
- How can we export data from spreadsheets in a way that is useful for downstream applications?

**Objectives:**
- Store spreadsheet data in universal file formats.
- Export data from a spreadsheet to a `.csv` file.

---

### Storing data using native formats is problematic
- We **do not** recommend using `.xls`, `.xlsx` (Excel), or `.ods` (LibreOffice) file formats
- More and more journals and grant agencies are requiring researchers to deposit data in a data repository (most of them do not accept Excel formats)
- Storing data in universal, open, and static formats keeps data consistent and reduces the possibility of error

---

### Suggested formats

- Tab-delimited (`.tsv`) files
- Comma-delimited (`.csv`) files

---

### Advantages of open formats

- Most software recognizes these formats
- The encoding is simple, so they will continue to be interpretable into the future
- These formats work well with version-control software like [Git](http://swcarpentry.github.io/git-novice)