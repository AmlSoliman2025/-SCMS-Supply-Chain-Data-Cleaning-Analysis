
# 📓 Notebook Description: `SCMS cleaning.ipynb`

**Purpose:**  
This Jupyter Notebook performs comprehensive data cleaning and transformation on raw supply chain data related to health commodities (HIV, malaria, etc.) sourced from the SCMS program. The goal is to create an analysis-ready dataset suitable for use in Power BI dashboards, descriptive analytics, and vendor performance monitoring.

---

## 🔍 Key Operations Performed:

### 🧹 1. Data Preprocessing
- Loaded raw Excel dataset
- Converted date columns from text to `datetime` with coercion for invalid flags
- Standardized string formats (title case, whitespace stripping)

### 🛠 2. Handling Missing & Invalid Dates
- Flagged and excluded rows with invalid PQ/PO/scheduled dates
- Imputed missing `PO Sent to Vendor Date` using **median delay per vendor**
- Created `pq_to_po_days` to calculate vendor responsiveness

### 🚚 3. Freight Cost Cleaning
- Removed records with `"freight included"` in commodity cost
- Calculated `freight_cost_per_unit` and used it to estimate missing freight costs based on:
  - `product group`
  - `country`
  - `shipment mode`

### 🧾 4. Insurance Cost Fixes
- Applied logic based on `vendor inco term` to fill or nullify insurance fields
- Used median group values where necessary
  
### ⚖️ 5. Weight Imputation 
- Detected missing weight (kilograms) values in the dataset.
- Imputed missing weights using median weight per kg from similar records:
      Grouped by product group 

 ### 🧭 5. Shipment Mode Imputation
- Imputed missing shipment modes using the **most common value per vendor**, falling back to the global mode ("Air")

### 🔠 6. Standardization & Cleanup
- Standardized `vendor`, `country`, and `shipment mode` text values
- Resolved encoding issues (e.g., `"CÃ´te d'Ivoire"` → `"Côte d'Ivoire"`)

### 📊 7. Final Flags & Export
- Created `valid_timeline` column to indicate complete and clean rows
- Saved final outputs in `.csv` and `.xlsx` formats for use in BI tools

---

## 📤 Outputs from the Notebook
- `SC_Cleaned.xlsx`: Final cleaned Excel file
- `clean_data_for_powerbi.csv`: CSV file optimized for Power BI
- Export-ready, analysis-friendly dataset
