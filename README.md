**Installing Docker:(Skip if it is already installed. To check run `sudo docker -v`)**

Add the GPG key for the official Docker repository to your system:


* ` curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

Add the Docker repository to APT sources:


*  `sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"`

Next, update the package database with the Docker packages from the newly added repo:


*  `sudo apt-get update`

Finally, install Docker:


*  `sudo apt-get install -y docker-ce`

Restart the docker service


*  `systemctl restart docker`

**Installing Docker-compose(Skip if it is already installed. To check run sudo `sudo docker-compose --version`)**


*  ````sudo curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose````

Next we’ll set the permissions to make docker-compose executable:

*  `sudo chmod +x /usr/local/bin/docker-compose`

Then we’ll verify that the installation was successful by checking the version:

*  `docker-compose --version`

Reference URL:

https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-16-04

**Magento Project Setup**

First clone the repository from magento branch in /usr/local/src

*  `git clone -b magento-nginx https://git.ranosys.org/ranosys/docker.git`

Run some commands:

* Switch pwd to docker  `cd /usr/local/src/docker`

*  Create directory for MySQL `mkdir mysql`

*  Create directory for code `mkdir -p web/htdocs`

Now you can edit the .env file which is located in /usr/local/src/docker path. This is a hidden file so you can see this file by run **`ls -ali`** command and open it with vim editor and set the database credential which you want to set. 

Then put your project code under web/htdocs folder and chage the mysql connection string in env.php file.

Here MySQL container name is mysql. You just put the **'host' => 'mysql'** also change the dbanme and password in env.php file. The db name and password is as same as you set in .env file,just one step above. 

Now stop your local apache/nginx and mysql service if you can. Otherwise you can run this docker to different port by chaning the port in docker-compose.yml file. Below are the steps to change the port in docker-compose.yml file.

1. open docker-compose.yml file in any editor like vim, nano.
2. Now for change the Nginx port go to web service section and change the first port. Currently, it is showing like 80:80 if you want to run Nginx on 81 port then change like 81:80. 
3. Now for change the mysql port go to mysql service section and change the first port. Currently it is showing like 3306:3306 if you want to run mysql on 3307 then change like 3307:3306
4. Now for change the PHPMyadmin port go to PHPMyAdmin service section and change the first port. Currently, it is showing like 8080:80 if you want to run phpmyadmin 8081 then change to 8081:80

Now all set. Run the below command where docker-compose.yml file are located. Location /usr/local/src/docker. 

* `sudo  docker-compose up -d`


When the above command succeeded,four container will start. 

1.  Web Container: In this container PHP 7.1, Nginx & php-fpm is installed. 
2.  Mysql: In this contanier MySQL 5.6 is installed. 
3.  PhpMyadmin: PHPMyAdmin is Intsalled
4.  Redis: Redis 6 is installed.


Use the below command to check the above three containers are running or not. 


* `sudo docker ps` 


URL(s)

*  Website URL: http://localhost


*  PHPMyAdmin: http://localhost:8080

Note: You can import the project database from phpmyadmin. If database size is large then follow the below steps to import the database from command line. 

1. Put your database dump file i.e *.sql in /usr/local/src/docker/mysql path.  
2. Then run **`sudo docker ps`** command
3. After run the above command copy the mysql container id. 
4. After copy the container id run the below command to ssh into MySQL container:
  

* `sudo docker exec -it CONTAINERID bash `


5.  Now we are in mysql container. Run the below two commands to import the database:
*  `cd /var/lib/mysql`
*  `mysql -u root -p db_name < db_name.sql`


**Note: Here the db name is which you set in .env file and the sql file name is the same name is which you put in /usr/local/src/docker/mysql**

After run this command password prompt will show then put the mysql root user password which is also set in .env file. 

**Other Useful docker commands:**

* `sudo docker ps` ### To check the running containers. 
* `sudo docker ps -a` ### To check the stopped containers. 
* `sudo docker-compose down `### To stop all the running container but this will run only that path where docker-compose file are located in our case path is /usr/local/src/docker.
* `sudo docker images` ## To check the docker images
* `sudo docker exec -it CONTAINERID bash `### To ssh into container

**How to SSH into container:**

1.  Run **`sudo docker ps`** command and copy the web container id
2.  Run **`sudo docker exec -it CONTAINERID bash`** to ssh into web container.
  
**How to check nginx logs in container**
1. Do ssh into web container.
2.  `cd /var/log/nginx`
3.  refer error.log file for Nginx and PHP logs.
4.  `tail -f error.log` ### For check the run time logs
5.  `tail -500 error.log` ### For check last 500 logs
  

**How to run magento commands:**
1.  Do ssh into web container.
2.  Ensure you are in /var/www/html 
3.  RUN the magento commands which you want to run.
3.  Then run exit to close the current contaner session.

**How to check magento logs:**

1.  In host OS find the logs in this /usr/local/src/docker/web/htdocs/var/logs location. 


**Note: If you want to change the PHP version then open the docker-compose file and change the web image name. Please use the below name for PHP7.2**


*  ranosys/magento_nginx:7.2

*  ranosys/magento_nginx:7.3

*  ranosys/magento_nginx:7.0 


If you want php with ioncube then please user below:

*  ranosys/magento_nginx:7.2_withioncube
*  ranosys/magento_nginx:7.1_withioncube


After changes the name again UP the docker-compose.
