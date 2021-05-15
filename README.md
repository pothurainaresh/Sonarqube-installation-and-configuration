# Sonarqube-installation-and-configuration

# Prerequisite
The only prerequisite for running SonarQube is to have Java (Oracle JRE 11 or OpenJDK 11) installed on your machine.

# Hardware Requirements

1.A small-scale (individual or small team) instance of the SonarQube server requires at least 2GB of RAM to run efficiently and 1GB of free RAM for the OS. If you are installing an instance for a large teams or Enterprise, please consider the additional recommendations below.
2.The amount of disk space you need will depend on how much code you analyze with SonarQube.
3.SonarQube must be installed on hard drives that have excellent read & write performance. Most importantly, the "data" folder houses the Elasticsearch indices on which a huge amount of I/O will be done when the server is up and running. Great read & write hard drive performance will therefore have a great impact on the overall SonarQube server performance.
4.SonarQube does not support 32-bit systems on the server side. SonarQube does, however, support 32-bit systems on the scanner side.


# Enterprise Hardware Recommendations
For large teams or Enterprise-scale installations of SonarQube, additional hardware is required. At the Enterprise level, monitoring your SonarQube instance is essential and should guide further hardware upgrades as your instance grows. A starting configuration should include at least:

8 cores, to allow the main SonarQube platform to run with multiple Compute Engine workers
16GB of RAM For additional requirements and recommendations relating to database and ElasticSearch, see Hardware Recommendations.

## Installation Steps for Redhat machine
### it Runs on port 9000

### To install java-openjdk11 package
$ sudo amazon-linux-extras install java-openjdk11
### If you wish to change java configuration use below command
$ sudo alternatives --config java
### Now go to /opt folder and download the package
### Now download the package of SonarQube server using below command 
$ wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.6.zip

### Unzip the package by using below command
$ sudo unzip <Name of package>
  
### Now add sonar user details
$ groupadd sonar 
$ useradd -c "Sonar System User" -d /opt/sonar -g sonar -s /bin/bash sonar
$ chown -R sonar:sonar /opt/sonar
$chmod -R 775 /opt/sonar/
$visudo
Add below file in the sudoers file 
sonar  ALL=(ALL)  NOPASSWD:ALL

### To start sonar server. Go to below path and execute the below commands
**To Start**
$./sonar.sh start
**To Check Status**
$./sonar.sh status

### Now go to browser and check the server by using your url
https://<ip address>:9000

### By default the username password will be admin & admin

# To connect your DB Server to your SonarQube Server 

## Follow below steps to configure your SonarQube server with DB server

## I am using postgresql db server in AWS services to connect it

### First you need to create postgresql db server in AWS by providing your **username and password**

### Now you can install postgresql client in EC2 instance by using below command

$ yum install -y postgresql

# Connect with your db server use the below command

$ psql --host=<End point of of your db server> --port=5432 --username=sonar --password --dbname=postgres
  
### Create database called sonardb and grant privileges on database sonardb to sonar user

$ create database sonardb;
$ grant all privileges on database sonardb to sonar;

### To check the databases and users in db run the below commands
$ \l     --------> to list databases in db
$ \du    --------> to list users in db
$ \q     --------> to exit from server

### Now go to sonar installation server

### go to the path cd /opt/sonar/conf and edit the sonar.properties file as like below mentioned bold lines

#### The schema must be created first.
***sonar.jdbc.username=sonar ***    
***sonar.jdbc.password=sonar123***

#----- PostgreSQL 9.3 or greater
#### By default the schema named "public" is used. It can be overridden with the parameter "currentSchema".
***sonar.jdbc.url=jdbc:postgresql:<Db server end point you have to give here>***

#### By default, ports will be used on all IP addresses associated with the server.
***sonar.web.host=0.0.0.0***

#### The default value is root context (empty value).
***sonar.web.context=/sonar***

#### TCP port for incoming HTTP connections. Default value is 9000.
***sonar.web.port=9000***

### Now Restart your SonarQube server 
**To Start**
$./sonar.sh start
**To Check Status**
$./sonar.sh status

### Errors and resloves
if error means:
bootstrap check failure [1] of [1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

sudo sysctl -w vm.max_map_count=262144

grep vm.max_map_count /etc/sysctl.config

vm.max_map_count=262144























