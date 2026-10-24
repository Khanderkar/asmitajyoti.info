# Asmita Jyoti CHSL – Redevelopment Offer Calculator

Upload this folder to the `Offer-Calculator` virtual directory (the name is case-sensitive). The page reads the CSVs in `data/` on every load. To update the page, edit a CSV. You don't need to change any code.

| File | What it holds |
|---|---|
| `data/flats.csv` | One row per flat, shop or unit – flat data only, no personal data: `sr_no, building, flat_no, unit_code, unit_type, location_tag, existing_carpet_area_sqft, monthly_maintenance, monthly_property_tax, language`. `unit_type` = Residential / Shop / Unit. For shops, `location_tag` = FRONT SIDE / MARVE ROAD FACING / SCHOOL FACING / BACKSIDE FACING / FIRST FLOOR (decides Ramky's shop rent and brokerage). `language` = the language shown automatically for that flat (en, hi, mr, gu, bn, pa, ur, or, ta, te, kn, ml). |
| `data/developers.csv` | Developer names and addresses. Row order = column order on the page. |
| `data/offers_area.csv` | Offers that depend on the unit, as dev_id + unit_type + item + value + basis. A blank `applies_to` is the default. If `applies_to` holds a building (e.g. `E1`) or a shop location tag, that row overrides the default for that unit. |
| `data/offers_general.csv` | Text offers grouped by `category`: Per member, Commercial (shown only for shops and units), Society, and Tender compliance (in `item`, text after ` | ` shows as the tender requirement). |
| `data/developer_profile.csv` | Developer background shown at the end of the page: `section` = Profile / Financials. Use ` \| ` inside a value to start a new line. |
| `data/sources.csv` | Numbered references (`ref_id`, title, URL, note). Pages show them as #1, #2…, listed with full URLs at the end. In `developer_profile.csv` the `ref_ids` column links each value to these (e.g. `5;4`). |
| `data/i18n.csv` | All on-screen text in 12 languages (one row per text `key`; columns `en hi mr gu bn pa ur or ta te kn ml`). Edit a cell to change a translation. A blank cell falls back to English. Save as **CSV UTF-8**. |
| `data/settings.csv` | `rent_months`, `statement_date`, `offers_last_updated`, plus section switches `show_*` (1 = show, 0 = hide): `show_other_offers`, `show_commercial_terms`, `show_society_offers`, `show_tender_compliance`, `show_developer_profile`, `show_closing_disclaimer`, `show_references`, `show_print_pdf_buttons`. The "Your offer comparison" table always shows. Switches also apply to Print / Export to PDF. |

**Language:** the page opens in English. When a flat is selected, it switches to that flat's language (the `language` column of `flats.csv`) and shows a note. Visitors can pick any language from the 🌐 menu at the top; after a manual choice, the page stops switching automatically. Offer notes and other text taken from the offer CSVs stay in English. Codes: en English, hi Hindi, mr Marathi, gu Gujarati, bn Bengali, pa Punjabi, ur Urdu, or Odia, ta Tamil, te Telugu, kn Kannada, ml Malayalam.

**Benefit in words:** below the comparison table, the page shows the lowest and highest total cash benefit across the developers (and, for rented flats, the net gain range), with the amount in words in the selected language. No developer is named there.

**Items in `offers_area.csv`:** `additional_area_pct`, `new_area_fixed`, `rent` (one row per quoted year), `rent_escalation_pct` / `rent_escalation_abs` (used after the last quoted year), `brokerage`, `relocation`, `hardship` (the corpus / hardship compensation), `refundable_deposit`, `free_parking`, `buyback`.

**Basis values:**
- `per_sqft_onetime`: value × existing area.
- `lumpsum`: a fixed ₹ amount.
- `months_rent`: value × that year's monthly rent. It is counted only if that year falls within the rent period.
- `months_rent_per_N_months`: one block of months' rent for every N months of the rent period.

To add a developer, add a row in `developers.csv` and matching rows in the offer files under the same `dev_id`.

Opening `index.html` by double-click won't work, because browsers block reading local CSV files. Use the web server.

`lib/html2pdf.bundle.min.js` powers the **Export to PDF** button and is loaded from this folder, so no internet CDN is needed. If it fails to load, the button falls back to the browser's Print → Save as PDF.

## Encrypted member columns in `data/flats.csv`

`flats.csv` is the single master file. The first 10 columns are plain flat data used by the calculator. The last 5 columns hold personal data and are **encrypted cell by cell**, so the file can sit in a public GitHub repository:

| Column | Content (after decryption) |
|---|---|
| `member_name` | Owner name(s) |
| `monthly_rent` | Rent the owner receives – blank = flat not rented |
| `permanent_address` | Owner's permanent / mailing address (rented flats) |
| `mobile` | Up to two numbers, separated by ` / ` |
| `email` | Up to two addresses, separated by `; ` |

Each cell is `ENC1:` + base64( 12-byte IV ‖ AES-256-GCM ciphertext ‖ 16-byte tag ). Every value is padded with spaces to a fixed length per column before encryption, so all cells in a column look alike (nobody can tell which flats are rented or have an email). The calculator ignores these columns. The key is kept by the Managing Committee outside this repository – never commit the key or a decrypted copy.
