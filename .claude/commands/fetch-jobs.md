Run the full job scraping pipeline for a new month. Execute these steps sequentially from `job-market/_internal/`:

1. Scrape listings: `uv run python scrapers/scrape_builtin_requests.py`
2. Deduplicate: `uv run python scrapers/clean_dedup.py all_jobs_{YYYYMMDD}.csv` (use the date from step 1 output)
3. Download HTML: `uv run python scrapers/download_all_html.py --csv all_jobs_{YYYYMMDD}_dedup.csv`
4. Extract to YAML: `uv run python scrapers/extract_from_html.py --all --csv all_jobs_{YYYYMMDD}_dedup.csv`
5. LLM enrichment: `uv run python extract_llm.py --all --csv all_jobs_{YYYYMMDD}_dedup.csv`

Replace `{YYYYMMDD}` with today's date throughout.

After each step, report the output (number of jobs scraped, deduplicated, downloaded, extracted, enriched). If a step fails, stop and report the error.

Working directory: `job-market/_internal/`
