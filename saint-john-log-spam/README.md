# Saint John – Log File Growth Issue

##  Scenario
A developer created a testing program that continuously writes to a log file located at:

/var/log/bad.log

Over time, this causes the file to grow indefinitely, consuming disk space and potentially impacting system stability.

The program is no longer needed and must be stopped.

---

##  Objective
- Identify the process writing to `/var/log/bad.log`
- Terminate the process safely
- Ensure the log file stops growing
- Do **not** delete the log file

---

## Investigation

To identify which process is writing to the log file:

```bash
sudo lsof /var/log/bad.log
````
Output:

badlog.py 596 admin ...
This indicates that a Python script (badlog.py) with PID 596 is writing to the file.

To verify process details:
```bash
ps -fp 596
````
---

## Solution
Terminate the identified process:
```bash
sudo kill 596
````
---

## Verification

Ensure the process is no longer running:
```bash
ps aux | grep badlog
````

Check that the log file is no longer growing:
```bash
watch -n 1 'ls -lh /var/log/bad.log'
````
The file size should remain constant.


---

## Takeaway
This scenario highlights a common production issue where uncontrolled logging can exhaust disk space.
Being able to trace file activity to a specific process and resolve it quickly is essential for maintaining system reliability.
