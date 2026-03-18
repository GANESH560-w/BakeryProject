# README

## 🗄️ MariaDB Setup and Configuration Guide for Windows/Ubuntu
This guide explains how to set up MariaDB, create a database, and Create Database User.

### 1. Installing MariaDB
**Installing MariaDB on Ubuntu:**
```bash
apt update && apt install mariadb-server -y
```

### 2. Securing MariaDB
Open the Command Prompt as Administrator and run the following command to secure your installation:
```bash
mysql_secure_installation
```

### 3. Setting Up the Database
Open terminal and login to MariaDB:
```bash
mysql -u root -p
```
Enter the root password when prompted.

**Create a new database and user:**
```sql
CREATE DATABASE bakery_db;
CREATE USER 'admin'@'localhost' IDENTIFIED BY 'ganeshjadhav';
GRANT ALL PRIVILEGES ON bakery_db.* TO 'admin'@'localhost';
FLUSH PRIVILEGES;
```

### 4. Database Credentials
- **DB_HOST**: `localhost` (or AWS RDS Endpoint)
- **DB_USER**: `admin`
- **DB_PASS**: `ganeshjadhav`
- **DB_NAME**: `bakery_db`

---

## ☁️ AWS EC2 & RDS Deployment Guide (Full Details)

### Step 1: Build Infrastructure
1.  **Build EC2**: Launch an Ubuntu 22.04 instance.
2.  **Build RDS**: Create a MySQL/MariaDB database (Choose defaults/Free Tier).
3.  **Security Groups**: 
    - Allow Port 80 (HTTP) and 8082 (Backend) for EC2.
    - Allow Port 3306 (MySQL) for RDS from EC2 Security Group.

### Step 2: Connect RDS with EC2
1. SSH into your EC2.
2. Update the system:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```
3. Install MySQL Client:
   ```bash
   sudo apt install mysql-client -y
   ```
4. Connect to RDS:
   ```bash
   mysql -h <rds-endpoint> -u <username> -p
   # Press Enter, then type your password
   ```
5. Create Database:
   ```sql
   CREATE DATABASE bakery_db;
   EXIT;
   ```

### Step 3: Configure EC2 with Docker
1. Install Docker & Docker Compose:
   ```bash
   sudo apt install docker.io docker-compose -y
   sudo systemctl start docker
   sudo systemctl enable docker
   ```
2. **Update Files with Public IP**:
   - In [main.js](file:///c:/Users/Ganesh/OneDrive/Desktop/Backery%20Mangement%20Project/js/main.js), change the IP to your **EC2 Public IP**.
   - This is necessary for Admin login, Emp login, and Contact forms to work.
3. Build and Run:
   ```bash
   docker-compose up --build -d
   ```
4. Check Logs:
   ```bash
   docker-compose logs -f
   ```
5. Access the app: Paste your **EC2 Public IP** in Chrome browser.

---
