# Asmita Jyoti CHSL – Redevelopment Offer Calculator

Upload this folder to the `Offer-Calculator` virtual directory (the name is case-sensitive). The page reads the CSVs in `data/` on every load. To update the page, edit a CSV. You don't need to change any code.

| File | What it holds |
|---|---|
| `data/flats.csv` | One row per flat, shop or unit. `unit_type` = Residential / Shop / Unit. For shops, `location_tag` = Front / Marve Road / School-facing / Back (it decides Ramky's shop rent and brokerage). **Fill `existing_carpet_area_sqft` for shops and units.** |
| `data/developers.csv` | Developer names and addresses. Row order = column order on the page. |
| `data/offers_area.csv` | Offers that depend on the unit, as dev_id + unit_type + item + value + basis. A blank `applies_to` is the default. If `applies_to` holds a building (e.g. `E1`) or a shop location tag, that row overrides the default for that unit. |
| `data/offers_general.csv` | Text offers grouped by `category`: Per member, Commercial (shown only for shops and units), and Society. |
| `data/settings.csv` | `rent_months`, `statement_date`, `offers_last_updated` |

**Items in `offers_area.csv`:** `additional_area_pct`, `new_area_fixed`, `rent` (one row per quoted year), `rent_escalation_pct` / `rent_escalation_abs` (used after the last quoted year), `brokerage`, `relocation`, `hardship` (the corpus / hardship compensation), `refundable_deposit`, `free_parking`, `buyback`.

**Basis values:**
- `per_sqft_onetime`: value × existing area.
- `lumpsum`: a fixed ₹ amount.
- `months_rent`: value × that year's monthly rent. It is counted only if that year falls within the rent period.
- `months_rent_per_N_months`: one block of months' rent for every N months of the rent period.

To add a developer, add a row in `developers.csv` and matching rows in the offer files under the same `dev_id`.

Opening `index.html` by double-click won't work, because browsers block reading local CSV files. Use the web server.
