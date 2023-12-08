# Monolithic Application Deployment on a Single EC2 Instance 

I successfully deployed a WordPress application and MySQL database on a single EC2 instance.

## 1. Launched an EC2 Instance:

I chose an appropriate instance type (t2.micro).
Image of EC2 instance is ubuntu.

## 2. Configured Security Groups:

I allowed inbound traffic for HTTP/HTTPS (port 80 and 443) for WordPress access and port 3306 for MySQL access.

## 3. Installed and Configured MySQL:

I securely installed MySQL using the apt-get command.
I created a dedicated user and database for WordPress using the mysql command.
```
 sudo apt install apache2
 sudo apt install php libapache2-mod-php php-mysql
 sudo mysql -u root
 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'Testpassword@123';
 CREATE USER 'wp_user'@localhost IDENTIFIED BY 'Testpassword@123';
 CREATE DATABASE wp;
 GRANT ALL PRIVILEGES ON wp.* TO 'wp_user'@localhost;
```
## 4. Installed and Configured WordPress:

I downloaded the latest WordPress version and extracted it to the desired directory.
I edited the wp-config.php file and configured the database connection details.
I launched the WordPress installation wizard and completed the setup process.
```
sudo apt update
sudo apt install apache2
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvf latest.tar.gz
sudo mv wordpress/ /var/www/html
cd wordpress/
vi wp-config.php
```

## 5. Tested and Verified Functionality:

I accessed the WordPress website through a web browser and verified everything was working as expected.
Overall, the deployment process was successful, and the WordPress application and MySQL database are now running on the single EC2 instance.

