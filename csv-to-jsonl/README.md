# csv-to-jsonl

Demonstrates a minimal ELT pipeline: extract from a CSV file using `tap-csv`, load to JSONL using `target-jsonl`, then display the result using a self-described inline utility plugin.

## What it demonstrates

- Running a full ELT pipeline with `meltano run`
- Configuring `tap-csv` to read a local CSV file and declare a primary key
- Using `target-jsonl` to write Singer records as newline-delimited JSON
- Defining an inline utility plugin using a system binary (`sh`) — no Hub lookup or pip install required
- Combining Hub-installed plugins and self-described plugins in a single project

## Prerequisites

- [Meltano installed](https://docs.meltano.com/getting-started/installation)
- `sh` available on your system PATH (standard on all Unix-like systems)

## How to run

```bash
cd csv-to-jsonl

# Install tap-csv and target-jsonl from the Meltano Hub
meltano install

# Run the pipeline: CSV → Singer stream → JSONL file
meltano run csv-to-jsonl

# Display the JSONL output using the self-described inline plugin
meltano invoke show-output:print-jsonl
```

## Expected output

After `meltano run csv-to-jsonl`, a file is written to `output/employees.jsonl`.

Running `meltano invoke show-output:print-jsonl` prints that file to stdout:

```json
{"id": "1", "name": "Alice Johnson", "department": "Engineering", "salary": "95000", "start_date": "2019-03-12"}
{"id": "2", "name": "Bob Smith", "department": "Marketing", "salary": "72000", "start_date": "2020-07-01"}
{"id": "3", "name": "Carol White", "department": "Engineering", "salary": "88000", "start_date": "2018-11-05"}
{"id": "4", "name": "David Brown", "department": "HR", "salary": "65000", "start_date": "2021-01-15"}
{"id": "5", "name": "Eve Davis", "department": "Marketing", "salary": "78000", "start_date": "2022-04-22"}
```

> Note: `tap-csv` reads all columns as strings by default, so numeric values like `salary` will appear quoted.

## Project structure

```
csv-to-jsonl/
  meltano.yml          # Meltano project: tap, loader, utility, and job
  data/
    employees.csv      # Mock input data
  output/              # Created at runtime; gitignored
    employees.jsonl    # Written by target-jsonl
```

## Key config notes

### tap-csv — mapping a CSV file to a Singer stream

```yaml
extractors:
- name: tap-csv
  variant: meltanolabs
  config:
    files:
    - entity: employees       # Becomes the Singer stream name (and output filename)
      path: data/employees.csv
      keys:
      - id                    # Primary key — required by the Singer spec
```

Each entry in `files` maps one CSV to one stream. The `entity` value becomes both the stream name in the Singer protocol and the base name of the output JSONL file (`output/employees.jsonl`).

### target-jsonl — writing JSONL output

```yaml
loaders:
- name: target-jsonl
  variant: andyh1203
  config:
    destination_path: output  # Directory where JSONL files are written
```

`target-jsonl` writes one file per stream, named `<stream>.jsonl`, into `destination_path`.

### show-output — self-described inline utility

```yaml
utilities:
- name: show-output
  namespace: show_output
  executable: sh              # System binary — no pip install needed
  commands:
    print-jsonl:
      args: -c "cat output/*.jsonl"
      description: Prints all JSONL output files to stdout
```

This plugin uses the same self-described pattern as the [hello-world](../hello-world/) example. The `executable` points to any binary on your PATH; `commands` attaches named argument sets. Here `sh -c "cat output/*.jsonl"` reads all JSONL files produced by the pipeline and writes them to stdout.

### The job

```yaml
jobs:
- name: csv-to-jsonl
  tasks: tap-csv target-jsonl
```

`meltano run csv-to-jsonl` is shorthand for `meltano run tap-csv target-jsonl`. Jobs are useful for naming and reusing pipeline sequences.
