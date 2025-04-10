
# 📦 SCMS Supply Chain Data Cleaning & Enrichment

This project focuses on preparing supply chain data related to health commodities (e.g., HIV test kits, antimalarials) for analysis. The dataset originates from the SCMS program and includes vendor info, shipment timelines, freight, and insurance costs.

---

## 🎯 Objective

To clean, transform, and enrich raw SCMS supply chain data into a reliable format for timeline analysis, cost evaluation, and performance tracking — with the end goal of using the data in Power BI dashboards.

---

## ✅ Cleaning & Transformation Steps

### 🔹 1. **Date Parsing & Flag Handling**
- Converted textual date columns (e.g. `pq first sent to client date`) to `datetime`.
- Replaced non-date flags (e.g., `"Pre-PQ Process"`, `"From RDC"`) with `NaT`.
- Created boolean flags `pq_flag`, `po_flag` for quality tracking.

### 🔹 2. **Timeline Reconstruction**
- Calculated `pq_to_po_days` = difference between PQ and PO dates.
- Imputed missing `po sent to vendor date` using **vendor-level median delay** from PQ.
- Created a `valid_timeline` flag to mark rows with clean and usable timeline data.

### 🔹 3. **Freight Cost Cleanup**
- Excluded rows where freight was **included in commodity cost**.
- Created a new variable `freight_cost_per_unit`.
- Imputed missing freight cost using median unit cost by:
  - `product group`
  - `country`
  - `shipment mode`

### 🔹 4. **Insurance Logic Fix**
- Where Incoterms (e.g., EXW, FCA) legally exclude insurance: filled with `0`.
- Where insurance was required but missing: filled using group-level median.

### 🔹 5. **Shipment Mode Imputation**
- Filled missing shipment modes using:
  - Vendor-level most common mode
  - Overall most common mode (fallback)

### 🔹 6. **Vendor & Country Standardization**
- Converted all names to title case for consistency.
- Fixed encoding issues like `"CÃ´te d'Ivoire"` → `"Côte d'Ivoire"`.
- Mapped duplicate vendor names (e.g., `"Acouns Nigeria Ltd"` → `"Accoun Nigeria Limited"`).



## 📁 Output Files

- The original file Supply_Chain_Shipment_Pricing from 
- `clean_data_for_powerbi.csv` → Power BI-ready CSV version
- `SCMS cleaning.ipynb` → Full Jupyter Notebook with code and explanations

---

## 📊 Key Fields for Analysis

| Column | Description |
|--------|-------------|
| `pq_to_po_days` | Days between PQ sent and PO sent |
| `freight cost (usd)` | Imputed or original freight cost |
| `freight_included_flag` | Boolean flag for freight inclusion |
| `valid_timeline` | Boolean flag for timeline reliability |
| `shipment mode` | Mode of delivery (e.g., Air, Sea) |

---

## 💡 Notes & Recommendations

- Use `valid_timeline == True` in any time-based analysis to avoid distorted metrics.
- Cleaned data is suitable for KPI dashboards in Power BI.
- Vendor consolidation is recommended for grouped performance reporting.

---

## 🧰 Tools Used

- Python (pandas, numpy, openpyxl)
- Jupyter Notebook
- Power BI (target platform)


