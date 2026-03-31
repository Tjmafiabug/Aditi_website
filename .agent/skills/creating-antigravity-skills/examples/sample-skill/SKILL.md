---
name: generating-csv-reports
description: Generates CSV summary reports from raw data files. Use when the user asks for a CSV export, data report, or wants to summarize a dataset into a file.
---

# Generating CSV Reports

## When to Use This Skill
- User asks for a CSV export or data summary file
- User wants to aggregate tabular data into a downloadable format

## Checklist
- [ ] Identify the input data source (file path or inline data)
- [ ] Confirm output columns and filename with user if ambiguous
- [ ] Run the script to generate the CSV
- [ ] Verify the output with a quick `head` check

## Workflow

**Plan**: Identify columns, source data, and output path.  
**Validate**: Dry-run or preview first 5 rows before writing.  
**Execute**: Write the file and confirm its location.

## Instructions

Use the helper script for generation:

```bash
bash scripts/generate_csv.sh <input_file> <output_file>
# Run --help if unsure of arguments:
bash scripts/generate_csv.sh --help
```

- Always validate output with `head -5 <output_file>` after generation
- If the input is JSON, the script handles conversion automatically
- Output path defaults to `./output/report.csv` if not specified

## Resources
- [scripts/generate_csv.sh](scripts/generate_csv.sh)
