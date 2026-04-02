# Docker Container Won't Start – Salta Scenario

##  Problem Overview
Run the Dockerized Node.js web application located in `/home/admin/app` so that it is accessible on:

- `localhost:8888`

and returns:

- `Hello World!`

There must be **exactly one running Docker container** for the solution to be valid.

----


##  Requirements
- The application must run inside a Docker container
- The app must be accessible on port `8888`
- The following command must work:

```bash
curl localhost:8888
````

----

##  Solution Approach

- Inspect the Dockerfile and application files  
- Fix the incorrect startup file in the Dockerfile  
- Identify the correct port the Node.js app listens on  
- Free up port `8888` because it was already being used by `nginx`  
- Build the Docker image  
- Run the container with the correct port mapping  

---

## Implementation

```bash
cd /home/admin/app

# Fix incorrect startup file in Dockerfile
sed -i 's/serve\.js/server.js/' Dockerfile

# Stop nginx because it was already using port 8888
sudo systemctl stop nginx

# Remove any existing container with the same name
sudo docker rm -f salta-web 2>/dev/null || true

# Build the Docker image
sudo docker build -t salta-app .

# Run the container with correct port mapping
sudo docker run -d --name salta-web -p 8888:8888 salta-app
````

---
## Verification
```bash
curl localhost:8888
sudo docker ps
````

---
