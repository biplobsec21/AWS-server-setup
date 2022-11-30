#  Amazone AWS server Install Using Apache, MySQL, PHP (LAMP) Stack on Ubuntu 22.04 
### Step 1 — Installing Apache and Updating the Firewall
```
sudo apt update
sudo apt install apache2
sudo ufw app list
sudo ufw allow in "Apache"
sudo ufw status
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

[Reference link 1](https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-ubuntu-22-04)

[Reference link 2](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-22-04)
