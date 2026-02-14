# 🚀 GCP Managed Instance Group + Load Balancer Project
 
## 📌 Project Overview

This project demonstrates how to build a **scalable, highly available web application architecture** in Google Cloud Platform (GCP) using:

* Google Cloud Managed Instance Group
* Google Cloud Load Balancing
* Health Checks
* Autoscaling (CPU-based)
* Startup Scripts

The goal of this project was to simulate a real-world production environment where application traffic is distributed across multiple virtual machines and automatically scales based on demand.

---

## 🏗 Architecture

```
User
  ↓
Global HTTP Load Balancer
  ↓
Backend Service
  ↓
Managed Instance Group (MIG)
  ↓
Multiple VM Instances (Apache Web Server)
```

---

## 🎯 Objectives

* Deploy identical VM instances using Instance Templates
* Automatically distribute traffic across instances
* Enable health checks and auto-healing
* Configure CPU-based autoscaling
* Simulate traffic and test scaling behavior

---

## ⚙️ Technologies Used

* Google Compute Engine
* Managed Instance Groups
* Global HTTP Load Balancer
* Health Checks
* Autoscaler
* Debian 12
* Apache Web Server
* gcloud CLI

---

## 🛠 Implementation Steps

### 1️⃣ Startup Script

Each VM installs and runs Apache automatically:

```bash
#!/bin/bash
apt-get update
apt-get install -y apache2

cat <<EOF > /var/www/html/index.html
<h1>Hello from $(hostname)</h1>
EOF

systemctl start apache2
systemctl enable apache2
```

---

### 2️⃣ Instance Template

Created a reusable VM template with:

* e2-micro machine type
* Debian 12 image
* Startup script
* HTTP firewall tag

---

### 3️⃣ Managed Instance Group (MIG)

* Initial size: 2 instances
* Auto-healing enabled via health checks
* Backend attached to load balancer

---

### 4️⃣ Health Check

HTTP health check configured on port 80 to ensure instance availability.

---

### 5️⃣ Global HTTP Load Balancer

Configured:

* Backend service
* URL map
* Target HTTP proxy
* Global forwarding rule (Port 80)

Traffic is evenly distributed across healthy instances.

---

### 6️⃣ Autoscaling Configuration

Configured CPU-based autoscaling:

* Target CPU utilization: 60%
* Minimum instances: 2
* Maximum instances: 5

The system automatically scales out during high load and scales in when traffic decreases.

---

## 🧪 Testing & Validation

### ✔ Load Balancing Verification

Repeated browser refresh showed different hostnames:

```
Hello from web-server-xxxx
```

Confirming traffic distribution.

### ✔ Health Check Verification

Backend status:

```
HEALTHY
```

### ✔ Autoscaling Test

Simulated traffic using continuous curl requests and CPU stress testing.

Observed:

* New instances created automatically
* Instances removed after load decreased

---

## 🚧 Challenges Faced

* Browser defaulted to HTTPS while only HTTP load balancer was configured.
* Encountered `ERR_CONNECTION_RESET` due to protocol mismatch.
* Resolved by explicitly using:

  ```
  http://<LOAD_BALANCER_IP>
  ```

This reinforced the importance of understanding HTTP vs HTTPS load balancer configurations.

---

## 🧠 Key Learnings

* Designing high availability systems in GCP
* Difference between HTTP and HTTPS Load Balancers
* Understanding backend health states
* Debugging load balancer connection issues
* Implementing autoscaling strategies
* Production-style cloud architecture thinking

---

## 📈 Production Concepts Demonstrated

* Horizontal Scaling
* Auto-healing Infrastructure
* Stateless Application Design
* Traffic Distribution
* Fault Tolerance

---

## 🏆 Outcome

Successfully built a scalable, fault-tolerant web infrastructure in GCP that:

* Automatically distributes traffic
* Detects and replaces failed instances
* Scales based on CPU usage
* Mimics real-world production environments

---

## 📌 Future Improvements

* Add HTTPS Load Balancer with SSL certificate
* Configure HTTP → HTTPS redirection
* Use Terraform for Infrastructure as Code
* Integrate monitoring with Cloud Monitoring
* Deploy custom domain

**Author**
Amulya
