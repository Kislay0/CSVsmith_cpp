# CSVsmith_cpp — README.md

> A lightweight C++17 CSV manipulation toolkit

![status-badge](https://img.shields.io/badge/status-active-brightgreen) ![lang-badge](https://img.shields.io/badge/C%2B%2B-17-blue)

## Overview

CSV-Smith is a small, practical C++ utility for loading CSV files, transforming data in-memory, and exporting results to a new CSV. It focuses on clarity and approachability while covering common tabular operations.

**Highlights**

* Load CSVs into an in-memory table
* Access/modify any cell value
* Add/insert/delete rows and columns
* Sort rows (numeric or lexicographic)
* Convert date formats (per cell or per column)
* Merge multiple CSV files
* Filter rows via user callbacks
* Save results back to CSV

## Getting Started

### Requirements

* C++17-compatible compiler (e.g., `g++`)
* `make`

### Build

```bash
make           # builds examples and library
make clean     # removes build artifacts
```

### Quick Start

```cpp
#include "csv_data_manipulator.hpp"
using namespace std;

int main(){
  CSVData data("input.csv");           // load file
  data.set_value(0, 1, "updated");     // edit a cell
  data.delete_row(2);                   // remove a row
  data.sort_by_col(0, CSVData::ACS);    // sort by column 0 (ascending)
  data.write_data("output.csv");       // save
}
```

### Command-line Examples (in `examples/`)

```bash
# Convert date column format
./example_1 -i file.csv -f 3

# Keep rows with custom condition
./example_2 -i file.csv

# Sort by a specific column
./example_3 -i file.csv -c 0

# Merge two CSV files
./example_4 -i file1.csv -a file2.csv

# Remove duplicate rows
./example_5
```

## Core API (glance)

```cpp
// constructors
CSVData();
CSVData(const std::string& file);
CSVData(const CSVData& other);

// info
bool is_modified() const;
bool is_unified() const;
size_t rows() const; size_t columns() const;

// cell/row/column ops
std::string get_value(size_t r, size_t c) const;
void set_value(size_t r, size_t c, const std::string& v);
void add_row(const std::vector<std::string>& row);
void add_row(const std::vector<std::string>& row, size_t pos);
void delete_row(size_t r);
void delete_col(size_t c);
void delete_item(size_t r, size_t c);

// file ops
void read_file(const std::string& file);
void write_data(const std::string& file) const;
void append_file(const std::string& file);

// date formatting
void convert_date_format(const std::string& from, const std::string& to, size_t col);
void convert_date_format(const std::string& from, const std::string& to, size_t row, size_t col);

// sorting
enum Order { ACS, DECS };
void sort_by_col(size_t col, Order order);
```

### Row Filtering via Callback

```cpp
bool remove_even_ids(int row, int col, const std::string& value) {
  if (col == 0) {
    int v = std::atoi(value.c_str());
    return (v > 0 && v % 2 == 0);
  }
  return false;
}

CSVData d("file.csv");
d.delete_row_if(remove_even_ids);
```

> Supported callback signatures include `(row, col, value)`, `(row, col, value, cbData)`, and `(row, row_vector, cbData)`.

## Notes

* All indices are zero-based.
* Sorting attempts numeric comparison first; falls back to lexicographic.
* Examples compile individually via `make`.

## Roadmap / Ideas

* Aggregations: sum/avg/min/max, group-by
* Richer CLI for bulk transforms
* Better error handling and logging

## Contributing

PRs and issues are welcome. Please keep the API simple and the examples approachable.

<!-- ## License

*Add a license file (e.g., MIT, Apache-2.0) to clarify usage.* -->
