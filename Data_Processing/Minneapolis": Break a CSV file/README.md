# CSV File Split – Minneapolis Scenario

##  Problem Overview
Split a large CSV file (`data.csv`) located in `/home/admin/` into **exactly 10 smaller files**:
- `data-00.csv`
- `data-01.csv`
- ...
- `data-09.csv`

##  Requirements
- Each file must:
  - Contain the **same header** (first line of `data.csv`)
  - Be approximately equal in size
  - Be **≤ 32KB**
- Splitting does **not** need to preserve proper CSV formatting (breaking lines is allowed)

---

## Solution Approach

- Extract the header from the original CSV
- Remove the header from the file content
- Split the remaining data into 10 equal parts
- Prepend the header to each split file
- Save the files in `/home/admin/` with correct naming

---

## Implementation

```bash
cd /home/admin

# Extract header
header=$(head -n 1 data.csv)

# Remove header (skip first line)
tail -c +$(($(printf "%s" "$header" | wc -c) + 2)) data.csv > /tmp/data_body.bin

# Split into 10 equal parts
split -n 10 -d -a 2 /tmp/data_body.bin /tmp/data-

# Add header to each split file
for f in /tmp/data-*; do
  out="/home/admin/$(basename "$f").csv"
  {
    printf '%s\n' "$header"
    cat "$f"
  } > "$out"
done
````
----

## Verification
Run the provided test script:
```bash
/home/admin/agent/check.sh
````

---
