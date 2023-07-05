
# WEB STACK IMPLEMENTATION (LEMP STACK)

This document highlights the step-by-step guide to implementing a LEMP web stack in AWS.

### STEP 0: CONNECT TO AWS EC2 INSTANCE 

<img width="389" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/55630c37-276d-4cc4-9e44-c2f4bd5473f5">
<img width="345" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/ba22c91f-6fac-407a-b6d6-4057d1b451a4">

<img width="366" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/f776dc9c-a73a-44f1-a165-328d4cd5ccf1">
<img width="354" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/7e38abfc-9c93-444d-9773-5bfebad2571c">


### STEP 1 – INSTALLING THE NGINX WEB SERVER

Since this is our first time using apt for this session, start off by updating your server’s package index using:

`sudo apt update`

After that is done, install the nginx web server using the code below:

`sudo apt install nginx`

<img width="564" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/98561743-4ba7-49e1-aebb-927044ba15e3">

To verify installation success, 

`sudo systemctl status nginx`

<img width="564" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/f536c772-1da4-4617-a3e7-d8c4a4d46036">

---
Because the TCP port 80 via HTTP has been opened through the editing of the inbound rule on the security group of the instance, we can now access the page via the public IP as seen below:

<img width="915" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/e97a3137-9c8c-4f46-82bc-4c4454a7f6f5">

---

### STEP 2 — INSTALLING MYSQL

Again, use ‘apt’ to acquire and install this software:

`sudo apt install mysql-server`

<img width="563" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/3082c675-4922-4fe5-932b-1cbca697eccb">

Logged in to MySQL console by typing:

`sudo mysql`

<img width="563" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/e67f55cb-730e-45ed-8500-d6af219be778">

Start interactive scripting to enable security recommendations on MySQL to secure account:

`sudo mysql_secure_installation`

<img width="554" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/583fe7df-03fd-4f39-8820-fdf91533115e">

---

After enabling security recommendations, you would be required to access mysql with a password going forward.

`sudo mysql -p`

<img width="565" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/0b4e6e31-1678-4198-8ab5-3a7ab70e20b9">


### STEP 3 — INSTALLING PHP

Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites, but it requires additional configuration. 
We’ll need to install php-fpm, which stands for “PHP fastCGI process manager”, and tell Nginx to pass PHP requests to this software for processing. Additionally, we’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. 
Core PHP packages will automatically be installed as dependencies.

To install these 2 packages at once, run:

`sudo apt install php-fpm php-mysql`

### STEP 4 — CONFIGURING NGINX TO USE PHP PROCESSOR

Created a root web directory for projectLEMP as follows:

`sudo mkdir /var/www/projectLEMP`

Assigning ownership of the directory with the $USER environment variable, which will reference my current system user

`sudo chown -R $USER:$USER /var/www/projectLEMP`

Opened a new configuration file in Nginx’s sites-available directory using your preferred command-line editor. Here, we’ll use nano:

`sudo nano /etc/nginx/sites-available/projectLEMP`

This will create a new blank file. Pasted in the following bare-bones configuration:

```
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
```

When you’re done editing, I saved and closed the file. Using nano, you can do so by typing CTRL+X and then y and ENTER to confirm.

Activated my configuration by linking to the config file from Nginx’s sites-enabled directory:

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

This will tell Nginx to use the configuration next time it is reloaded. You can test your configuration for syntax errors by typing:

`sudo nginx -t`

See the following message below:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

<img width="564" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/2db7a3c4-172c-4a19-b13c-d7d1f456542c">

We also need to disable default Nginx host that is currently configured to listen on port 80, for this run:

`sudo unlink /etc/nginx/sites-enabled/default`

Reloaded Nginx to apply the changes:

`sudo systemctl reload nginx`

Created an index.html file in that location so that we can test that your new server block works as expected:

```
sudo echo 'Hello LEMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname)
'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html
```

