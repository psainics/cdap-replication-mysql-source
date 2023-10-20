# MySql setup for replication job on ubuntu vm
> This setup was performed on `Ubuntu 20.04.6 LTS x86_64`

## Step 1 ) Update the package index
```bash
sudo apt update
```
## Step 2 ) Install `mysql-server` package
```bash
sudo apt install mysql-server
```
## Step 3 )  Start the `mysql.service`
```bash
sudo systemctl start mysql.service
```
## Step 4 )  Secure the instance
- Yes for all the options for best security
```bash
sudo mysql_secure_installation
```
## Step 5 )  Root Login
- You should be able to login with the root user
```bash
sudo mysql
```
## Step 6 ) Create new user
- You may change the username `cdfuser`
- You may change the password `cdfpasswd`
```mysql
CREATE USER 'cdfuser'@'%' IDENTIFIED WITH BY 'cdfpasswd'; 
```
## Step 7 ) Grant the `REPLICATION SLAVE` and `REPLICATION CLIENT` permissions to the user
- Replication permissions is required to read the binlogs
- Some installation may turn off binlogs verify with `SHOW MASTER STATUS`
	- Not required for ubuntu installation
```mysql
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'cdfuser'@'%';
```
## Step 8 ) Grant the `RELOAD`  privileges to the user
```mysql
GRANT RELOAD ON *.* TO 'cdfuser'@'%';
```
## Step 9 ) Make a new db and grant all privileges to user
- You may change the database name `cdftest`
```mysql
CREATE DATABASE cdftest;
GRANT ALL PRIVILEGES ON cdftest.* TO 'cdfuser'@'%';
```
## Step 10 ) Flush Privileges and exit
- After granting the privileges, it's a good practice to flush the privileges to ensure that the changes take effect immediately
```mysql
FLUSH PRIVILEGES;
```
## Step 11 ) Login as new user and create table 
```bash
mysql -u cdfuser -p
```

> Note: By default the server only binds to 127.0.0.1 
> If you wish to expose it to public you will have to update the `bind-address` to `0.0.0.0`
