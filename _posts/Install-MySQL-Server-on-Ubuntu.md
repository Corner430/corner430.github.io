---
title: Install MySQL Server on Ubuntu
date: 2023-04-03 18:57:39
tags:
    - MySQL
    - Ubuntu
declare: true
---
##### 1. Install MySQL Server on Ubuntu
```shell
sudo apt-get update
sudo apt-get install mysql-server
```
During the installation, you will be prompted to set a password for the root user. Note that this password is very important, please remember it.<!--more-->

##### 2. Run MySQL. After the installation is complete, the MySQL or MariaDB server will start automatically. You can check its status with the following command:
`sudo systemctl status mysql`

##### 3. Connect to the MySQL server
`mysql -u root -p`
This will open a MySQL or MariaDB shell and prompt you for your password. Enter the password you set during installation and press "Enter".

##### 4. Create a database: To create a database, use the following command in the MySQL shell:
`CREATE DATABASE database_name;`
where "database_name" is the name of the database you want to create. You can use any name as long as it is not already used.

##### 5. Create a user and grant permissions: To create a user and grant permissions, use the following command in the MySQL shell:
```sql
CREATE USER 'username'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON database_name.* TO 'username'@'localhost';
FLUSH PRIVILEGES;
```

Where "username" is the username you want to create, "password" is the user's password, and "database_name" is the name of the database you created in step 4. These commands will grant that user all privileges on the database and associate their password with their username.

##### 6. Test Connection: To test your connection, exit the MySQL shell and reconnect using the following command
`mysql -u username -p -h localhost database_name`

where "username" is the username you created in step 5 and "database_name" is the database name you created in step 4. After entering your user password, you should be able to see the MySQL or MariaDB shell and start using SQL commands.
