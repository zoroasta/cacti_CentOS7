# cacti_CentOS7
install cacti incentos7
# Install Apache
yum install httpd httpd-devel
# Install MySQL
yum install mariadb-server -y
# Install PHP
yum install php-mysql php-pear php-common php-gd php-devel php php-mbstring php-cli
# Install PHP-SNMP
yum install php-snmp
# Install NET-SNMP
yum install net-snmp-utils net-snmp-libs
# Install RRDTool
yum install rrdtool
# Staring Apache, MySQL and SNMP Services
systemctl enable httpd.service
systemctl enable mariadb.service
systemctl enable snmpd.service
# Install Cacti
yum install cacti
# Configuring MySQL Server for Cacti Installation
mysqladmin -u root password YOUR-PASSWORD-HERE
# Create MySQL Cacti Database
mysql -u root -p
Enter password:;
            Welcome to the MariaDB monitor.  Commands end with ; or \g.;
            Your MariaDB connection id is 3
            Server version: 5.5.41-MariaDB MariaDB Server
            Copyright (c) 2000, 2014, Oracle, MariaDB Corporation Ab and others.
            Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database cacti;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> GRANT ALL ON cacti.* TO cacti@localhost IDENTIFIED BY 'tecmint';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> FLUSH privileges;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> quit;
Bye

# Install Cacti Tables to MySQL
rpm -ql cacti | grep cacti.sql
Sample Output:/usr/share/doc/cacti-0.8.8b/cacti.sql
#Now we’ve of the location of Cacti.sql file, type the following command to install tables, here you need to type the Cacti user password
mysql -u cacti -p cacti < /usr/share/doc/cacti-0.8.8b/cacti.sql
Enter password:

# Configure MySQL settings for Cacti
vi /etc/cacti/db.php
/* make sure these values reflect your actual database/host/user/password */
$database_type = "mysql";
$database_default = "cacti";
$database_hostname = "localhost";
$database_username = "cacti";
$database_password = "your-password-here";
$database_port = "3306";
$database_ssl = false;

# Configuring Firewall for Cacti
firewall-cmd --permanent --zone=public --add-service=http
firewall-cmd --reload

# Configuring Apache Server for Cacti Installation
vi /etc/httpd/conf.d/cacti.conf
  Alias /cacti    /usr/share/cacti

  <Directory /usr/share/cacti/>
        <IfModule mod_authz_core.c>
                # httpd 2.4
                Require all granted
        </IfModule>
        <IfModule !mod_authz_core.c>
                # httpd 2.2
                Order deny,allow
                Deny from all
                Allow from localhost
            
            
# Finally, restart the Apache service
systemctl restart httpd.service

# Setting Cron for Cacti
vi /etc/cron.d/cacti
Uncomment the following line
    #*/5 * * * *    cacti   /usr/bin/php /usr/share/cacti/poller.php > /dev/null 2>&1



