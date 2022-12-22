#  Amazone AWS server Install Using Apache, MySQL, PHP (LAMP) Stack on Ubuntu 22.04 
### Step 1 — Installing Apache and Updating the Firewall
```
sudo apt update
sudo apt install apache2
sudo ufw app list
sudo ufw allow in "Apache"
sudo ufw status
sudo ufw enable
http://your_server_ip
curl http://icanhazip.com
```
### Step 2 — Installing MySQL
```
sudo apt install mysql-server
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password by 'your_password';
sudo mysql
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: NO)
 mysql -u root -p
 your_password
```
### Step 3 — Installing PHP
```
sudo apt install php libapache2-mod-php php-mysql
php -v
```
### Step 4 — Creating a Virtual Host for your Website
```
sudo mkdir /var/www/your_domain
sudo chown -R $USER:$USER /var/www/your_domain
sudo nano /etc/apache2/sites-available/your_domain.conf
sudo a2ensite your_domain
nano /var/www/your_domain/index.html
sudo nano /etc/apache2/mods-enabled/dir.conf
sudo systemctl reload apache2
```
### Step 5 — Testing PHP Processing on your Web Server
```
nano /var/www/your_domain/info.php
```
### Step 6 — Testing Database Connection from PHP (Optional)
```
sudo mysql
CREATE DATABASE example_database;
CREATE USER 'example_user'@'%' IDENTIFIED BY 'password';
GRANT ALL ON example_database.* TO 'example_user'@'%';
exit
mysql -u example_user -p
SHOW DATABASES;
CREATE TABLE example_database.todo_list (
	item_id INT AUTO_INCREMENT,
	content VARCHAR(255),
	PRIMARY KEY(item_id)
);
INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
SELECT * FROM example_database.todo_list;
exit
nano /var/www/your_domain/todo_list.php

```
```php

<?php
$user = "example_user";
$password = "password";
$database = "example_database";
$table = "todo_list";

try {
  $db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);
  echo "<h2>TODO</h2><ol>"; 
  foreach($db->query("SELECT content FROM $table") as $row) {
    echo "<li>" . $row['content'] . "</li>";
  }
  echo "</ol>";
} catch (PDOException $e) {
    print "Error!: " . $e->getMessage() . "<br/>";
    die();
}
```
```
<VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to
        # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName ashura.miraikurukuru.com

        ServerAlias www.ashura.miraikurukuru.com
        ServerAdmin webmaster@ashura.miraikurukuru.com
        #DocumentRoot /var/www/html
        DocumentRoot /var/www/market_inventory/public/
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
RewriteEngine on
RewriteCond %{SERVER_NAME} =www.ashura.miraikurukuru.com [OR]
RewriteCond %{SERVER_NAME} =ashura.miraikurukuru.com
RewriteRule ^ https://%{SERVER_NAME}%{REQUEST_URI} [END,NE,R=permanent]
</VirtualHost>
```
[Reference link 1](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-22-04)

[Reference link 2](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04)

[Important link 3](https://stackoverflow.com/questions/21461499/how-to-recover-ssh-access-to-amazon-ec2-instance-after-ufw-firewall-activation-b)

