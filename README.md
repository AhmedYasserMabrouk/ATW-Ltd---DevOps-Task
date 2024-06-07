# ATW-Ltd---DevOps-Task
## Task #1 Linux Server Simulation:
### 1. Install Apache, MySQL and PHP 

```
sudo apt-get install apache2
sudo systemctl status apache2
sudo apt-get install mysql-server
sudo mysql_secure_installation
sudo apt-get install php libapache2-mod-php

```
### 2. Configure Apache to serve the website from the /var/www/html/ directory.
```
sudo nano /etc/apache2/sites-available/000-default.conf

```
#### Inside the file, you will find a <VirtualHost> block. Replace the DocumentRoot directive with the following line:
```
DocumentRoot /var/www/html/
```
Restart Apache for the changes to take effect by running the following command
```
sudo systemctl restart apache2
```
Create a new file named index.html in the /var/www/html/ directory by running the following command:
```
sudo nano /var/www/html/index.html
```
In the Nano text editor, enter the following line:
```
<html>
<head>
    <title>Hello World!</title>
</head>
<body>
    <h1>Hello World!</h1>
</body>
</html>

```
### 3 Open a web browser and enter http://localhost in the address bar. You should see the "Hello World!" message displayed on the page.


### 4 Configure MySQL to create a new database, user, and password for the website.
Log in to MySQL as the root user by running the following command:
```
sudo mysql -u root -p

```
Once you are logged in to the MySQL shell, create a new database by executing the following command:
```
CREATE DATABASE project_db;

CREATE USER 'ahmed'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON project_db.* TO 'ahmed'@'localhost';
FLUSH PRIVILEGES;

exit
```
### 5. Modify the website to use the newly created database to display a message that includes the visitor's IP address and the current time.
Open the PHP file that generates the website content. Assuming it's the index.php file, run the following command to open it with a text editor:
```
sudo nano /var/www/html/index.php
```
Inside the file, you can add the following PHP code to retrieve the visitor's IP address and current time, and display it on the website:
```
<?php
$ipAddress = $_SERVER['REMOTE_ADDR'];
$currentTime = date('Y-m-d H:i:s');
echo "Hello visitor! Your IP address is $ipAddress. The current time is $currentTime.";
?>

```
### 6. Test the website by accessing it through a web browser and verifying that it displays the expected message.
```
To test the modified website, open a web browser and enter http://localhost/index.php
```
## Task #3 Containerize Your Repository Using Docker Compose:
Create a Dockerfile for the PHP Laravel application. This file specifies the base image, installs any necessary dependencies, and copies the application code into the container. Below is an example of a basic Dockerfile for a PHP Laravel application:
```
# Base image
FROM php:7.4-apache

# Set working directory
WORKDIR /var/www/html

# Install dependencies
RUN apt-get update && apt-get install -y \
    libzip-dev \
    zip \
    && docker-php-ext-install zip

# Copy application code
COPY . .

# Expose port
EXPOSE 80

# Start Apache server
CMD ["apache2-foreground"]

```
Create a Dockerfile for the MySQL server. This file specifies the base image, sets environment variables, and configures any necessary settings. Below is an example of a basic Dockerfile for MySQL:
```
# Base image
FROM mysql:8.0

# Set environment variables
ENV MYSQL_ROOT_PASSWORD=your-root-password
ENV MYSQL_DATABASE=your-database-name
ENV MYSQL_USER=your-username
ENV MYSQL_PASSWORD=your-password

# Copy database initialization script
COPY init.sql /docker-entrypoint-initdb.d/

# Expose port
EXPOSE 3306

# Start MySQL server
CMD ["mysqld"]

```
Create a docker-compose.yml file in the root of your project. This file defines the services, their configurations, and the connections between them. Below is an example of a basic docker-compose.yml file with PHP Laravel and MySQL services:
```
version: '3'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 80:80
    volumes:
      - .:/var/www/html

  db:
    build:
      context: .
      dockerfile: Dockerfile.mysql
    ports:
      - 3306:3306
    volumes:
      - ./data:/var/lib/mysql

```



## Task #4 Networking Basics:
In the context of networking basics and the tasks you mentioned, I'll provide an overview of IP addresses and routing protocols, as well as the steps to connect to a cloud instance from a remote machine using SSH. However, please note that the actual implementation may vary based on your specific cloud provider and network configuration.

1. IP Address and Routing Protocols:
   - IP Address: An IP address is a unique numerical identifier assigned to each device (computer, server, router, etc.) connected to a network. It serves as the address that allows devices to communicate with each other over the Internet or a local network. There are two main types of IP addresses: IPv4 (32-bit) and IPv6 (128-bit).
   - Routing Protocols: Routing protocols are a set of rules and algorithms used by routers to determine the best path for forwarding network traffic between different IP addresses and networks. They enable routers to exchange information and make intelligent routing decisions. Common routing protocols include Border Gateway Protocol (BGP), Open Shortest Path First (OSPF), and Routing Information Protocol (RIP).

2. Connecting to a Cloud Instance from a Remote Machine (SSH Process):
   Here is a general outline of the steps to connect to a cloud instance from a remote machine using SSH:

   - Obtain the necessary credentials: Ensure you have the necessary credentials to access the cloud instance, including the IP address or hostname of the instance, the username, and the private key or password associated with the SSH connection.
   - Install an SSH client: If you don't have an SSH client installed on your local machine, you'll need to install one. OpenSSH is a widely used and recommended SSH client available for various operating systems.
   - Generate or obtain the SSH key pair (optional): If you don't already have an SSH key pair, you can generate one on your local machine using a tool like `ssh-keygen`. Alternatively, you may have been provided with an SSH key pair by your cloud provider.
   - Configure inbound firewall rules (if required): Ensure that the firewall rules in the cloud environment allow incoming SSH connections from your remote machine's IP address. This step may involve configuring security groups, network access control lists (ACLs), or firewall settings specific to your cloud provider.
   - Connect to the cloud instance via SSH:
     - Open a terminal or command prompt on your local machine.
     - Use the `ssh` command to establish an SSH connection to the cloud instance, providing the appropriate parameters:
       ```shell
       ssh -i /path/to/private_key.pem username@instance_ip_address
       ```
       Replace `/path/to/private_key.pem` with the path to your private key file (if using key-based authentication) or provide the password when prompted.
       Replace `username` with the username associated with the cloud instance.
       Replace `instance_ip_address` with the IP address or hostname of the cloud instance.
     - If the connection is successful, you should be logged in to the cloud instance via SSH, and you can execute commands remotely.



