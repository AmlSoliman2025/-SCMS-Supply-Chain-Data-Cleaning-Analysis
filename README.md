
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
 ### 🔹 4. ** Weight Imputation**
- Detected missing weight (kilograms) values in the dataset.
- Imputed missing weights:
    - Use the median weight per unit within each product group
  

### 🔹 5. **Insurance Logic Fix**
- Where Incoterms (e.g., EXW, FCA) legally exclude insurance: filled with `0`.
- Where insurance was required but missing: filled using group-level median.

### 🔹 6. **Shipment Mode Imputation**
- Filled missing shipment modes using:
  - Vendor-level most common mode
  - Overall most common mode (fallback)

### 🔹 7. **Vendor & Country Standardization**
- Converted all names to title case for consistency.
- Fixed encoding issues like `"CÃ´te d'Ivoire"` → `"Côte d'Ivoire"`.
-  vendor names (e.g., `"Acouns Nigeria Ltd"` → `"Accoun Nigeria Limited"`) 
    Not the Same Vendor: The name discrepancies, coupled with inconsistent pricing and product descriptions, suggest these are separate entries.





## 📁 Output Files

- The original file Supply_Chain_Shipment_Pricing from (( https://www.kaggle.com/datasets/divyeshardeshana/supply-chain-shipment-pricing-data)).
- `clean_data_for_powerbi.csv` → Power BI-ready CSV version
- `SCMS cleaning.ipynb` → Full Jupyter Notebook with code and explanations

---

## 📊 Key Fields for Analysis

| Column                     | Description                                         |
|----------------------------|-----------------------------------------------------|
| `project code`             | Shipment grouping by donor or project               |
| `country`                  | For regional and country-level analysis             |
| `vendor`                   | Central to performance and cost comparisons         |
| `shipment mode`            | Impacts timing and cost                             |
| `pq_to_po_days`            | Vendor response time to quotation                   |
| `po_to_delivery_days`      | Vendor fulfillment speed (to be derived)            |
| `freight cost (usd)`       | Direct logistics cost                               |
| `freight_cost_per_unit`    | Normalized freight efficiency                       |
| `line item value`          | Total order value                                   |
| `weight (kilograms)`       | For freight cost per kg analysis                    |
| `line item insurance (usd)`| Risk and cost-sharing under Incoterms               |
| `vendor inco term`         | Defines cost responsibility                         |
| `valid_timeline`           | Filters reliable time records                       |
| `delivery recorded date`   | Actual inventory confirmation                       |

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

## 💡 Next Steps


1. **Descriptive & Diagnostic Analysis**
   - Explore delivery patterns, delays, and cost drivers

2. **KPI Development**
   - Build metrics like % on-time, average delay, freight per kg

3. **Power BI Visualization**
   - Create dashboards with filters by country, vendor, shipment mode

4. **Reporting**
   - Deliver actionable insights through Power Point presenation.
  
  ## 📬 Contact

For questions or collaboration, reach out to [Aml Soliman] at [amlsoliman50@gmail.com].
