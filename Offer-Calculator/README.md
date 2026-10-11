# Asmita Jyoti CHSL – Redevelopment Offer Calculator

Upload this folder to GitHub Pages (or any web server). The page reads the CSVs in `data/` on every load. To update the page, edit a CSV and commit it. You don't need to change any code.

| File | What it holds |
|---|---|
| `data/flats.csv` | One row per flat/shop/unit: building, flat_no, unit_type (Residential/Commercial), existing carpet area. **Fill `existing_carpet_area_sqft` for shops/units here.** |
| `data/developers.csv` | Developer names, addresses, scheme |
| `data/offers_area.csv` | Offers that scale with your area. Each row is dev_id + unit_type + item + value + basis. Items: `additional_area_pct`, `rent` (one row per year, or year 1 + `rent_escalation_pct`), `brokerage`, `hardship`, `refundable_deposit`, `buyback`, `corpus` (fill `value` in ₹/sq ft when known). |
| `data/offers_general.csv` | Flat-amount and descriptive offers, per member or per society (relocation, parking, BG, lien, amenities…). `amount` is numeric when it is used in calculations. |
| `data/settings.csv` | `rent_months` (default rent period) and the statement date |

To add a developer, add a row in `developers.csv` and matching rows in the offer files under the same `dev_id`.

Opening `index.html` by double-click won't work, because browsers block local CSV reads. Use a web server or run `python -m http.server` in this folder for testing.
