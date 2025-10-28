## 🚀 Project Setup Guide
---

## ⚙️ 1. Local Setup

### 1. Install Prerequisites
### 2. Setup Project Folder

Create a folder (e.g., `user-registration-system`) and add:

* `index.html` – Homepage
* `register.html` – Registration form
* `login.html` – Login page
* `config.php` – Database connection file
* `register.php` & `login.php` – Backend scripts

### 3. Local Database Setup

* Open mysql workbench → Create a database named `user_db`

  CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255)
  );
  ```
* Update `config.php`:

  ```php
  <?php
  $conn = mysqli_connect("localhost", "root", "", "user_db");
  if (!$conn) {
      die("Connection failed: " . mysqli_connect_error());
  }
  ?>
  ```

---

## ☁️ 2. AWS Setup

### 1. Launch EC2 Instance

* Go to AWS Management Console → **EC2**
* Launch a new instance (Amazon Linux or Ubuntu)
* Configure security group:

  * Allow **HTTP (port 80)**
  * Allow **MySQL (port 3306)** (only for RDS)
  * Allow **SSH (port 22)** (for access)
* Connect to your instance using SSH.

### 2. Install Required Packages

```bash
sudo yum update
sudo yum install apache2 php php-mysql -y
```

### 3. Upload Your Website Files

You can use SCP or upload via terminal:

```bash
scp -i "your-key.pem" -r /path/to/your/project ubuntu@your-ec2-public-ip:/var/www/html/
```

Restart Apache:

```bash
sudo systemctl restart apache2
```

---

## 🗄️ 3. Configure RDS Database

### 1. Create RDS Instance

* Go to **AWS RDS → Create Database**
* Engine: MySQL
* Set username & password
* Enable “Publicly Accessible”
* Add security group rule for EC2 access

### 2. Get RDS Endpoint

Copy the endpoint (e.g., `mydb.xxxxx.us-east-1.rds.amazonaws.com`)

### 3. Update `config.php`

```php
<?php
$conn = mysqli_connect("mydb.xxxxx.us-east-1.rds.amazonaws.com", "admin", "yourpassword", "user_db");
if (!$conn) {
    die("Connection failed: " . mysqli_connect_error());
}
?>
```

---

## 🌐 4. Configure Domain with Route 53

### 1. Register Domain

* Go to **AWS Route 53 → Registered Domains**
* Buy or connect an existing domain (e.g., `www.yourdomain.com`)

### 2. Create Hosted Zone

* Add a **record set**:

  * Type: `A - IPv4 address`
  * Value: your **EC2 public IP**

### 3. Test

Visit your domain — it should load your website hosted on EC2.

---

## ✅ 5. Verify End-to-End Functionality

1. Open your domain in a browser
2. Fill out the registration form
3. Check your RDS database — user details should appear there
