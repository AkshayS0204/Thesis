# Firm Network Hyperlink Analysis — Thesis Code & Data

This repository contains the code and processed data for my BSc thesis on firm
networks and domestic dependence in company towns, comparing Volkswagen
(Wolfsburg) and ASML (Veldhoven). Hyperlink data is collected from the anchor
companies' websites using the [ARGUS web scraper](https://github.com/JanKinne/ARGUS)
(Kinne, 2018), then cleaned, classified geographically, and analysed as a network.

## What the code does

`clean_argus_links.py` processes the raw output of an ARGUS website crawl and
produces a clean list of meaningful outgoing hyperlinks. Specifically, it:

1. **Reads** one or more ARGUS `dualspider` output files (`.jl` / JSON-lines format).
2. **Extracts** all outgoing hyperlinks from the crawled pages.
3. **Removes noise** — generic platforms that nearly every firm links to (social
   media, payment/e-commerce services, generic tech and web services). The full
   list is in the `NOISE_DOMAINS` set at the top of the script.
4. **Removes duplicates** so each domain is counted once.
5. **Classifies** each remaining link geographically (Local NL/DE, Domestic NL/DE,
   or International) based on its city name and top-level domain.
6. **Writes** the result to a CSV with two columns: `domain` and `geography`.

The cleaned CSV files for each company and crawl size (e.g.
`VW500_cleaned_links.csv`, `ASML100_cleaned_links.csv`) are included.

## Requirements

- **Python 3.7 or newer**
- **pandas** — install with:
  ```
  pip install pandas
  ```
- `json`, `argparse`, and `os` are part of the Python standard library (no install
  needed).

To produce the raw `.jl` input files in the first place, you also need ARGUS itself;
see its repository for installation and usage: https://github.com/JanKinne/ARGUS

## How to run

1. Run an ARGUS crawl on a company website to produce the `dualspider` `.jl`
   output file(s).
2. Run the cleaning script, pointing it at those files:

   ```
   python clean_argus_links.py --input file1.jl file2.jl --output cleaned_links.csv
   ```

   For example, to reproduce the Volkswagen 500-page crawl:

   ```
   python clean_argus_links.py \
       --input VW500file1.jl VW500file2.jl \
       --output VW500_cleaned_links.csv
   ```

3. The script prints a summary (total links kept and the geographic breakdown) and
   writes the CSV to the path given with `--output`.

Run `python clean_argus_links.py --help` to see all options.

## Repository contents

| File | Description |
|------|-------------|
| `clean_code.py` | Cleaning and geographic classification script (above). |
| `*_cleaned_links.csv` | Cleaned output for each company / crawl size. |


## Use of AI tools

AI assistance (Claude) was used to debug the ARGUS scraper 
