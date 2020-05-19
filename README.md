# REDCAP


## Apache 2
```
sudo apt install  apache2 apachetop apache2-utils 
sudo a2enmod rewrite
sudo a2enmod deflate
sudo a2enmod expires
sudo systemctl restart apache2
sudo systemctl status apache2


```

## PHP 7.4
```
sudo apt install php7.4-{cli,common,curl,intl,opcache,readline,mysql,json}
sudo apt install libapache2-mod-php7.4 
sudo a2enmod php7.4
sudo journalctl -f -u apache2
apache2ctl -M

```
## Essential PHP Extensions
```
sudo apt install php-xml
sudo apt install php-gd libgd-tools
sudo apt install php7.4-zip
sudo systemctl restart apache
```

## PHP settings
```
sudo cp /etc/php/7.4/apache2/php.ini   /etc/php/7.4/apache2/php.ini.orig

sudo sed -i "s/;max_input_vars = 1000/max_input_vars = 10000/" /etc/php/7.4/apache2/php.ini 
sudo cat /etc/php/7.4/apache2/php.ini  | grep max_input_vars

sudo cat /etc/php/7.4/apache2/php.ini  | grep upload_max_filesize
sudo cat /etc/php/7.4/apache2/php.ini  | grep post_max_size
sudo sed -i "s/upload_max_filesize = .*/upload_max_filesize = 15M/" /etc/php/7.4/apache2/php.ini 
sudo sed -i "s/post_max_size = .*/post_max_size = 50M/" /etc/php/7.4/apache2/php.ini 
sudo cat /etc/php/7.4/apache2/php.ini  | grep upload_max_filesize
sudo cat /etc/php/7.4/apache2/php.ini  | grep post_max_size

sudo sed -i "s/;session.cookie_secure =*/session.cookie_secure =1/" /etc/php/7.4/apache2/php.ini 
sudo systemctl restart apache2

```


## Cron JOb
```
sudo crontab -l
sudo crontab -e
# REDCap Cron Job (runs every minute)
* * * * * /usr/bin/php /var/www/html/redcap990/cron.php > /dev/null
```

## COPY FILES

```
scp Downloads/redcap9.9.0.zip USER@192.168.XX.XX:~/  
ssh orangecap



```


## Redcap 9.9.0
```
unzip redcap9.9.0.zip 
mv redcap redcap990

cd ~
sudo rm -r  /var/www/html/redcap990
sudo cp -R redcap990/ /var/www/html/
sudo chown -R www-data:www-data /var/www/html

cd /var/www/html/redcap990

sudo sed -i "s/your_mysql_host_name/192.168.XX.XXX/" /var/www/html/redcap991/database.php
sudo sed -i "s/'your_mysql_db_name'/'redcap'/" /var/www/html/redcap991/database.php
sudo sed -i "s/'your_mysql_db_username'/'USERNAME'/" /var/www/html/redcap991/database.php
sudo sed -i "s/'your_mysql_db_password'/'PASSWORD'/" /var/www/html/redcap991/database.php
sudo sed -i "s/salt = ''/salt = 'RANDOMTEXT'/" /var/www/html/redcap991/database.php
cat /var/www/html/redcap991/database.php 

```

## Configure Apache
```
sudo sed -i "s/\/var\/www\/html/\/var\/www\/html\/redcap990/" /etc/apache2/sites-available/000-default.conf
sudo systemctl restart apache

```

### A working Structure
```
  GNU nano 4.8                                                                                          /etc/apache2/sites-available/000-default.conf                                                                                          Modified  
<VirtualHost *:80>
        ServerName redcap.aiims.edu
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html/redcap990

    <Directory /var/www/html/redcap990>
#        Options -Indexes +FollowSymLinks +MultiViews +FollowSymLinks +Includes
        AllowOverride All
        Require all granted
    </Directory>

#    <FilesMatch \.php$>
        # 2.4.10+ can proxy to unix socket
#       SetHandler "proxy:unix:/var/run/php/php7.3-fpm.sock|fcgi://localhost"
#   </FilesMatch>

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet


```

## Configure Redcap

Check configuration
Create directory for user upload Files

```
cd /var/www/html
sudo mkdir redcap_uploads
sudo chown -R www-data:www-data /var/www/html/redcap_uploads
```

https://redcap.aiims.edu/redcap_v9.9.0/ControlCenter/file_upload_settings.php

### Add Users - deisgnate Admins - Enable Authentication

https://redcap.aiims.edu/redcap_v9.9.0/ControlCenter/create_user.php

https://redcap.aiims.edu/redcap_v9.9.0/ControlCenter/superusers.php

https://redcap.aiims.edu/redcap_v9.9.0/ControlCenter/security_settings.php







## Redcap 9.9.1
```
cd ~
rm -R rc991
mkdir rc991
cp redcap9.9.1.zip  rc991
cd rc991
unzip redcap9.9.1.zip 
mv redcap redcap991


sudo rm -r  /var/www/html/redcap991

sudo cp -R redcap991/ /var/www/html/
sudo chown -R www-data:www-data /var/www/html

cd /var/www/html/redcap991

sudo sed -i "s/your_mysql_host_name/192.168.XX.XXX/" /var/www/html/redcap991/database.php
sudo sed -i "s/'your_mysql_db_name'/'redcap'/" /var/www/html/redcap991/database.php
sudo sed -i "s/'your_mysql_db_username'/'USERNAME'/" /var/www/html/redcap991/database.php
sudo sed -i "s/'your_mysql_db_password'/'PASSWORD'/" /var/www/html/redcap991/database.php
sudo sed -i "s/salt = ''/salt = 'RANDOMTEXT'/" /var/www/html/redcap991/database.php
cat /var/www/html/redcap991/database.php 
```


