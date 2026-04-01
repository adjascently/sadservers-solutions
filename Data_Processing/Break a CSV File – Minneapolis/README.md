# Break a CSV File – Minneapolis with a Vengeance Scenario

## Problem Overview
Split the CSV file (`data.csv`) located in `/home/admin/` into **exactly 10 smaller CSV files**:

- `data-00.csv`
- `data-01.csv`
- ...
- `data-09.csv`

---

##  Requirements

- Each file must:
  - Contain the **same header** (first line of `data.csv`)
  - Be a **valid CSV file** (no broken rows)
  - Be approximately equal in size
  - Be **≤ 32KB**
- Files must be created in `/home/admin/`

---

## Solution Approach

- Extract the header from the original CSV
- Read all rows while preserving CSV structure
- Distribute rows across 10 files
- Ensure:
  - No row is split across files
  - Each file stays within the 32KB limit
- Write each chunk with the header included

---

##  Implementation

```bash
cd /home/admin

python3 - << 'EOF'
import csv
import os
import io

input_file = "data.csv"
max_size = 32 * 1024
num_files = 10

# Read CSV
with open(input_file, newline='') as f:
    reader = list(csv.reader(f))

header = reader[0]
rows = reader[1:]

# Initial split (roughly equal rows)
base = len(rows) // num_files
extra = len(rows) % num_files

chunks = []
start = 0
for i in range(num_files):
    size = base + (1 if i < extra else 0)
    chunks.append(rows[start:start + size])
    start += size

# Function to calculate CSV size
def get_size(header, chunk):
    buf = io.StringIO()
    writer = csv.writer(buf)
    writer.writerow(header)
    writer.writerows(chunk)
    return len(buf.getvalue().encode())

# Adjust chunks if size exceeds 32KB
changed = True
while changed:
    changed = False
    for i in range(num_files):
        while get_size(header, chunks[i]) > max_size and len(chunks[i]) > 1:
            row = chunks[i].pop()
            smallest = min(
                range(num_files),
                key=lambda j: get_size(header, chunks[j]) if j != i else float('inf')
            )
            chunks[smallest].append(row)
            changed = True

# Write output files
for i, chunk in enumerate(chunks):
    with open(f"data-{i:02d}.csv", "w", newline='') as f:
        writer = csv.writer(f)
        writer.writerow(header)
        writer.writerows(chunk)
EOF
````


---

## Verification
```bash
/home/admin/agent/check.sh
````
