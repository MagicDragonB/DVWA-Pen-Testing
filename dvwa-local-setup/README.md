# Local Deployment Notes

# The first step is make sure the kali linux have update the lastet version.
sudo apt update

# Go to the Linux website default file road
cd /var/www/html

# get DVWA resource from github
sudo git clone https://github.com/digininja/DVWA.git

# list the file
ls 
DVWA  index.html  index.nginx-debian.html

# Give the DVWA file the highest permission avoid the after operation
sudo chmod -R 777 DVWA

# Go to the config file and copy the config template
cp config.inc.php.dist config.inc.php 

# modify the username and password setting for login into the DVMA website
sudo mousepad config.inc.php

$_DVWA = array();
$_DVWA[ 'db_server' ]   = getenv('DB_SERVER') ?: '127.0.0.1'
$_DVWA[ 'db_database' ] = getenv('DB_DATABASE') ?: 'dvwa';
$_DVWA[ 'db_user' ]     = getenv('DB_USER') ?: 'admin';
$_DVWA[ 'db_password' ] = getenv('DB_PASSWORD') ?: 'password';
$_DVWA[ 'db_port']      = getenv('DB_PORT') ?: '3306';

# start mysql and check the status
sudo systemctl start mysql
sudo systemctl status mysql

mariadb.service - MariaDB 11.4.5 database server
     Loaded: loaded (/usr/lib/systemd/system/mariadb.service; disabled; preset: disabled)
     Active: active (running) since Sat 2025-04-26 05:07:27 EDT; 17s ago
 Invocation: b791bc4e2a0f49bf913f1451de4f727d
       Docs: man:mariadbd(8)
             https://mariadb.com/kb/en/library/systemd/
    Process: 5311 ExecStartPre=/usr/bin/install -m 755 -o mysql -g root -d /var/run/mysqld (code=exited>
    Process: 5313 ExecStartPre=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exite>
    Process: 5315 ExecStartPre=/bin/sh -c [ ! -e /usr/bin/galera_recovery ] && VAR= ||   VAR=`/usr/bin/>
    Process: 5396 ExecStartPost=/bin/sh -c systemctl unset-environment _WSREP_START_POSITION (code=exit>
    Process: 5407 ExecStartPost=/etc/mysql/debian-start (code=exited, status=0/SUCCESS)
   Main PID: 5375 (mariadbd)
     Status: "Taking your SQL requests now..."
      Tasks: 13 (limit: 125377)
     Memory: 322.8M (peak: 413.5M)
        CPU: 7.852s
     CGroup: /system.slice/mariadb.service
             └─5375 /usr/sbin/mariadbd

Apr 26 05:07:27 kali mariadbd[5375]: 2025-04-26  5:07:27 0 [Note] InnoDB: Loading buffer pool(s) from />
Apr 26 05:07:27 kali mariadbd[5375]: 2025-04-26  5:07:27 0 [Note] Plugin 'FEEDBACK' is disabled.
Apr 26 05:07:27 kali mariadbd[5375]: 2025-04-26  5:07:27 0 [Note] Plugin 'wsrep-provider' is disabled.
Apr 26 05:07:27 kali mariadbd[5375]: 2025-04-26  5:07:27 0 [Note] InnoDB: Buffer pool(s) load completed>
Apr 26 05:07:27 kali mariadbd[5375]: 2025-04-26  5:07:27 0 [Note] Server socket created on IP: '127.0.0>

# Use Kali root 
sudo su

# Create a sql table (if you not setting the password just press enter) 
mysql -u root -p 

create database dvwa;

create user 'admin'@'127.0.0.1' identified by 'password';

grant all privileges on dvwa.* to 'admin'@'127.0.0.1';

exit

# start the web server and check the status
systemctl start apache2
systemctl status apache2

apache2.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/apache2.service; disabled; preset: disabled)
     Active: active (running) since Sat 2025-04-26 05:12:47 EDT; 14s ago

# Go the php config file to check 
cd /etc/php
ls ( check the version: mine is 8.4)
cd 8.4
cd apache2
mousepad php.ini (make sure that the below setting is on)
{
allow_url_fopen = On
llow_url_include = On
}

# restart the apache2 server
systemctl restart apache2

# Then open the website put http://127.0.0.1/dvwa, then use admin and password to login, once login 
# click the below botton of the website reconnect the login in again 
