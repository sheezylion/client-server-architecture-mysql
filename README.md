# Implementing the client-server architecture with MYSQL

![images](https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/c510cb6e-eae3-4cc4-a3fc-98757a3c12ac)

Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.

In their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".

In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL), and the communication between them happens 
over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).

## Implementing a Client Server Architecture using MySQL Database Management System (DBMS).
One of the most widely used server-side technologies for data storage is MySQL, an open-source relational database management system. 
In this project, I will give a step-by-step guide on how to implement client-server architecture with MySQL.

## Prerequisites
Create and configure two Linux-based virtual servers (EC2 instances in AWS).

### How to create and configure two Linux-based virtual servers (EC2 instances in AWS)
The two Linux-based virtual servers we are creating are:
- mysql server
- mysql client

Step 1: 

Login into your AWS Account and open EC2-Dashboard

Note: Be sure to select the AWS Region that you want to launch the instance in, for me it is (N. Virginia) us-east-1.

Now, when you are in the EC2 Dashboard you see there is 0 running instances, for creating a new instance click on “Launch Instance”

Step 2:

Type name of your desirable instance, since we are creating 2 servers we would name it mysql server

<img width="794" alt="Screenshot 2024-05-20 at 17 59 49" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/c77d97d0-7c8c-4da9-bfe2-d61c96292ccc">

Step 3:

Choose AMI

Here you see multiple AMI options for selection. 
AMI stands for Amazon Machine Image, which is basically the software and the operating system that you want to launch on the server, 
in my case I chose Ubuntu 24.04.

<img width="782" alt="Screenshot 2024-05-20 at 18 01 22" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/158369ff-f302-4b71-a701-759239095be1">

Step 4:

Select instance type 

Here you can select the type of machine, number of vCPUs and memory that you want to have. 
Select the free tier eligible option and move to next step (create new key pair)

<img width="802" alt="Screenshot 2024-05-20 at 18 03 12" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/5d12cb47-f4a8-4144-b7c0-ea1a624be83f">

Step 5:

Create new key pair 

This key pair will allow you to access using SSH into the machine you just created. 
Create the key pair and download it, make sure to keep it safe as you will not be able to download it again. In our case we have created a key-pair
earlier.

<img width="804" alt="Screenshot 2024-05-20 at 18 04 24" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/f32e2302-93af-4269-9ce6-de1c373c2544">

Step 6:

Configure network setting

Here you change vpc, subnet and security group and other configuration. I will leave this to default.


<img width="807" alt="Screenshot 2024-05-20 at 18 05 38" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/a5138533-4575-4a60-aef2-511aed45624c">

Step 7:

Configure storage

Select the amount of storage you want your instance to have, for free tier you can choose up to 30 GB of EBS. 
I’ll leave this to default and Click on “Launch Instance”

<img width="805" alt="Screenshot 2024-05-20 at 18 06 28" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/82f68408-c117-4edc-855d-3cea6db023ce">


Step 8:

In the summary section change the number of instances from 1 to 2 since we are creating 2 servers. Double check and launch instance


<img width="797" alt="Screenshot 2024-05-20 at 18 07 12" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/5a16e4da-5285-4c9e-ab46-f12f010796b0">

Instance launch successful, click on the instance link to access your instance

<img width="935" alt="Screenshot 2024-05-20 at 18 09 04" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/3890d2d1-3d8e-4a69-b8b8-d122f015dc05">


Step 9:

Access your instance 

Firstly, rename the second instance from mysql server to msql client. Now we have our 2-servers- mysql server and mysql client running

<img width="1366" alt="Screenshot 2024-05-20 at 18 12 26" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/47d7b664-15df-4a29-b66e-66619e3f9d55">

mysql server

<img width="1397" alt="Screenshot 2024-05-20 at 18 13 55" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/a9f7fe66-380e-46f2-bf0e-3aa453e6e33c">

mysql client

