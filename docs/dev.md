# Development memo

This document describe more detail information of setting environtment for development.

We use the URL http://local.crm-yii.com to access to the test environment.

## Environment

* Use PHP 7.0.x. Don't use newer version of PHP (because the confliction of Object class). 
* The folder of the program should be ```www.crm-yii.com``` (due to the setting in *web/server-configuration.php*)


## Installation

Steps to prepare development environtment.
1. Create dabtabases.
2. Setup *hosts* file.
3. Setup vhosts of Apache.
 
### Create database

This software need MySQL as database. Its migration cannot work with PostgreSQL.

To create MySQL databases for both dev, staging and production.

```SQL
CREATE DATABASE crm_yii CHARACTER SET UTF8;
GRANT ALL PRIVILEGES ON crm_yii.* TO 'crm-yii'@'localhost' IDENTIFIED BY 'crm-yii-pass';

CREATE DATABASE crm_yii_stage CHARACTER SET UTF8;
GRANT ALL PRIVILEGES ON crm_yii.* TO 'crm-yii-stage'@'localhost' IDENTIFIED BY 'crm-yii-stage-pass';

CREATE DATABASE crm_yii_prod CHARACTER SET UTF8;
GRANT ALL PRIVILEGES ON crm_yii.* TO 'crm-yii-prod'@'localhost' IDENTIFIED BY 'crm-yii-prod-pass';
```

### Setup hosts file on your client PC.

The example below assumes that 192.168.33.10 is your server's IP address.

```
192.168.33.10       crm-yii.com
192.168.33.10       test.crm-yii.com
192.168.33.10       local.crm-yii.com
192.168.33.10       www.crm-yii.com
```

### Setup vhosts of Apache

```
<VirtualHost *:80>
    ServerName local.crm-yii.com
    DocumentRoot "/vagrant/openSource/www.crm-yii.com/web"
    ErrorLog "logs/crm-yii-error.log"
    CustomLog "logs/crm-yii-access.log" common
    <Directory "/vagrant/openSource/www.crm-yii.com/web">
                AllowOverride All
                Require all granted
                DirectoryIndex index.php index.html
                Options Indexes MultiViews FollowSymLinks
    </Directory>
</VirtualHost>

```

### Using yii2-check-login-attempts

* Run migration
```shell
./yii migrate --migrationPath="@vendor/giannisdag/yii2-check-login-attempts/src/migrations"
```