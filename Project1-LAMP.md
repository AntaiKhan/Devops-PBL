
# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

This document shows the steps taken to complete LAMP stack project implementation on AWS.

There are different stack of technologies that make different solutions possible. These stack of technologies are regarded as WEB STACKS. Examples of Web Stacks include LAMP, LEMP, MEAN, and MERN stacks.

* LAMP (Linux, Apache, MySQL, PHP or Python, or Perl)
* LEMP (Linux, Nginx, MySQL, PHP or Python, or Perl)
* MERN (MongoDB, ExpressJS, ReactJS, NodeJS)
* MEAN (MongoDB, ExpressJS, AngularJS, NodeJS)

A technology stack is a set of frameworks and tools used to develop a software product. This set of frameworks and tools are very specifically chosen to work together in creating a well-functioning software. 

**Pre-Requisites for Project Implementation**
* AWS Account (For EC2 Instances)
* Terminal (MacOS/Windows 11) || WSL/VSCode (Windows 10 and below)

### STEP 0 - PREPPING AWS

Connect to AWS EC2 Instance using code below:

`chmod 400 test-antai-projectone.pem`  (for MacOS Users only)

`ssh -i "test-antai-projectone.pem" ubuntu@ec2-16-170-236-100.eu-north-1.compute.amazonaws.com`



<img width="469" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/ca0068fc-b4eb-40bf-b4ee-c6aa66117b28">
<img width="469" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/f5d9ea2e-0ae0-43d1-88a4-39a2b113dcaf">


### STEP 1 — INSTALLING APACHE AND UPDATING THE FIREWALL

Apache is the most widely used web server software. Developed and maintained by Apache Software Foundation, Apache is an open source software available for free. It runs on 67% of all webservers in the world. It is fast, reliable, and secure. 

Update a list of packages in package manager

`sudo apt update`

Run apache2 package installation

`sudo apt install apache2`

After install, verify installation with code below:

`sudo systemctl status apache2`

<img width="469" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/c35d495f-4fab-4dc9-9a46-a84131d7aa13">

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet. This is done on the Security Group of the Security tab of the EC2 instance as seen below:


<img width="912" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/af8a084a-7a67-4fd3-a23a-9219a2cfc3d1">


<img width="912" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/67b568b5-b162-4ba8-afc3-95176b8f6d72">

After trying to access public IP, I get the default page below:

<img width="798" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/e758dbbd-442d-4b52-8f7d-1705b0989c76">


### STEP 2 — INSTALLING MYSQL

Again, we use ‘apt’ to acquire and install this software:

`sudo apt install mysql-server`

After installation, log in to the MySQL console by typing:

`sudo mysql`

`sudo mysql -p`  After password has been configured.

<img width="555" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/6be644da-fabb-44d9-9419-7035dc091d23">


To exit the MySQL console, type:

`mysql> exit`


### STEP 3 — INSTALLING PHP

In addition to the php package, we’ll need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases. We’ll also need libapache2-mod-php to enable Apache to handle PHP files.

To install these 3 packages at once, run:

`sudo apt install php libapache2-mod-php php-mysql`

Once the installation is finished, you can run the following command to confirm your PHP version:

`php -v`

<img width="551" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/a9e6419f-c7f4-495f-a000-d88792f8fcc2">


### STEP 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE
Created the directory for projectlamp using ‘mkdir’ command as follows and assigned ownership of directory to current system user:

`sudo mkdir /var/www/projectlamp`

`sudo chown -R $USER:$USER /var/www/projectlamp`

created and opened a new configuration file in Apache’s sites-available directory:

`sudo vi /etc/apache2/sites-available/projectlamp.conf`

<img width="570" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/dec0bdc9-75e1-44c6-a869-ec55c3ddd76a">


`sudo ls /etc/apache2/sites-available`

Created configuration file can be seen in the **sites-available** directory:

<img width="494" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/4fca6751-74e6-414c-b733-35d4e14bbf85">

You can now use **a2ensite** command to enable the new virtual host:

`sudo a2ensite projectlamp`

You might want to disable the default website that comes installed with Apache. This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. To disable Apache’s default website I used the **a2dissite** command below:

`sudo a2dissite 000-default`

To make sure your configuration file doesn’t contain syntax errors, I ran:

`sudo apache2ctl configtest`

<img width="413" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/56200de3-3bb4-4fca-98e6-e66888d09219">

Finally, I reloaded Apache so these changes take effect using:

`sudo systemctl reload apache2`

Created an index.html file in that location so that we can test that the virtual host works as expected:

`sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html`

<img width="803" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/8cdf9f80-5869-45b5-8896-9b3bb2b68936">

### STEP 5 — ENABLE PHP ON THE WEBSITE

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file.

I want to change this behavior, I edited the /etc/apache2/mods-enabled/dir.conf file and changed the order in which the index.php file is listed within the DirectoryIndex directive using the command below:

`sudo vim /etc/apache2/mods-enabled/dir.conf`

<img width="566" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/936df242-7524-42c7-a3c4-a3024a404023">

Saved and closed the file, I reloaded Apache so the changes take effect:

`sudo systemctl reload apache2`

Created a new file named index.php inside your custom web root folder:

`vim /var/www/projectlamp/index.php`

This opened a blank file. Added the following text, which is valid PHP code, inside the file:

`<?php 
phpinfo();`


<img width="365" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/f05dfd30-06dc-4290-8811-e8dbb5ba5e9f">

<img width="574" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/3bc26401-bb5a-4c6c-bf76-b06f7c5a0c77">

Removed the file I created as it contains sensitive information about your PHP environment -and your Ubuntu server.

`sudo rm /var/www/projectlamp/index.php`


<img width="799" alt="image" src="https://github.com/AntaiKhan/Devops-PBL/assets/25984697/29072c89-c489-44a2-a48e-d1c9da2c0dff">

Done deploying a LAMP stack website in AWS Cloud!


