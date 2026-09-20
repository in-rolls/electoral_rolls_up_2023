## Uttar Pradesh Electoral Rolls 2023

[![Data: Dataverse](https://img.shields.io/badge/data-10.7910%2FDVN%2FOG47IV-blue)](https://doi.org/10.7910/DVN/OG47IV)
[![License: MIT](https://img.shields.io/badge/code-MIT-green)](LICENSE)

Every electoral-roll PDF the Chief Electoral Officer, Uttar Pradesh listed at [ceouttarpradesh.nic.in/rollpdf/rollpdf.aspx](https://ceouttarpradesh.nic.in/rollpdf/rollpdf.aspx) in February 2023: 161,203 parts (one PDF per polling-station part) across 403 assembly constituencies and 75 districts. This repository holds the scraper and the index of every part it found. The PDFs themselves are in the [Indian Electoral Roll PDF Corpus](https://github.com/in-rolls/electoral_rolls) on Harvard Dataverse at [doi:10.7910/DVN/OG47IV](https://doi.org/10.7910/DVN/OG47IV); because the rolls carry personal information, access is restricted to research use.

### Data

| File | Where | What |
|---|---|---|
| `data/up-2023-electoral-rolls.csv` | this repository | 161,203 rows, one per part: district, constituency, part number, polling station, and the portal link to its PDF |
| `up-electral-roll-001.7z` ... `up-electral-roll-323.7z` | Dataverse, restricted | the PDFs, 323 archives totalling 627 GiB, uploaded March 2024 |

The archive names carry a typo (`electral`) from upload; they are kept as published so existing references still resolve. The archives differ in size (the last is 9 MB), so each appears to be a standalone 7z archive, not one volume of a split archive; extract each with `7z x up-electral-roll-NNN.7z`. Inside, PDFs are named `<District>_<AC_Number>_<Part_Number>.pdf` (set in `02-download_pdf.py`), which joins back to the CSV on those three columns.

**Columns** of `data/up-2023-electoral-rolls.csv`:

| Column | Notes |
|---|---|
| `District` | District name as in the portal's dropdown, e.g. `Agra`. 75 values. |
| `AC_Name` | Assembly constituency name as in the dropdown, e.g. `Agra Cantt.`. 19% of values carry a trailing space; strip before matching. |
| `AC_Number` | Assembly constituency number, 1--403. |
| `Part_Number` | Part (polling-station roll) number within the constituency, 1--1,121. `(AC_Number, Part_Number)` is unique. |
| `Polling_Station_Name` | Polling station name in Hindi, e.g. `कलैक्ट्रेट कचहरी नई इमारत क.नं. 1`. |
| `ElectorRollPdf` | Portal URL of the CAPTCHA page that serves the PDF, e.g. `https://ceouttarpradesh.nic.in/rollpdf/PopupCaptcha.aspx?link=...`. Each link is gated by a CAPTCHA and may no longer resolve. |

Four constituencies have parts listed under two districts (AC 30: Moradabad/Sambhal; 58: Ghaziabad/Hapur; 184: Amethi/Sultanpur; 271: Ayodhya/Barabanki), so `AC_Number` does not nest cleanly inside `District`.

### How the data were collected

The scrape ran in two steps, in February--March 2023:

1. [`01-up-scrape-pdf-links.ipynb`](01-up-scrape-pdf-links.ipynb) walks the portal's ASP.NET form: for each district it posts back to list the constituencies, for each constituency it posts `Show`, then pages through the results grid (`Page$N` postbacks), recording one row per part. Duplicate rows are dropped and the result written to `data/up-2023-electoral-rolls.csv`.
2. [`02-download_pdf.py`](02-download_pdf.py) opens each part's CAPTCHA page, fetches the CAPTCHA image, solves it through a paid solving service (anti-captcha by default; a 2captcha path is also present), posts the answer, and saves the PDF. It retries up to five times per part and skips parts already on disk, so it can be re-run after a crash.

The scripts are kept as a record of how the data were produced. The portal has changed since, so they are not expected to run unmodified.

### Usage

```bash
pip install -r requirements.txt
# put your CAPTCHA-service key in TOKEN_ANTI_CAPTCHA in 02-download_pdf.py
python 02-download_pdf.py
```

### Getting the PDFs

Request access through the procedure described in [in-rolls/electoral_rolls](https://github.com/in-rolls/electoral_rolls#how-do-i-get-the-electoral-roll-pdfs) (a form plus IRB approval), then download from [Dataverse](https://doi.org/10.7910/DVN/OG47IV).

### Citation

See [CITATION.cff](CITATION.cff). Please cite the Dataverse DOI for the data.

### License

Code is MIT. The rolls are published by the Chief Electoral Officer, Uttar Pradesh; the Dataverse copy is restricted to research use.

## 🔗 Adjacent Repositories

- [in-rolls/electoral_rolls_bihar_2020](https://github.com/in-rolls/electoral_rolls_bihar_2020) — Bihar Electoral Rolls 2020
- [in-rolls/electoral_rolls](https://github.com/in-rolls/electoral_rolls) — PDFs of Indian Electoral Rolls
- [in-rolls/parse_unsearchable_rolls](https://github.com/in-rolls/parse_unsearchable_rolls) — Parse Unsearchable Electoral Rolls
- [in-rolls/parse_searchable_rolls](https://github.com/in-rolls/parse_searchable_rolls) — Parse Searchable Electoral Rolls
- [in-rolls/mnrega_social](https://github.com/in-rolls/mnrega_social) — MNREGA Social Audit Data

✨ _Powered by [Adjacent](https://github.com/gojiplus/adjacent)_ 🚀
