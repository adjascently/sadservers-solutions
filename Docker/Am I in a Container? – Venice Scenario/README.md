# Am I in a Container? – Venice Scenario

##  Problem Overview
Determine whether the current environment is:

- A **container** (e.g., Docker), or  
- A **virtual machine**

There is no automated test — the goal is to **investigate the system and conclude correctly**.

---

##  Requirements

- Use Linux commands to inspect the environment  
- Identify indicators of:
  - containerized environments  
  - virtual machines  
- Provide a clear conclusion based on observations  

---

##  Solution Approach

- Check control groups (`cgroups`) for container-related entries  
- Look for Docker-specific files like `/.dockerenv`  
- Inspect process tree and PID 1  
- Check system information and virtualization hints  
- Analyze filesystem and kernel behavior  

---

##  Implementation

```bash
# Check control groups for container indicators
cat /proc/1/cgroup

# Check for Docker-specific environment file
ls -la /.dockerenv

# Check PID 1 process (container vs full OS)
ps -p 1 -o comm=

# Check system hostname
hostname

# Check for container-related mounts
cat /proc/self/mountinfo | grep -i docker
````


---

## Conclusion

- No container-related entries found in cgroup
- No /.dockerenv file present
- System behaves like a full Linux environment

👉 Therefore, the environment is a Virtual Machine (VM) and not a container
