Project 2
INSTALLING NGINX WEB SERVER
run
      sudo apt update
       sudo apt install nginx
	sudo systemctl status
	 curl http://localhost:80
Open web browser to access 
         http://3.137.143.88:80
INSTALLING MYSQL
run
      sudo apt install mysql-server
       sudo mysql_secure_installation
        sudo mysql -p
INSTALLING PHP
run
      sudo apt install php-fpm php-mysql
CONFIGURING NGINX TO USE PHP
run
      sudo mkdir /var/www/projectLEMP
       sudo chown -R $USER:$USER /var/www/projectLEMP
        sudo nano /etc/nginx/sites-available/projectLEMP then pasted 
#/etc/nginx/sites-available/projectLEMP

server {
    listen 80;
    server_name projectLEMP www.projectLEMP;
    root /var/www/projectLEMP;

    index index.html index.htm index.php;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
            sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/
	     sudo nginx -t
	      sudo unlink /etc/nginx/sites-enabled/default (disable default Nginx)
	       sudo systemctl reload nginx
		sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
TESTING PHP WITH NGINX
run
      sudo nano /var/www/projectLEMP/info.php (ran into some error because i didn’t disable apache2 before running nginx so i ran sudo sudo systemctl stop apache2 and sudo systemctl disable apache2 before restart nginx)
		sudo rm /var/www/your_domain/info.php
RETRIEVING DATA FROM MYSQLM WITH PHP
run
      sudo mysql
mysql> CREATE DATABASE bodakolade;
mysql> CREATE DATABASE `example_database`;
mysql> CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY 'password';
mysql> GRANT ALL ON example_database.* TO 'example_user'@'%';
mysql>exit
  			mysql -u example_user -p
mysql> SHOW DATABASES;
mysql> CREATE TABLE example_database.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
mysql> INSERT INTO example_database.todo_list (content) VALUES ("My first important item");
mysql> SELECT * FROM example_database.todo_list; > exit
run
      nano /var/www/projectLEMP/todo_list.php then pasted <?php
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

PROJECT 2 ACCOMPLISHED
