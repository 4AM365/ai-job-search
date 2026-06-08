# Salary Benchmark Tool

## What is this?

The salary lookup tool (`salary_lookup.py`) lets you benchmark company salaries against a baseline from your own data. It's used during the `/apply` workflow to show how a company's compensation compares to market rates.

**This tool is optional.** If you don't have salary data, the salary step is simply skipped during `/apply`.

## How it works

The tool reads a `salary_data.json` file in the repo root containing company salary benchmarks. It uses fuzzy matching to find companies by name, handling US legal suffixes (Inc, LLC, Corp, Co, Ltd, LLP), accented characters, and common spelling variations.

The data format supports any index-based or absolute salary data. For example:
- Index 100 = median salary, higher is better
- Absolute salary values in USD (annual base, total comp, etc.)
- Any custom metric you want to track

## Data format

The tool expects `salary_data.json` with this structure:

```json
{
  "metadata": {
    "source": "My Union Statistics 2025",
    "index_baseline": 100,
    "index_label": "Index",
    "baseline_description": "Index 100 = median salary for private sector"
  },
  "companies": [
    {
      "company": "Acme Corp",
      "city": "Austin, TX",
      "categories": {
        "all_employees": { "count": 500, "index": 108.5 },
        "engineering": { "count": 120, "index": 112.3 }
      }
    },
    {
      "company": "Globex Inc",
      "city": "Seattle, WA",
      "categories": {
        "all_employees": { "count": 200, "index": 105.2 }
      }
    }
  ]
}
```

### Fields

- **metadata.source**: Where the data comes from (for reference)
- **metadata.index_baseline**: The baseline value (e.g., 100 for index-based data)
- **metadata.index_label**: Label for the index column in output
- **metadata.baseline_description**: Human-readable explanation of the baseline
- **companies[].company**: Company name (required)
- **companies[].city**: City/location (optional, used for filtering)
- **companies[].categories**: Named salary categories, each with `count` and/or `index`

## Setup options

### Option A: Create salary_data.json manually

Create the file by hand with data from any source: Glassdoor, Levels.fyi, Payscale, Salary.com, BLS OEWS wage data, H-1B LCA disclosure data (public DOL filings showing offered salaries by employer), salary surveys, networking, or personal research.

### Option B: Convert from Excel

If you have salary data in an Excel file:

```bash
pip install openpyxl
python tools/convert_salary_excel.py path/to/salary-data.xlsx \
  --source "Glassdoor export 2025" \
  --baseline 100 \
  --baseline-desc "Index 100 = median salary"
```

The converter auto-detects the Excel layout:
- Looks for a "Company" column and an optional "City"/"Location" column
- Treats remaining columns as salary data (auto-pairs count/index columns)

### Option C: Build from research

Start with an empty template and add companies as you research them:

```json
{
  "metadata": {
    "source": "Personal research",
    "index_baseline": 0,
    "index_label": "Annual salary (USD)",
    "baseline_description": "Approximate annual base salary"
  },
  "companies": [
    {
      "company": "Example Corp",
      "city": "Austin, TX",
      "categories": {
        "entry_level": { "index": 95000 },
        "senior": { "index": 150000 }
      }
    }
  ]
}
```

## Usage

```bash
python salary_lookup.py "Acme Corp"
python salary_lookup.py "Globex" --city "Seattle"
python salary_lookup.py "Initech" --json
python salary_lookup.py --list-all
```

## Important notes

- The data file (`salary_data.json`) is **excluded from git** (see `.gitignore`). Your salary data may be proprietary or confidential.
- If the data file is missing, `salary_lookup.py` exits with a helpful error message and the `/apply` workflow skips the salary benchmark step.
- The fuzzy matcher handles US company name variations: legal suffixes (Inc, LLC, Corp, Co, Ltd, LLP), accented characters, parentheticals, and partial matches.
