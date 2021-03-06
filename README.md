# A TUTORIAL TO DEPLOY A COMPLETE APACHE, PHP, MYSQL AND PHPMYADMIN USING DOCKER

Prepared by:
1. Alfin Najeehah binti Zahid (2019618)
2. Nur Syazana binti Abdul Kadir (2016730)
3. Nurul Zahidah binti Jamaludin (2011130)

## What we are going to cover?
   * [What is Docker?](##What-is-Docker?)
     * [Docker Installation](installation.md)
   * [Steps to deploy:](##Steps-to-deploy-PHP,-Apache,-MySQL-and-phpMyAdmin)
     * [PHP](###PHP)
     * [Apache](###Apache)
     * [MySQL](###MySQL)
     * [phpMyAdmin](###phpMyAdmin)

## What is Docker?
Docker is an open platform for developing, shipping, and running applications. Docker allows us to separate our applications from the infrastructure so we can quickly deploy software. With Docker, we can manage the infrastructure the same way we manage the applications. By using Docker methods to quickly ship, test, and deploy code, we can significantly reduce the time between writing the code and running it in production.

Docker provides the ability to package and run an application in a loosely isolated environment called a container. Isolation and security allow us to run many containers concurrently on a given host. Containers are lightweight and contain everything needed to run the application, so we don't have to rely on what's currently installed on the host. We can easily share containers as we work and make sure everyone we share with gets the same container that works the same way.

Docker provides tools and a platform to manage the lifecycle of the containers: 
 
- Develop the application and its supporting components using containers. 
- The container becomes the unit for distributing and testing the application. 
-  When we are ready, deploy our app to production environment either as a container or as an orchestrated service. This works whether our production environment is an on-premises data center, a cloud provider, or a mix of both.

![image](https://user-images.githubusercontent.com/106062805/174482239-ac7c6f45-1ac1-4aa3-bfa1-0e9df2fa4691.png)

 
 * Have a look at the installation steps and procedures here -> [Install Docker](https://github.com/nursyana/Term-Paper/blob/main/installation.md)


## Steps to deploy PHP, Apache, MySQL and PHPMyAdmin

 ### PHP
PHP  (recursive acronym for PHP: Hypertext Preprocessor) is a web programming platform and is a widely-used open source general-purpose scripting language. It can be embedded into HTML. Let us jump in into the steps to setup PHP using Docker.

1. Open [Docker Hub](https://hub.docker.com/) and search for PHP image. Choose the official one.
2. You need to have any source code editor, mine is using Visual Code Editor ([VS Code](https://code.visualstudio.com/download)). Have a folder in the VS Code for your project.
3. Now, let's create a Dockerfile in the project.  
  ![image](https://user-images.githubusercontent.com/93193178/174470445-dcf11155-785d-48fe-93e7-95d75f189d46.png)   
4. Pull the PHP image by paste this code in the Dockerfile.
   ```
      FROM php:7.4-cli
      COPY . /usr/src/myapp
      WORKDIR /usr/src/myapp
      CMD [ "php", "./index.php" ]
   ```
   **COPY** is going to copy all the scripts or files from the current local storage into our WORKDIR and have it run.  
   **WORKDIR** telling us working directory where our container is going to be stored.  
   You can change ```"./index.php``` to any file name for the default of the page of the website.   
   
   ![image](https://user-images.githubusercontent.com/106062805/174464219-1a89ef9a-e747-4470-a275-9fd85f331ccd.png)

5. In the index.php file, just quickly create a php script.

   ![image](https://user-images.githubusercontent.com/106062805/174464232-03ddc426-f1f2-4aa4-a0bc-f81aac24fb67.png)

6. To build the image we're going to run this command in the terminal:
   ```
      docker build -t my-php-app .
   ```
   ```my-php-app``` is the name of the image, so you can rename it.  
   
7. And, to run that image, we need to run another command:
   ```
      docker run -it --rm --name my-running-app my-php-app
   ```
   ```my-running-app``` is going to be the name of the container.  
   
   After you run this command, you can see it execute the script you insert before.  
      
  ![image](https://user-images.githubusercontent.com/106062805/174463722-ddcea9a1-12e8-458b-9848-16ed515ecc61.png)
  
We are done with the PHP. However, most websites are not going to be running on a command prompt or terminal. So, let's think of the means for these running on the browser. Here, we choose to have Apache as the web server to work with.  


 ### Apache
 
 Apache is the most commonly used web server software, and it is open-source and free. It is quick, dependable, and secure. Let's proceed to deploy it.
 
   1. To run the downloaded Apache Docker Image, invoke the run command below to create a new container.
   ```
      docker run -d --name docker-apache -p 8000:80 -d httpd
   ```
   ```docker-apache``` is the name of the container.  
   ```-p 8000:80``` is mapping the local computer's port 8000 to the container's port 80.  
   
![image](https://user-images.githubusercontent.com/106062805/174464047-a9554fa3-4e05-41f8-9b16-cc30095c11af.png)

   Image below shows that the container is running.
![image](https://user-images.githubusercontent.com/106062805/174464310-426a6ad9-2126-40ac-9d52-eeda9e491dd6.png)

   2. When the container is running, we should be able to go to localhost, port 8000 and see the code inside of the container running on the local machine. To make sure, it really running, you can make a change, save and refresh the localhost. It will follow the changes because we have set up ```-v```, a volume from our local machine into the container. 

![image](https://user-images.githubusercontent.com/106062805/174464449-edd9cb8c-ded4-490b-9095-570fb31aaba4.png)

SO, that's how we running our browser. As simple as this 2 steps. Now we are ready to setup docker compose and MySQL.

 ### MySQL
 
MySQL is a relational database management system that is free and open source. It allows us to save all of the posts, users, plugin information, and so on. That information is stored in separate "tables" and linked with "keys," which is why it is relational. It impossible for a site to have no database. So, we choose MySQL for this tutorial. Let follow us.

   1. The container need to be stop first before we proceed. Use the command below to stop it.
   ```
      docker stop $(docker ps -a-q)
   ```
![image](https://user-images.githubusercontent.com/106062805/174465069-d4e9b84d-b118-41f9-b2a2-c2807c9f31a0.png)

   2. In our project, create a file named ```docker-compose.yml```. Paste this code in that file:
   ```
      version: '3.1'
      
      services:
         php:
           image: php: 8.0-apache
         ports:
           - 8000:80
         volumes:
           - ./src:/var/www/html/
   ```
   Basically, we're setting the version of docker compose to ```3.1``` and the services, each one of it represent separate container.  
   So, this container name ```php``` that using and refer to the image we created before ```php: 8.0-apache```.  
   With the ports ```8000:80``` where we are going to make use 8000 local computer mapping to 80 in the container.  
   Then we setting the volume for this code or the source file, ```./src:``` in our local computer and, ```/www/html/``` on the container.  
   So we are going to move our index file to the source folder in the next step.  
   
![image](https://user-images.githubusercontent.com/106062805/174465181-c3d3f89d-ceef-4d67-9a86-ac25f0b6e931.png)

   3. So, create a new folder under the same project named ```src```. Then move the ```index.php``` to that source folder. Now, we have same exact setup but we're just using docker compose.
   
   4. Next, we should be able to run docker compose by using this command:
   ```
      docker-compose up -d
   ```
![image](https://user-images.githubusercontent.com/106062805/174465199-05f91bd2-d930-45e0-a124-dc1f37ab3e32.png)

   We can see the compose running from the image below. Another way to make sure it running it by make some changes to the code and refresh the localhost.  
   
![image](https://user-images.githubusercontent.com/106062805/174465210-90d4c8e2-b4af-467c-a7bc-cfae3b3e0695.png)

Okay! Now docker compose file is working, so we get MySQL image installed.  

  5. Head over to the [Docker Hub](https://hub.docker.com/) and get to the official MySQL image.
  
  6. Scroll down and copy the services or you can simply copy it from here:
  ```
     Mysqldb:
       image: mysql:latest
       command: --default-authentication-plugin=mysql_native_password
       restart: always
       environment:
         MYSQL_ROOT_PASSWORD: example
  ```
  _NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password_  
 _(this is just an example, not intended to be a production configuration)_  
 
  7. Paste the code under the ```services``` in the docker-compose.yml file. ```db``` should be the same indention with the php.  
     Basically, we are referring to ```mysql``` image and setting up some plugin, ```--default-authentication-plugin=mysql_native_password```.  
     Then, we've got the ```environment``` variable where we setting it to ```MYSQL_ROOT_PASSWORD:```. ```example``` is the password.
     
In the [Docker Hub](https://hub.docker.com/) for the code give, there's Adminer but we are not going to use it instead we will show how to connect MySQL with phpMyAdmin.  

 ### phpMyAdmin

phpMyAdmin is a free PHP software utility designed to manage MySQL and offers a wide variety of MySQL operations. 

1. Insert this code to the source file which this will run phpMyAmin and also allowing to specify MySQL server on login page after run the code. 

```
version: '3.1'

services:
  db:
    image: mariadb:10.3
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: notSecureChangeMe

  phpmyadmin:
    image: phpmyadmin
    restart: always
    ports:
      - 8080:80
    environment:
      - PMA_ARBITRARY=1
   ```

![image](https://user-images.githubusercontent.com/106062805/174479215-92fb4d86-c572-42e1-8740-99b02aff8f4a.png)

2. Next, we should be able to run docker compose by using this command:

   ```
      docker-compose up -d
   ```

![image](https://user-images.githubusercontent.com/106062805/174479226-ab79330c-bf9a-4745-8ca2-c3659c8430e2.png)

3. After that, we should be able to see the php login page by enter ```localhost:8001```

![image](https://user-images.githubusercontent.com/106062805/174479234-60d75e2e-7b2c-4363-8cff-c3e41296fad5.png)

4. After login to the phpMyAdmin, we will be able to access all the table related. 

![image](https://user-images.githubusercontent.com/106062805/174479331-455139e7-6c4d-4587-9b34-71ec538d9156.png)

5. To see whether we already connected to the phpMyAdmin or not, we can run those code. 

![image](https://user-images.githubusercontent.com/106062805/174479509-2a132e4f-223d-4faa-a0d5-3308b75613d6.png)

6. Lastly, enter ```localhost:8000``` to get to see the output.

![image](https://user-images.githubusercontent.com/106062805/174479521-9f6ea51a-3bec-4101-8fba-8238e8c83b1b.png)


This tutorial is a modification from [Truth Seekers](https://www.youtube.com/watch?v=ThpnqYpvnIM) and [Ceed Media](https://www.youtube.com/watch?v=PvDoZDWDUzw)
