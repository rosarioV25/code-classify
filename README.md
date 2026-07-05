# CodeClassify — Free Barcode & Business Code Tools + API

**[code-classify.com](https://code-classify.com)** — 16 free, instant, in-browser tools to validate and convert the product & business codes you deal with every day, plus a deterministic pay-per-use JSON API. No signup. Same input, same output, every time — no AI guessing.

## 🧰 Free tools (run in your browser, no signup)

**Barcodes & products**
- [GTIN / UPC / EAN check-digit calculator](https://code-classify.com/gtin-check-digit/) — validate or compute the GS1 Mod-10 check digit for GTIN-8, UPC-A, EAN-13, GTIN-14
- [UPC ↔ EAN converter](https://code-classify.com/upc-to-ean/) — convert UPC-A ↔ EAN-13 and build GTIN-14 case codes
- [ISBN-10 ↔ ISBN-13 converter](https://code-classify.com/isbn-converter/)
- [Bulk barcode validator](https://code-classify.com/bulk-barcode-validator/) — check a whole list at once
- [Barcode generator](https://code-classify.com/barcode-generator/) — EAN-13 / UPC-A / Code 128 as SVG
- [SSCC-18 calculator + GS1-128 label](https://code-classify.com/sscc-check-digit-calculator/)

**Finance & banking**
- [IBAN checker](https://code-classify.com/iban-checker/) — MOD-97 + per-country length rules
- [EU VAT number validator](https://code-classify.com/vat-number-validator/) — all 27 member states
- [ISIN validator](https://code-classify.com/isin-validator/) — ISO 6166
- [Luhn checker (cards & IMEI)](https://code-classify.com/luhn-checker/)
- [ABA routing number validator](https://code-classify.com/routing-number-validator/)

**Trade & logistics**
- [VIN validator & decoder](https://code-classify.com/vin-validator/) — ISO 3779 check digit + free NHTSA decode
- [ISO 6346 container number validator](https://code-classify.com/container-number-validator/)
- [NAICS & SIC code lookup](https://code-classify.com/naics-lookup/)
- [SIC ↔ NAICS crosswalk](https://code-classify.com/sic-to-naics/)
- [HS code lookup](https://code-classify.com/hs-code-lookup/)

Browse the full databases: [NAICS directory](https://code-classify.com/naics/) · [SIC directory](https://code-classify.com/sic/)

## ⚡ API

The same checks, programmatically — see [code-classify.com/api](https://code-classify.com/api/) (also on [RapidAPI](https://rapidapi.com/rosariovitale0096/api/barcode-business-code-toolkit)). Batch up to 100 items per call. Free tier, then pay-per-use.

Example:

    curl -X POST https://codeclassify-api.rosariovitale0096.workers.dev/v1/gtin/validate \
      -H "Content-Type: application/json" \
      -d '{"codes":["036000291452","4006381333931"]}'

16 endpoints: GTIN validate / check-digit / convert, ISBN convert, IBAN, EU VAT, Luhn, ISIN, SSCC, container, ABA routing, VIN, NAICS / SIC / HS search, and the SIC↔NAICS crosswalk. Full docs at [code-classify.com/api](https://code-classify.com/api/).

## Why deterministic?

Results are computed from official public standards (GS1 Mod-10, ISO 13616, ISO 6166, ISO 3779, U.S. Census NAICS/SIC, U.S. HTS) — auditable and repeatable, unlike AI classifiers that can invent codes that do not exist. More: [methodology](https://code-classify.com/methodology/) · [data sources](https://code-classify.com/data-sources/).
