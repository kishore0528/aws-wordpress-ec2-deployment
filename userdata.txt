#!/bin/bash
# 0. Change the Host name
sudo 	hostnamectl set-hostname wordpress-server

# 1. UPDATE AND INSTALL ALL REQUIRED SOFTWARE
# This single line updates the package list and installs Apache, PHP,
# and all the necessary PHP extensions (for MySQL, XML, cURL, and images) at once.
apt update
apt install apache2 php libapache2-mod-php php-mysql php-xml php-curl php-gd mysql-client-core-8.0 -y

# 2. DOWNLOAD AND SET UP WORDPRESS
# Navigate to a temporary directory to download the files
cd /tmp
wget https://wordpress.org/latest.tar.gz

# Unpack the WordPress files
tar -xzf latest.tar.gz

# Copy the unpacked WordPress files to the web server's root directory
cp -r wordpress/* /var/www/html/

# Remove the default Apache welcome page to ensure WordPress loads
rm /var/www/html/index.html

# 3. CONFIGURE WORDPRESS TO CONNECT TO THE RDS DATABASE
# Navigate to the web root directory
cd /var/www/html/

# Create the wp-config.php file from the sample file provided by WordPress
cp wp-config-sample.php wp-config.php

# --- IMPORTANT: REPLACE THESE THREE VALUES BEFORE USE ---
DB_USER="<your_rds_username>"
DB_PASSWORD="<your_rds_password>"
DB_HOST="<your_rds_endpoint_url>"
# --------------------------------------------------------

# Use the 'sed' command to automatically insert your database details into the config file
sed -i "s/database_name_here/wordpressdb/g" wp-config.php
sed -i "s/username_here/$DB_USER/g" wp-config.php
sed -i "s/password_here/$DB_PASSWORD/g" wp-config.php
sed -i "s/localhost/$DB_HOST/g" wp-config.php

# 4. SET PERMISSIONS AND RESTART THE SERVER
# Change the ownership of all files in the web root to the web server user (www-data for Ubuntu)
# This allows WordPress to manage themes, plugins, and uploads.
chown -R www-data:www-data /var/www/html/

# Restart the Apache web server to apply all changes
systemctl restart apache2