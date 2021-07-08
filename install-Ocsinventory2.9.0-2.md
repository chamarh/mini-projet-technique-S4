Step 1 - Install Apache

#yum install -y httpd

#systemctl start httpd

#systemctl enable httpd

-------------------------------------------

Step 2 - Install MariaDB Database 10x

*create repo for MariaDb 10x : 

#vim /etc/yum.repos.d/MariaDB.repo

*add this code to the file created: 

---------------------
[mariadb]
name = MariaDB

baseurl = http://yum.mariadb.org/10.2/centos7-amd64

gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB

gpgcheck=1

---------------------

*then install MariaDB 10.2 : 

#yum install -y MariaDB-server MariaDB-client

*start mariadb: 

#systemctl start mariadb 

#systemctl enable mariadb

*secure MariaDb : 

#mysql_secure_installation

-------------------------------------------

Step 3 - Install PHP 7x and the required PHP extensions

#yum -y install epel-release yum-utils

#yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm

*enable the remi php 7.2 repository : 

#yum-config-manager --enable remi-php72

*install php and php server :

#yum install -y php php-common php-domxml php-gd php-imap php-ldap php-mbstring php-mysql php-opcache php-pdo php-pear-CAS php-pecl-apcu php-pecl-zip php-soap php-xmlrpc

-------------------------------------------

Step 4 - Install serveral PERL extensions required by OCS Inventory

#yum install -y perl-Archive-Zip perl-Compress-Zlib perl-DBD-MySQL perl-DBI perl-Mojolicious perl-Net-IP perl-Plack perl-SOAP-Lite perl-Switch perl-XML-Entities perl-XML-Simple

-------------------------------------------

Step 5 - Create database for OCS Inventory

*loginMariaDB server: 

#mysql -uroot -p 

>create database ocsweb;

>grant all privileges on ocsweb.* to ocs@localhost identified by 'ocs';

>flush privilages;

/**Recommend during the initial setup to use the default database,username and password.

Database : ocsweb

User : ocs

Password : ocs

**/

-------------------------------------------

Step 6 - Install OCS Inventory Server

#yum install -y https://rpm.ocsinventory-ng.org/ocsinventory-release-latest.el7.ocs.noarch.rpm

#yum install -y ocsinventory-server

-------------------------------------------

Step 7 - Install the OCS Management Server 

#cd /opt/

*download ocs management servery : 

#wget https://github.com/OCSInventory-NG/OCSInventory-ocsreports/releases/download/2.6/OCSNG_UNIX_SERVER_2.6.tar.gz

#tar -xvf OCSNG_UNIX_SERVER_2.6.tar.gz

#cd OCSNG_UNIX_SERVER_2.6/

#sh setup.sh

#cd /usr/share

#chmod -R 766 ocsinventory-reports/

*changing the owner and group to ocsinventory-reports directory

#chown -R apache:apache ocsinventory-reports

*changing the owner and group to /var/lib/ocsinventory-reports/ directory

#chown -R apache:apache /var/lib/ocsinventory-reports/

#systemctl restart httpd 

#systemctl restart mariadb

-------------------------------------------
Step 8 - Configure Firewall

#firewall-cmd --zone=public --add-service=http 

#firewall-cmd --zone=public --add-service=https

#firewall-cmd --reload

#firewall-cmd --zone=public --permanent --add-service=http

#firewall-cmd --zone=public --permanent --add-service=https

#firewall-cmd --reload

-------------------------------------------
go to the browser and enter this link

link: http://<@ip_of _the_server>/ocsreports

MySQL login: ocs

MySQL password: ocs 

Name of Database: ocsweb

MySQL HostName: localhost

![6](https://user-images.githubusercontent.com/63731183/121808470-8e31a400-cc50-11eb-844a-020f1f0a1614.JPG)

*for "SECURITY ALERT! (execute the folowing comands)

#cd /usr/share/ocsinventory-reports/ocsreports/

we will ewmove the "install.php" file : 

#mv install.php install.php.bak

#systemctl restart httpd

-------------------------------------------
go to the browser and click on 'Click here to enter OCS-NG GUI

![7](https://user-images.githubusercontent.com/63731183/121808562-f41e2b80-cc50-11eb-8752-2edf8358f41b.JPG)

the default login and password : admin/admin 

![8](https://user-images.githubusercontent.com/63731183/121808636-58d98600-cc51-11eb-94d2-7fc668201209.JPG)
