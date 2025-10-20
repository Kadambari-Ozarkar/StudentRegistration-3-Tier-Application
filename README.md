# AWS 3-Tier Student Registration Web Application
________________________________________________________________________________
To design, develop, and deploy a **cloud-based Student Registration System** using **AWS 3-Tier Architecture**, ensuring secure data management, modular structure, and high scalability through the **LEMP Stack (Linux, Nginx, MySQL, PHP)**.
________________________________________________________________________________
## 💡  1. Architecture Overview

The application follows a classic **3-Tier model**:

| Tier            | Component      | Function                      | Network Placement |
|-----------------|---------------|-------------------------------|-----------------|
| Web Tier        | Nginx + HTML  | Handles frontend requests     | Public Subnet   |
| Application Tier| PHP           | Processes logic and data flow | Private Subnet  |
| Database Tier   | MySQL         | Stores data securely          | Private Subnet  |

________________________________________________________________________________
## 🛠️  2. Tech Stack
- **Cloud Platform:** AWS 
- **Frontend:** HTML, CSS
- **Backend:** PHP
- **Database:** MySQL
- **Web Server:** Nginx
- **Operating System:** Amazon Linux 2
________________________________________________________________________________
##  📂  3.Project Structure
##### 3-tier-architecture-aws/
├─ webserver/           
│   ├─ form.html        
│   ├─ nginx.conf       
│
├─ appserver/           
│   ├─ test.php         
│   ├─ submit.php       
│
├─ dbserver/            
│   ├─ database.sql     
│
└─ README.md          



________________________________________________________________________________


## 🖥️️ ️3. AWS Infrastructure Setup

### I. VPC and Subnets
- Created a **Custom VPC** with CIDR `10.0.0.0/16`.
- Created three subnets:
  - **web-subnet (Public)**
  - **app-subnet (Private)**
  - **db-subnet (Private)**
### II.Create an Internet Gateway
- This creates a new Internet Gateway with the tag MyIGW
- Attach the Internet Gateway to your VPC
### III. Route Tables
- **Public Route Table:** Connected to the **Internet Gateway**.  
- **Private Route Table:** Connected to the **NAT Gateway**.
- Attach **Public Subnet** to **Public Route Table**.
 
### IV. EC2 Instances

- **Web Server (Public Subnet):** Delivers the HTML frontend and forwards requests to the Application Server.  
- **Application Server (Private Subnet):** Executes backend logic using PHP.  
- **Database Server (Private Subnet):** Stores and manages data securely using MySQL.
________________________________________________________________________________
## 🔒 4. Security Group Configuration

| Server   | Allowed Source      | Allowed Ports | Description                       |
|----------|-------------------|---------------|-----------------------------------|
| Web SG   | Internet (0.0.0.0/0) | 80            | Public web access                 |
| App SG   | Web SG             | 80            | Accepts traffic from Web Server  |
| DB SG    | App SG             | 3306          | Accepts only App Server connections |


________________________________________________________________________________

## 🖥️ Server Setup & Configuration

### 🌐 Web Server (Public Subnet)



#### 1.Install Nginx
```bash
sudo yum install nginx -y
sudo systemctl enable nginx.service
sudo systemctl start nginx.service 
```
#### 2. Deploy Frontend
Edit your HTML file to create the registration form:
```bash
sudo nano /usr/share/nginx/html/form.html
```
### 3.Configure Nginx (Web Server)
Update Nginx to forward requests to the App Server:
```bash
sudo nano /etc/nginx/nginx.conf
```
Note :
1 . This block should be added inside the server block of your Nginx configuration.
2 .Replace **<APP-SERVER-PRIVATE-IP>** with the private IP of your App Server.
```
location ~ \.php$ {
    proxy_pass http://<APP-SERVER-PRIVATE-IP>;
}
```
### 4.Restart Nginx
```bash
sudo service nginx reload
sudo service nginx restart
```
________________________________________________________________________________
## ⚙️ Application Server (Private Subnet)


### 1. Install PHP and dependencies
```bash
sudo dnf install php php-fpm mariadb105-server php-mysqlnd -y
sudo systemctl enable php-fpm.service
sudo systemctl start php-fpm.service
```
### 2.Deploy Backend Scripts
```bash
cd /usr/share/nginx/html/
sudo nano submit.php
sudo nano test.php
```
________________________________________________________________________________
## 🗄️ Database Server (Private Subnet)
### 1.Install MariaDB
```bash
sudo dnf install mariadb105-server -y
sudo systemctl enable mariadb.service
sudo systemctl start mariadb.service
```
### 2.Login to mysql
```bash
sudo mysql -u root -p
```
### 3.Run SQL script:
```bash
SOURCE /path/to/studentdb.sql;
```

### ⚠️ Cleanup Note
> Delete the **NAT Gateway** after use.  
> Release the associated **Elastic IP** to avoid extra charges.
________________________________________________________________________________
## ✅verification
>Open in browser: http://WEB-SERVER-PUBLIC-IP/form.html
>Check the database entries
```
select * from students;
```
________________________________________________________________________________
## 📡  Deployment 
- The **Web Server** acts as a bastion host for accessing private subnets.  
- Use `scp` to transfer `.pem` keys to the Web Server for connecting to App & DB servers.  
- Always use **private IPs** for communication between the App and DB tiers.  
- Keep database credentials inside the **private subnet only** (never exposed publicly).  
________________________________________________________________________________
### 🔮 Future Enhancements
- Implement **user authentication and role-based access control** for secure logins.  
- Add **email notifications** for successful registration or updates.  
- Integrate **AWS RDS** for a managed database service.  
- Enable **autoscaling and load balancing** for higher availability.  
- Implement **logging and monitoring** using AWS CloudWatch.  
- Add a **responsive frontend design** for mobile and tablet users.  
________________________________________________________________________________
## ✍️ Author
  
 **Kadambari Ozarkar**

















  
