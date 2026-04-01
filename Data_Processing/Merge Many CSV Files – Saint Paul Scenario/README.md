# Merge Many CSV Files – Saint Paul Scenario

## Problem Overview
Merge all **338 CSV files** located in `/home/admin/` with the pattern:

---

##  Requirements

- The final file must:
  - Contain data from **all 338 CSV files**
  - Include **only one header row**
  - Allow files to be merged in **any order**
- Each input file already contains a header, so duplicates must be removed

---

##  Solution Approach

- Identify one CSV file and extract its header
- Write that header once into `all.csv`
- Loop through all CSV files:
  - Skip the first line (header)
  - Append remaining data to `all.csv`

---

##  Implementation

```bash
cd /home/admin

# Get the first file to extract header
first=$(ls polldayregistrations_enregistjourduscrutin?????.csv | head -n 1)

# Write header to output file
head -n 1 "$first" > all.csv

# Append all data (skip headers)
for f in polldayregistrations_enregistjourduscrutin?????.csv; do
  tail -n +2 "$f" >> all.csv
done
````

---


## Verification
```bash
/home/admin/agent/check.sh
````

