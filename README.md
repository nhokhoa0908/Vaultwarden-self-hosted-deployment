# Vaultwarden Self-Hosting Deployment
Deploying a Self-Hosting Enterprise Password Manager using Vaultwarden, Docker, and Nginx Proxy Manager

# Initializing the Password Management System (Vaultwarden & Nginx Proxy Manager)
This project provides detailed instructions on how to self-host a Vaultwarden password vault, using Nginx Proxy Manager as a reverse proxy to manage access flow and configure SSL/TLS certificate-only.

---

Creating the project directory and configuration file:
``` bash
mkdir ~/vaultwarden
cd ~/vaultwarden
nano docker-compose.yml
```

## 1. Environment Preparation
- A **Linux** virtual machine operating system.
- **Docker** and **Docker Compose** are installed.

## 2. Writing a Docker Compose File

Instead of automatically building the application from raw source code using a `Dockerfile`, this system uses `docker-compose.yml` to download optimized official images from Docker Hub.

Link Vaultwarden: [https://github.com/dani-garcia/vaultwarden]

## 3. Create a  YAML file docker-compose.yml
  - By applying the Bind Mount mechanism, all password data (/data) will be leaked and stored in the ./vw-data directory on the Ubuntu server. If the container is deleted, the data remains secure.
  - Resource limitations: 0.5 CPU, 512mb RAM
  - Only the Nginx Proxy Manager is allowed to open ports (80, 443 for the Internet, 81 for administration). Vaultwarden is completely hidden behind a virtual network.
  
## 5. Configure Internal SSL Certificate
``` bash 
openssl req -x509 -nodes -days 3650 -newkey rsa:2048 -keyout /tmp/vault_local.key -out /tmp/vault_local.crt -subj "/CN=vault.local" -addext "subjectAltName=DNS:vault.local,IP:<...Your host IP...>" 
```
Note: Replace <...Your host IP...> with the actual IP address of your vitural machine (e.g., 192.168.10.142).

Access the Nginx Proxy Manager admin interface (Port 81), add the newly created .key and .crt files to the SSL Certificates section, and configure the Proxy Host to point the vault.local domain to the server's IP address, along with the Force SSL option.


## 6. Add bitwardern extension to your browser

Upload SSL certificate
<img width="1278" height="799" alt="image" src="https://github.com/user-attachments/assets/c00a5bf8-e1b2-4e59-a819-0fda7b68bd8b" />
Create a proxy host with custom SSL
<img width="1275" height="796" alt="image" src="https://github.com/user-attachments/assets/47de4082-27a7-4f4a-bc40-0a0a56975070" />

## 7. Add bitwardern extension to your browser

Login through ```vault.local```
<img width="1279" height="797" alt="image" src="https://github.com/user-attachments/assets/ec9c28b4-49ce-4e3f-94d1-ae38124697b7" />
Add new login credential
<img width="1278" height="796" alt="image" src="https://github.com/user-attachments/assets/35ef5a95-716a-4050-8671-b3e5c0c2be1b" />



## 8. Add bitwardern extension to your browser
Add extension
<img width="1276" height="507" alt="image" src="https://github.com/user-attachments/assets/a5ee2868-2b3a-4ae7-ae46-aeb588ba3dcf" />
Choose self host
<img width="1279" height="799" alt="image" src="https://github.com/user-attachments/assets/0faa6272-6606-41fd-b493-7ca0f9760615" />
<img width="1275" height="829" alt="image" src="https://github.com/user-attachments/assets/3432302e-5cbc-4772-ae1e-b688525676a7" />
Fill your credential
<img width="1280" height="800" alt="image" src="https://github.com/user-attachments/assets/397286a2-70e5-4513-a5b0-88fcaecee91f" />

## 9. Locked signup

After successfully accessing the website and registering an Admin account, you must disable the free registration feature to block unauthorized users.
```bash
nano docker-compose.yml
```

Change the line SIGNUPS_ALLOWED=true to false:
``` bash
- SIGNUPS_ALLOWED=false
```

Compare the configuration file state with the actual system. If there are any changes, it will delete the old container, reconfigure the network/volume, and create a completely new container.
``` bash
docker compose up -d
```