<img width="1422" alt="Screenshot 2024-05-20 at 18 14 51" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/8585c2ba-4ae1-46c2-9f84-8f13724da58f">



### On mysql server do the following

1. open your  terminal and use the below command to access the mysql server.

Where username = ubuntu and public ip address = 52.91.158.125 of mysql server

```
ssh -i ~/Downloads/demo-pair.pem ubuntu@52.91.158.125
```

<img width="700" alt="Screenshot 2024-05-20 at 18 18 56" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/59942563-fe9d-4376-9b8a-0712f37244c4">

2. Update and upgrade Ubuntu

```
sudo apt update && sudo apt upgrade -y
```

<img width="958" alt="Screenshot 2024-05-20 at 18 21 50" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/979bdc9c-eaf0-43f7-a55a-efff20be811f">

3. Install MySQL Server software

```
sudo apt install mysql-server -y
```
<img width="1035" alt="Screenshot 2024-05-20 at 18 23 42" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/1c66cf4e-9ca3-413b-9faa-3789a687e1c6">

4. Enable mysql server

```
sudo systemctl enable mysql
```

5. Check the status to ensure it is running

```
sudo systemctl status mysql
```

<img width="933" alt="Screenshot 2024-05-20 at 18 26 29" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/1d95784c-e356-4bed-9419-3cc7e4642517">

### On mysql client do the following

1. Connect to the instance. Where username = ubuntu and public ip address = 34.229.147.174 of mysql client

```
ssh -i ~/Downloads/demo-pair.pem ubuntu@34.229.147.174
```

<img width="665" alt="Screenshot 2024-05-20 at 18 34 13" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/12b6073b-972c-453a-a35e-46d6020b059a">

2. Update and upgrade Ubuntu

```
sudo apt update && sudo apt upgrade -y
```

<img width="1188" alt="Screenshot 2024-05-20 at 18 35 42" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/74b7573d-75ee-4400-aea7-cb882f674d4d">

3. Install MySQL Client software

```
sudo apt install mysql-client -y
```

<img width="937" alt="Screenshot 2024-05-20 at 18 37 00" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/8620fe10-0b70-4a9c-abc5-19921db31e26">


### Connect to the MYSQL server from mysql client-server 

Both EC2 instances were launched in the same VPC, so by default they can communicate with each other using their local IP addresses. 
We will be using the MySQL server's local IP address (private IP) to connect from the MySQL client. MySQL server uses TCP port 3306 by default, 
so you will have to open it by creating a new entry in ‘Inbound Rules in ‘MySQL Server Security Groups. For extra security, 
do not allow all IP addresses to reach your ‘MySQL server—allow access only to the specific local IP address of your ‘MySQL client’.

<img width="1344" alt="Screenshot 2024-05-20 at 18 44 04" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/ea614a5a-5aeb-4587-81f9-234fddc58fd8">

### Configure MySQL server to allow connections from remote hosts.

For fresh installations of MySQL, you’ll want to run the DBMS’s included security script. 
This script modifies some of the less secure default settings, such as remote root logins and sample users. 
This script attempt to set a password for the installation’s root MySQL account, but, by default on Ubuntu installations, 
this account is not configured to connect using a password. 
To avoid having an error, we will first log in to MySQL and set the root user's password.

```
# First, open up the MySQL prompt:
sudo mysql
# Change the root user’s authentication method to one that uses a password.
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
# exit the MySQL prompt:
exit
```
<img width="738" alt="Screenshot 2024-05-20 at 18 52 43" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/a6abf441-30cf-4aee-8bf8-1d30dad3130c">

1. Run the security script with sudo:

```
sudo mysql_secure_installation
```

The process involves a series of prompts that allow users to make changes. The first prompt is to enter the root user's password, the one set above, to continue.

The second prompt offers the option to set up the Validate Password Plugin, which tests the strength of new MySQL user passwords. Select “yes” and enter “2” which is the strongest password option. The strongest policy requires passwords to be at least eight characters long and include a mix of uppercase, lowercase, numeric, and special characters.

