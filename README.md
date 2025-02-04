# AssessableActivity2_WAD_2024_25

# Assessable Activity 2 - Web Applications Deployment

This document describes the process of deploying a **React (Vite) application** on **AWS EC2** using **Apache with  proxy** and configuring the server.

## EC2 Instance Setup

I launched an **EC2 instance** on **AWS** with the following specifications:

- **Instance Type:** `t2.micro`
- **Operating System:** Ubuntu (latest version)
- **Security Group:** `default-web-sg`
- Allows HTTP (port 80) and SSH (port 22) traffic
- **Elastic IP:** I assigned an elastic IP to our EC2 instance assesable2-daw  to keep a static public IP address.

---

## Steps to Deploy the Application on EC2

### 1. Update the system package
```bash
sudo apt-get update
sudo apt-get install apache2
sudo apt-get install npm
sudo apt-get install -y nodejs

## 2. I clone the respository from github

git --version 
git clone https://github.com/gisgarme/adivina-el-numero

Then I navigated into the project directory. Then I renamed the directory to adivina to make it shorter.
 

## 3. Install dependecies 
Inside the project directory, I installed the required dependencies:

npm install
sudo npm install -g pm2

## 4. Start aplication with Vite

I used pm2 to run the Vite development server:

pm2 start npm --name "vite-server" -- run dev -- --host
npm run dev


## 5. Configure Apache as a reverse proxy

sudo a2enmod proxy proxy_http
sudo service  apache2 restart

Then, I created a Virtual Host configuration file:

cd /etc/apache2/sites-available/
sudo cp 000-default.conf numero.conf
sudo nano numero.conf


I added the following configuration:

<VirtualHost *:80>
    ServerName 18.210.2.231  
    ProxyRequests Off
    ProxyPreserveHost On
    ProxyPass / http://127.0.0.1:5173/
    ProxyPassReverse / http://127.0.0.1:5173/
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

sudo service apache2 restart 


## Deploy the application

Finally, I deployed  the application in a web browser using the Elastic IP:

http://18.210.2.231





