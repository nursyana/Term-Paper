# OPERATING SYSTEM - TERM PAPER

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
Docker is

//docker version
![image](https://user-images.githubusercontent.com/106062805/174463192-02a25eef-7741-4968-b921-0ab30ef8dcc0.png)

![image](https://user-images.githubusercontent.com/106062805/174458081-a218b086-75de-4c01-96d9-e5be69054737.png)
 
 * Have a look at the installation steps and procedures here -> [Install Docker](docker-installation.md)


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

//stop container
![image](https://user-images.githubusercontent.com/106062805/174465069-d4e9b84d-b118-41f9-b2a2-c2807c9f31a0.png)

//create composer
![image](https://user-images.githubusercontent.com/106062805/174465181-c3d3f89d-ceef-4d67-9a86-ac25f0b6e931.png)

//create new file for src and put index.txt to src file

//compose
![image](https://user-images.githubusercontent.com/106062805/174465199-05f91bd2-d930-45e0-a124-dc1f37ab3e32.png)

//compose running
![image](https://user-images.githubusercontent.com/106062805/174465210-90d4c8e2-b4af-467c-a7bc-cfae3b3e0695.png)





 




 ### phpMyAdmin

![image](https://user-images.githubusercontent.com/106062805/174479215-92fb4d86-c572-42e1-8740-99b02aff8f4a.png)

![image](https://user-images.githubusercontent.com/106062805/174479226-ab79330c-bf9a-4745-8ca2-c3659c8430e2.png)

![image](https://user-images.githubusercontent.com/106062805/174479234-60d75e2e-7b2c-4363-8cff-c3e41296fad5.png)


![image](https://user-images.githubusercontent.com/106062805/174479331-455139e7-6c4d-4587-9b34-71ec538d9156.png)

//connected phpadmin
![image](https://user-images.githubusercontent.com/106062805/174479509-2a132e4f-223d-4faa-a0d5-3308b75613d6.png)

![image](https://user-images.githubusercontent.com/106062805/174479521-9f6ea51a-3bec-4101-8fba-8238e8c83b1b.png)


//all images in docker
![image](https://user-images.githubusercontent.com/106062805/174481392-a4e92d30-1621-458a-9c16-6ca3617f724e.png)