<img width="797" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/b1ca2618-e00c-414f-b985-ba7502028c21">

### STEP 5 – TESTING PHP WITH NGINX

Testing to validate that Nginx can correctly hand .php files off to my PHP processor.

Creating a test PHP file in my document root. 

Opened a new file called info.php within my document root:

`sudo nano /var/www/projectLEMP/info.php`

Type or paste the following lines into the new file. This is valid PHP code that will return information about your server:

```
<?php
phpinfo();
```
Saved with CTRl+X, Y, ENTER

I can now access this page in your web browser by visiting the domain name or public IP address you’ve set up in your Nginx configuration file, followed by /info.php:

```
http://`server_domain_or_IP`/info.php
```

A web page containing detailed information about my server:

<img width="500" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/93334d6c-abb1-4fba-9233-4bf93d440ae5">


After checking the relevant information about your PHP server through that page, it’s best to remove the file you created as it contains sensitive information about your PHP environment and your Ubuntu server. You can use rm to remove that file:

`sudo rm /var/www/projectLEMP/info.php`

You can always regenerate this file if you need it later.


### STEP 6 – RETRIEVING DATA FROM MYSQL DATABASE WITH PHP (CONTINUED)

We will create a database named example_database and a user named example_user.

First, connecting to the MySQL console using the root account:

`sudo mysql -p`

To create a new database, run the following command from your MySQL console:

```
CREATE DATABASE `projecttwo_db`;
```
<img width="556" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/87577690-8a59-4391-9260-c5e5dd0df66a">

Create a new user and grant user all privilges to the projecttwo_db database:

`CREATE USER 'power_user'@'%' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';`

`GRANT ALL ON projecttwo_db.* TO 'power_user'@'%';`

Now exit the MySQL shell with:

`mysql> exit`

<img width="573" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/a9457f07-dcd7-4dac-9485-c3359fb39ac1">
<img width="564" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/b1a60882-58dd-40b4-aacd-db3e27622598">



Test if the new user has the proper permissions by logging in to the MySQL console again, this time using the custom user credentials:

`mysql -u example_user -p`

<img width="555" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/9b67b782-b4f7-45bb-9bf1-91ef8fa7f27b">

After logging in to the MySQL console, confirm that you have access to the projecttwo_db database:

`SHOW DATABASES`

<img width="561" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/038d882f-a1c6-436c-8c50-aa5edf0a890d">

Creating a test table named **todo_list** and insert few records into the table. From the MySQL console, run the following statements:

```
CREATE TABLE projecttwo_db.todo_list (item_id INT AUTO_INCREMENT,content VARCHAR(255),PRIMARY KEY(item_id));
```
`INSERT INTO projecttwo_db.todo_list (content) VALUES ("My first important item");`

`INSERT INTO projecttwo_db.todo_list (content) VALUES ("My second important item");`

To confirm that the data was successfully saved to your table, run:

`SELECT * FROM example_database.todo_list;`

<img width="568" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/c765a3d9-734c-429f-8d1f-9b566a06de31">
<img width="568" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/45df287f-1c9b-464c-a5be-f48b5c2b5bff">


After confirming that there is valid data in your test table, you can exit the MySQL console:

`mysql> exit`

Creating a new PHP file in your custom web root directory using nano.

`sudo nano /var/www/projectLEMP/todo_list.php`

The following PHP script connects to the MySQL database and queries for the content of the todo_list table, displays the results in a list. If there is a problem with the database connection, it will throw an exception.

Copy this content into your todo_list.php script:

```
<?php
$user = "power_user";
$password = "PassWord.1";
$database = "projecttwo_db";
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

Saving and closing the file after edit. (CTRL+X > Y > ENTER)

I can now access this page in your web browser by visiting the domain name or public IP address configured for your website, followed by /todo_list.php:

<img width="801" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/a347e369-1563-4f74-85d4-705c3a80c1d5">

---

That means my PHP environment is ready to connect and interact with your MySQL server.


