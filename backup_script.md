# Create a script that create a backup of the application and save From EC2 instance it to the s3 bucket.

## Steps to follow
* Launch EC2 instance  
* Install ngnix server
```
sudo apt update -y
sudo apt install nginx -y
```
* After installation, you can check the status of your Nginx server 
```
sudo systemctl status nginx
```
* Before testing Nginx, the firewall software needs to be adjusted to allow access to the service.
```
sudo ufw app list
```
* You should get a listing of the application profiles:
    * Nginx Full
    * Nginx HTTP
    * Nginx HTTPS
    * OpenSSH
* Create a web page using html,css etc and name it as index.html 
* Move index.html file to ngnix server location.
```
sudo mv index.html /usr/share/nginx/html/
```
* Now run the server and web page.
* Configure aws-cli for creating s3 bucket script.
```
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```
* Now create s3 bucket 
* Edit the security rule of EC2 instance and add IAM role in it.
* Give amazonfullaccess to the role and update it.
* Now write script for backup
```
sudo tar -cvzf nginx_$(date +%d%m%y).tgz nginx/
aws s3 cp /var/log/nginx_251223.tgz s3://backup-bucket-script-23/backup
```
* NGINX writes its events in two types of logs - the error log and the access log,access and error log can be found in /var/log/nginx 
* We'll take take backup of these logs by running the script.
```
S3_BUCKET="backup-bucket-script-23"
BACKUP_DIR="/var/log"
APP_NAME="nginx_server_backup"

# Create a timestamp for the backup file
TIMESTAMP=$(date "+%Y%m%d%H%M%S")
BACKUP_FILE="${APP_NAME}_backup_${TIMESTAMP}.tar.gz"

# Step 1: Create a backup of your application (adjust this based on your application)
tar -czvf "${BACKUP_DIR}/${BACKUP_FILE}" /usr/sbin/nginx

# Step 2: Upload the backup to S3
aws s3 cp "${BACKUP_DIR}/${BACKUP_FILE}" "s3://${S3_BUCKET}/${BACKUP_FILE}" --region "${AWS_REGION}"

# Step 3: Clean up local backup (optional)
# Uncomment the line below if you want to delete the local backup after uploading to S3
# rm "${BACKUP_DIR}/${BACKUP_FILE}"

echo "Backup completed and uploaded to S3: s3://${S3_BUCKET}/${BACKUP_FILE}"

```

