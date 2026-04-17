# Control One Container from Another – Quito Scenario

##  Problem Overview
There are two Docker containers on the system:

- `docker-access` – currently running
- `nginx` – currently stopped

The goal is to **start the `nginx` container from inside the `docker-access` container**.

You must **not** start `nginx` from the host system or from any container other than `docker-access`.

---

##  Requirements

- `nginx` must be started **from inside** `docker-access`
- `docker-access` may be restarted if needed
- The following command must succeed:

```bash
sudo docker exec docker-access docker ps
````


The provided validation script must pass:
```bash
/home/admin/agent/check.sh
```

---

## Solution Approach
- Recreate the `docker-access` container with access to the Docker daemon
- Mount the Docker socket into `docker-access`
- Keep the container running with a long-lived command
- Enter the docker-access container
- Start the `nginx`  container from inside it
- Verify from the host that Docker commands can be executed inside `docker-access`

  ---
## Implementation 
```bash
sudo docker rm -f docker-access

sudo docker run -d \
  --name docker-access \
  -v /var/run/docker.sock:/var/run/docker.sock \
  docker-access \
  sleep infinity

sudo docker exec -it docker-access sh
docker start nginx
docker ps
exit

sudo docker exec docker-access docker ps
/home/admin/agent/check.sh
```

---

## Verification
```bash
sudo docker exec docker-access docker ps
/home/admin/agent/check.sh
```

Expected result:

OK
