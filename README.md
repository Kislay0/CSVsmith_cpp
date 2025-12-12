
============================================================
                       CSV-Smith
        A lightweight C++ CSV manipulation toolkit
============================================================

CSV-Smith is a small and practical C++ utility designed for
loading CSV files, modifying the data in memory, applying
transformations, and exporting the results as a new CSV file.
It provides a simple API that supports row/column operations,
sorting, date-format conversion, merging CSV files, and
conditional filtering through callback functions.

This project is intended as a compact, easy-to-understand
example of C++17 programming focused on file I/O, STL usage,
and data manipulation.

------------------------------------------------------------
Features
------------------------------------------------------------
• Load CSV files into an in-memory table representation
• Access or modify any cell value
• Add, delete, and insert rows
• Delete columns or individual cells
• Sort rows by a selected column (numeric or lexicographic)
• Convert date formats across a column or a single cell
• Merge multiple CSV files into one dataset
• Remove rows using user-defined callback logic
• Save processed data into a new CSV file

------------------------------------------------------------
Build Instructions
------------------------------------------------------------
Requirements:
  - GNU g++ compiler
  - make

Build:
  make

Clean build artifacts:
  make clean

------------------------------------------------------------
Quick Example (conceptual)
------------------------------------------------------------
// Load file
CSVData data("input.csv");

// Edit a cell
data.set_value(0, 1, "updated");

// Remove a row
data.delete_row(2);

// Sort by column 0
data.sort_by_col(0, CSVData::ACS);

// Save result
data.write_data("output.csv");

------------------------------------------------------------
Core API Overview
------------------------------------------------------------
Constructors:
  CSVData();
  CSVData("file.csv");
  CSVData(const CSVData& other);

Information methods:
  data.is_modified();
  data.is_unified();
  data.rows();
  data.columns();

Cell/Row/Column operations:
  data.get_value(r, c);
  data.set_value(r, c, value);
  data.add_row(vector<string>);
  data.add_row(vector<string>, position);
  data.delete_row(r);
  data.delete_col(c);
  data.delete_item(r, c);

File operations:
  data.read_file("file.csv");
  data.write_data("out.csv");
  data.append_file("more.csv");

Date formatting:
  data.convert_date_format(old_fmt, new_fmt, col);
  data.convert_date_format(old_fmt, new_fmt, row, col);

Sorting:
  data.sort_by_col(col, CSVData::ACS);
  data.sort_by_col(col, CSVData::DECS);

------------------------------------------------------------
Filtering Rows with delete_row_if
------------------------------------------------------------
You can remove rows based on custom callback logic.
The callback receives row/col info and the cell value.

Example:
bool remove_even_ids(int row, int col, const string& value) {
    if (col == 0) {
        int v = atoi(value.c_str());
        return (v > 0 && v % 2 == 0);
    }
    return false;
}

CSVData d("file.csv");
d.delete_row_if(remove_even_ids);

Several callback signatures are supported:
  (row, col, value)
  (row, col, value, cbData)
  (row, row_vector, cbData)

------------------------------------------------------------
Examples (in the /examples directory)
------------------------------------------------------------
Example 1:
  Convert a date column to a new format.
  ./example_1 -i file.csv -f 3

Example 2:
  Keep rows with custom conditions.
  ./example_2 -i file.csv

Example 3:
  Sort data by a specific column.
  ./example_3 -i file.csv -c 0

Example 4:
  Merge two CSV files.
  ./example_4 -i file1.csv -a file2.csv

Example 5:
  Remove duplicate rows.
  ./example_5

------------------------------------------------------------
Notes
------------------------------------------------------------
• All indices are zero-based.
• Sorting attempts numeric comparison first.
• delete_row_if can operate per-cell or per-row.
• Examples must be compiled individually using make.

------------------------------------------------------------
About this Project
------------------------------------------------------------
CSV-Smith focuses on clarity and approachability.
It is suitable for:
  - learning file I/O in C++
  - exploring STL usage
  - understanding tabular data operations
  - building small command-line tools

You are free to extend it with:
  - more analytics (sum, average, group-by)
  - a richer command-line interface
  - error handling improvements
  - additional row/column operations

============================================================

