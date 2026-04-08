# Docker Web Container Can't Connect to DB Container 

##  Problem Overview
Two Docker containers are running:

- a **WordPress** web application container
- a **MariaDB** database container

However, the web application cannot connect to the database. Running:

```bash
curl -s localhost:80 | tail -4
````
The web app shows:

Error establishing a database connection




---

##  Requirements
- Both containers must remain running
- WordPress must connect to DB using hostname `mysql`
- The following command must work:

```bash
sudo docker exec wordpress mysqladmin -h mysql -u root -ppassword ping
````
---

## Implementation
```bash
# Create a user-defined network
sudo docker network create wp-net

# Connect wordpress container
sudo docker network connect wp-net wordpress

# Connect mariadb container with alias "mysql"
sudo docker network connect --alias mysql wp-net mariadb
````

---
## Verification
```bash
sudo docker exec wordpress mysqladmin -h mysql -u root -ppassword ping
````

---
