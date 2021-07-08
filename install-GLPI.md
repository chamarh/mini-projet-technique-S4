step 1 : install Lamp 
*install epel repository: yum install epel-release
*install Lamp: yum -y install httpd php php-mysql php-pdo php-gd php-mbstring php-imap php-ldap mariadb mariadb-server mariadb-devel wget 
*enable and start mariadb and apache 
systemctl start httpd 
systemctl enable httpd 
systemctl start mariadb
systemctl enable mariadb

mysql_secure_installation

-----------------------------------

step 2 : create a database for Glpi 
mysql -uroot -p 
>create database glpidb;
>grant all privileges on glpidb.* to glpiuser@localhost identified by 'glpipwd';
>flush privileges;

-----------------------------------
step 3 : install and config CLPI 
#cd /var/www/html/
#wget https://github.com/glpi-project/glpi/releases/download/9.1.3/glpi-9.1.3.tgz
#tar -xvf glpi-9.1.3.tgz
#chown -R apache:apache glpi 
#chmod -R 755 glpi 
systemctl restart httpd 

lien http://192.168.1.9/glpi 
error : Web access to the files directory, should not be allowed Check the .htaccess file and the web server configuration.
vim /etc/httpd/conf/http.conf
-->AllowOverride all
systemctl restart httpd 

-----------------------------------
http://<@IP_server>/glpi

SQL server (MariaDB or MySQL):localhost
SQL user:glpiuser
SQL password:glpipwd

Default logins / passwords are:

    glpi/glpi for the administrator account
    tech/tech for the technician account
    normal/normal for the normal account
    post-only/postonly for the postonly account

For security reasons, please change the password for the default users: glpi post-only tech normal 
For security reasons, please remove file: install/install.php	 For security reasons, please remove file: install/install.php

-------------------------------------

l’installation du plugin FusionInventory:
glpi version : 9.1.3
ocsinventory version :2.9.0-2

#wget https://github.com/fusioninventory/fusioninventory-for-glpi/releases/download/glpi9.1%2B1.0/fusioninventory-for-glpi_9.1.1.0.tar.gz
#tar -zxvf fusioninventory-for-glpi_9.1.1.0.tar.gz -C /var/www/html/glpi/plugins
#chown -R apache:apache /var/www/html/glpi/plugins
instalation par l'interface graphic via plugins 
#*/1 * * * * /usr/bin/php5 /var/www/html/glpi/front/cron.php &>/dev/null

Configuration > Actions Automatiques > TaskScheduler : executer 

-------------------------------------

installer l’agent Fusion sur les machines du parc(Windows):
>>l’agent Fusion est un agent logiciel qui s’installe sur les postes clients de votre parc informatique.
>>https://github.com/fusioninventory/fusioninventory-agent/releases/download/2.6/fusioninventory-agent_windows-x64_2.6.exe
http://localhost:62354/  >> agent client 

-------------------------------------

Installer le plugin OCS dans GLPI:
wget https://github.com/pluginsGLPI/ocsinventoryng/releases/download/1.3.5/glpi-ocsinventoryng-1.3.5.tar.gz
instalation par l'interface graphic via plugins
go to tools >OCS Inventory NG  pour la configurer 

Invalid OCSNG configuration (TRACE_DELETED must be active) : sur le serveur OCS aller vers configuration>server> TRACE_DELETED 'passer a ON' 