The next prompt will be to set a password for the MySQL root user. Enter “yes” and then confirm a secure password of your choice. The script will then prompt you to continue using the password you just entered or to enter a new one. Assuming you’re OK with the strength of the password you just typed, press Y to proceed with the script.

You can press Y and then ENTER to accept the defaults for all the subsequent questions. This will remove some anonymous users and the test database, disable remote root logins, and load these new rules so that MySQL immediately respects the changes you have made.

<img width="799" alt="Screenshot 2024-05-20 at 18 55 58" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/a4090b14-e5d0-4c0e-9692-355e98eb0be5">

2. Access MySQL shell

```
sudo mysql -p
```

<img width="696" alt="Screenshot 2024-05-20 at 18 57 50" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/4a3d96b3-b8d1-408d-9d0d-43f7223be3ee">

3. On mysql server, create a user named client and a database named test_db.

```
CREATE USER 'client'@'%' IDENTIFIED WITH mysql_native_password BY 'Lion1234#';
```

```
CREATE DATABASE test_db;
```

```
GRANT ALL ON test_db.* TO 'client'@'%' WITH GRANT OPTION;
```

```
FLUSH PRIVILEGES;
```

<img width="657" alt="Screenshot 2024-05-20 at 19 03 12" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/6351b2ff-c080-4495-8bf2-ebf93558bd14">

4. Now, configure MySQL server to allow connections from remote hosts.

You might need to configure MySQL server to allow connections from remote hosts. 
To do this, open the MySQL config file and replace 127.0.0.1’ to ‘0.0.0.0’ in the “binding-address”

```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```


Locate bind-address = 127.0.0.1

Replace 127.0.0.1 with 0.0.0.0

<img width="634" alt="Screenshot 2024-05-20 at 19 05 57" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/23a394da-f662-48b9-b048-fe4504b6dd8f">

5. Restart the MySQL service using

```
sudo systemctl restart mysql
```

### From MySQL client server connects remotely to the MySQL server database engine without using SSH. 

1. You must use the MySQL utility to perform this action.

```
sudo mysql -u client -h 172.31.23.232 -p
```

<img width="637" alt="Screenshot 2024-05-20 at 19 11 48" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/4a24dab9-e8dd-45d8-8e4a-b38e18f945d8">

2. Confirm that you have successfully perform SQL queries from the client server:

```
show databases;
```

<img width="387" alt="Screenshot 2024-05-20 at 19 12 56" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/f64b8849-9891-4b7e-9cc5-a4e356762043">

If you get the above output, it means you have successfully logged into the MySQL server from a client server and performed query successfully.

3. Create table, insert rows into table and select from the table

```
CREATE TABLE test_db.test_table (
  item_id INT AUTO_INCREMENT,
  content VARCHAR(255),
  PRIMARY KEY(item_id)
);

INSERT INTO test_db.test_table (content) VALUES ("My first project is LAMP stack");

INSERT INTO test_db.test_table (content) VALUES ("My second project is LEMP stack");

INSERT INTO test_db.test_table (content) VALUES ("My third project is MERN stack");

INSERT INTO test_db.test_table (content) VALUES ("My forth project is MEAN stack");

INSERT INTO test_db.test_table (content) VALUES ("My fifth project is client-server-architecture stack");

SELECT * FROM test_db.test_table;
```

<img width="916" alt="Screenshot 2024-05-20 at 19 18 41" src="https://github.com/sheezylion/client-server-architecture-mysql/assets/142250556/0bfe0581-94c4-498b-907b-7097d4563556">


## Conclusion
In this project, we have successfully gone through the steps to set up MySQL server on two ubuntu servers (MySQL server and client), 
secure the MySQL instances, and created users, and also configured MySQL to allow remote connection by editing the MySQL config file. 
We also modified the security groups of both instance to allow communication between the the MySQL server and client, 
and we successfully logged into the MySQL server from the client server using the private IP address of the MySQL server.

