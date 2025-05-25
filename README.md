## UP 2023 Electoral Rolls

We scrape electoral rolls from https://ceouttarpradesh.nic.in/rollpdf/rollpdf.aspx

We split scraping into two steps:

1. [harvesting all the links](01-up-scrape-pdf-links.ipynb) --- CSV with links to the pdf is [here](data/)
2. [downloading the pdfs](02-download_pdf.py)

The PDFs are posted to https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/OG47IV

## 🔗 Adjacent Repositories

- [in-rolls/mnrega_social](https://github.com/in-rolls/mnrega_social) — MNREGA Social Audit Data
- [in-rolls/quota](https://github.com/in-rolls/quota) — Effects of Randomly Assigned Reservations for Women Leaders in Indian Local Government on Allocation and Development Outcomes
- [in-rolls/local_elections_kerala](https://github.com/in-rolls/local_elections_kerala) — Kerala Local Government Seat Reservation Data and Winner Attributes
- [in-rolls/local_elections_up](https://github.com/in-rolls/local_elections_up) — UP Local Election Data --- GP and ULB. Seat reservation, winner, and candidates for some elections
- [in-rolls/local_elections_bihar](https://github.com/in-rolls/local_elections_bihar) — Candidate Info. + Valid Votes Won by Cands. in the 2016 Bihar Panchayat Elections
