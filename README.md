# Coffee Roaster Website Scraper

Scrapes coffee products from Shopify, WooCommerce, Squarespace, and WordPress storefronts, then upserts results into Airtable.

This public package contains the **generic scraping engine** only. Roaster-specific tuning rules are intentionally excluded.

## Security first

- Do **not** commit real credentials.
- Copy `.env.example` to `.env` for local runs.
- For GitHub Actions, store secrets in **Settings → Secrets and variables → Actions**.
- If an Airtable token was ever committed or shared, **rotate it immediately** in Airtable.

## Quick start (local)

1. Install dependencies:

```bash
python3 -m pip install -r requirements.txt
```

2. Create your env file:

```bash
cp .env.example .env
```

3. Fill in `.env` with your Airtable token/base and table field names.

4. Run:

```bash
python3 Coffee_Roaster_Website_Scrapper
```

## Required Airtable setup

Your Airtable base should include:

- A **roasters table** with at least:
  - roaster name field
  - website URL field
  - web scraping enabled field (`Yes` values are scraped)
  - optional platform override field (`Shopify`, `Woocommerce`, `Squarespace`)
- A **products table** for scraped output rows

Set the exact table/field names in `.env` to match your base schema.

## GitHub Actions automation

1. Push this repository to GitHub.
2. Add repository secrets:
   - `AIRTABLE_TOKEN`
   - `AIRTABLE_BASE_ID`
3. (Optional) Add repository variables for table/field names and run flags.
4. Run manually from **Actions → Roaster Scrape → Run workflow**, or wait for schedule.

## Dry run mode

Set in `.env`:

```env
DRY_RUN=true
```

This logs discovery/filter behavior without writing to Airtable.

Optional roaster subset during dry run:

```env
ROASTER_BASE_URL_SUBSTRING_FILTER=example.com
```

## Notes

- `REPLACE_ROASTER_PRODUCTS_EACH_RUN=true` replaces each roaster's existing rows before re-adding fresh results.
- This public version does not include private roaster-specific inclusion/exclusion overrides.
