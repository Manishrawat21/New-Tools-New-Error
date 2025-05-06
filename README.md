# New-Tools-New-Error

# 🛡️ TheHive and Cortex Docker Deployment & Troubleshooting Guide

This repository documents the full journey of deploying and troubleshooting **TheHive** and **Cortex** using **Docker** on a Kali Linux system. It highlights common issues, debugging strategies, and provides a clear installation path—especially useful for security analysts and SOC teams looking to automate incident response.

---

## 📌 Table of Contents

- [🔍 Introduction](#-introduction)
- [🧰 Pre-requisites](#-pre-requisites)
- [🐝 Installing TheHive](#-installing-thehive)
- [⚠️ Troubleshooting TheHive](#-troubleshooting-thehive)
- [🔬 Installing Cortex](#-installing-cortex)
- [⚒️ Troubleshooting Cortex](#-troubleshooting-cortex)
- [📦 Docker Cleanup & Image Management](#-docker-cleanup--image-management)
- [✅ Conclusion](#-conclusion)
- [🤝 Contributions](#-contributions)

---

## 🔍 Introduction

**TheHive** is an open-source Security Incident Response Platform (SIRP).  
**Cortex** complements TheHive by providing automated analysis capabilities.

This guide shares a hands-on experience setting up both tools in Docker and documents errors encountered—along with how they were resolved—for future reference and community support.

---

## 🧰 Pre-requisites

- Docker installed (`docker --version`)
- Sufficient privileges (sudo/root)
- Kali Linux environment (or any modern Debian-based distro)

```bash
sudo apt update && sudo apt install docker.io
sudo systemctl enable --now docker

🐝 Installing TheHive

docker pull strangebee/thehive:5.0.23
docker run -d --name thehive -p 9000:9000 strangebee/thehive:5.0.23

⚠️ Troubleshooting TheHive
❌ Error: AccessDeniedException: /opt/thp/thehive/db/je.properties
🔎 Cause:

File system permissions were incorrect.
✅ Fix:

Create a valid user and set ownership:

sudo useradd -r thehive
sudo mkdir -p /opt/thp/thehive/db
sudo chown -R thehive:thehive /opt/thp

❌ Error: thehive.service not found
🔎 Cause:

TheHive wasn't installed as a system service—this is expected in Docker-based deployment.
🔬 Installing Cortex

docker pull cortexproject/cortex:latest
docker run -d -p 9001:9001 --name cortex cortexproject/cortex:latest

⚒️ Troubleshooting Cortex

If the image was pulled but not run:

docker images  # to list downloaded images

If trying to remove an image in use:
❌ Error:

Error response from daemon: conflict: unable to delete (must be forced) - image is being used by a container

✅ Fix:

docker ps -a                 # List all containers
docker rm <container-id>     # Remove the container
docker rmi <image-id>        # Now remove the image

📦 Docker Cleanup & Image Management

Remove stopped containers:

docker container prune

Remove unused images:

docker image prune

✅ Conclusion

This documentation serves as a quick reference for setting up TheHive and Cortex using Docker. It reflects real-world setup challenges and resolutions, helping analysts avoid common pitfalls.
🤝 Contributions

Feel free to fork, contribute, or open issues to help improve this documentation. Security is a collaborative effort!

Author: Manish Rawat – Security Analyst
License: MIT
