**Prepare**
* Check lsb
    - sudo yum install redhat-lsb-core
    - lsb_release -d

**1. SSH**
+ Path ~./ssh/config
+ ssh server_name

* Host server_name
* HostName 123.456.789.123
* User username
* Port 22
* IdentityFile ~/.ssh/keys.pem

**2. PHP**
+ sudo yum install -y amazon-linux-extras
+ sudo amazon-linux-extras | grep php
+ sudo amazon-linux-extras enable php7.4
+ sudo yum install php php7.4-{pear,cgi,common,curl,mbstring,gd,mysqlnd,gettext,bcmath,json,xml,fpm,intl,zip,imap}

**3. Composer and git**
+ cd ~ && sudo curl -sS https://getcomposer.org/installer | sudo php && sudo mv composer.phar /usr/local/bin/composer && sudo ln -s /usr/local/bin/composer /usr/bin/composer
+ sudo yum install git

**4. Nginx**
* Install:
  - sudo amazon-linux-extras install nginx1
  - nginx -t
  - nginx -v
* Config path:
  - /etc/nginx/nginx.conf
  - In nginx.conf, change root to /var/www/src/public
  - Add file manual.conf
    + cd /etc/nginx/default.d
    + nano manual.conf
        . location / {
			try_files $uri $uri/ /index.php?$query_string;
		}
* Restart service nginx
  - sudo fuser -k 80/tcp
  - sudo fuser -k 443/tcp
  - sudo service nginx start

**5. Mysql install**
* Install:
  - sudo yum install mysql
* Check connect
  - mysql -h hostname -u username -p
  - show databases;
  - create database mydb;

**6. Install src**
- in /var/www, mkdir src/
- sudo chmod -R 777 src/
- cd src
- git init
- git remote add origin git@github.com:User/UserRepo.git
- cp .env.example .env
- composer install
- sudo chmod -R 775 storage/

**7. EC2 and Mysql RDS**
* EC2
  - permit port 80 and 443
* RDS
  - permit 3306 with ec2's ip
  (at VPC security groups -> Inbound rules)
  - EX :
    + mysql/Aurora : 3306 
	 + Custom : 10.100.0.137/32 ( => 10.100.0.137 login SSH : ec2-user@ip-10-100-0-137 )
