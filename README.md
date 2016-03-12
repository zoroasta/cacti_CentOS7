# cacti_CentOS7
install cacti incentos7
# install from EPEL
[root@blank ~]# yum --enablerepo=epel -y install cacti net-snmp net-snmp-utils php-mysql php-snmp rrdtool
#Configure SNMP (Simple Network Management Protocol)
[root@blank ~]# vi /etc/snmp/snmpd.conf
