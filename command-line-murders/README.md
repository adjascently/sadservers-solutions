# 🕵️ The Command Line Murders (SadServers)

## Scenario Overview

A murder has occurred in Terminal City. Using only command-line tools, the goal is to investigate clues across multiple files and identify the killer.

Once identified, the murderer’s full name must be written to:

```bash
/home/admin/mysolution
````
---
## Approach

The investigation follows a structured pipeline:

1.Analyze crime scene clues
2.Identify the correct witness
3.Extract suspect information from interviews
4.Cross-reference vehicle data
5.Filter candidates using membership records
6.Narrow down using physical attributes

---

##  Step 1: Analyze Crime Scene

```bash
grep -i "CLUE" mystery/crimescene
````
Key Findings:

Suspect is a tall male (≥ 6')
Wallet contained memberships:
Rotary Club
Delta SkyMiles
Terminal City Library
Museum of Bash History
Witness: Annabel (female)

---
## Step 2: Locate their addresses:
```bash
sed -n '40p' mystery/streets/Hart_Place
sed -n '179p' mystery/streets/Buckingham_Place
````
These point to interview IDs.

---


## Step 3: Read Interviews
```bash
cat mystery/interviews/interview-47246024
cat mystery/interviews/interview-699607
```
Key Witness Insight (Annabel Church)
Saw getaway vehicle:
Blue Honda
Plate starts with L337
Plate ends with 9

---

## Step 4: Identify Vehicle Owner
```bash
grep -B 5 -A 5 -E "License Plate L337.*9" mystery/vehicles
```
Result
Owner: Jacqui Maher

Note: Not the killer (female), but likely connected to the suspect.

---
## Step 5: Membership Filtering
We need suspects who appear in ALL four:
```bash
Rotary_Club
Delta_SkyMiles
Terminal_City_Library
Museum_of_Bash_History
comm -12 \
  <(comm -12 \
      <(sort mystery/memberships/Rotary_Club) \
      <(sort mystery/memberships/Delta_SkyMiles)) \
  <(comm -12 \
      <(sort mystery/memberships/Terminal_City_Library) \
      <(sort mystery/memberships/Museum_of_Bash_History))
````
This produces a shortlist of candidates.

---

## Step 6: Filter by Physical Traits

Check candidate details:
```bash
grep -A5 -B2 "NAME" mystery/vehicles
````

Apply Filters
Male
Height ≥ 6'
Matches vehicle clue (Blue Honda)

 Final Result

Only one candidate satisfies all constraints:
 Joe Germuska
---


## Solution
echo "Joe Germuska" > ~/mysolution
md5sum ~/mysolution
/home/admin/agent/check.sh

---
## Key Learnings
Use grep for targeted clue extraction
Use sed for precise line-based retrieval
Use comm + sort for dataset intersection
Combine multiple datasets (people, vehicles, memberships)
Always validate assumptions step-by-step

---

## Tools Used
grep – pattern searching
sed – line extraction
comm – set intersection
sort – data normalization
cat – file inspection

---

