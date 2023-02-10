# Laravel-Setup-Using-Docker
## Prerequisites
#### Before you start, you will need;
#### 1. A running ubuntu operating system download or you can use a cloud provider 
#### 2. You must have docker installed, etc

## Step1 - Download laravel and it's dependencies
#### The first step is to get the laravel clone to our home directory or if you can create a directory and clone laravel app to the directory. The Github repo comes with a composer file, an application-level dependey manager for PHP. Since we are trying to used all the dependencies as a docker container. let fire up our terminal...
#### **cd to home directory or you can cd to your project directory.**
#### **git clone https://github.com/laravel/laravel.git laravel-app**. After it is done cloning, move into your laravel-app directory and use Docker’s composer image to mount the directories that you will need for your Laravel project and avoid the overhead of installing Composer globally with the below command:
       docker run --rm -v $(pwd):/app composer install
#### The -v and -rm create an an ephemeral container that will be bind-mounted to your current directory before being removed. And copy the content of you laravel-app directory to the container and also make sure that the vendor folder Composer creates inside the container is copied to your current directory. 
#### The next step is to set a permissions on the laravel-app project directory so that it is owned by your non-root user
       sudo chown -R $USER:$USER ~/laravel-app
#### This will be very important when you are writing a dockerfile for your application image, as it will allow your application container to run as non-root user.

## Step - 2 Let create dockerfile for our laravel-app application
#### Dockerfile includes instructions that Docker can use to build custom Docker images. It can also install the software required and configure the necessary settings for your application. They specify the environment inside a container that will host your application code. You may push the images you create to docker hub for sharing or place them on other private registries.

#### We will create a Dockerfile that will specify the instructions to build the Laravel application image. Use nano to create the Dockerfile in ~/laravel-web directory:
###        Please checkout the dockerfile in this repo and used it
## What is going in the dockerfile??
#### First, the Dockerfile creates an image on top of the php:7.2-fpm Docker image. This is a Debian-based image that has the PHP FastCGI implementation PHP-FPM installed. The file also installs the prerequisite packages for Laravel: mcrypt, pdo_mysql, mbstring, and imagick with composer.

#### The RUN directive specifies the commands to update, install, and configure settings inside the container, including creating a dedicated user and group called www. The WORKDIR instruction specifies the /var/www directory as the working directory for the application.

#### Creating a dedicated user and group with restricted permissions mitigates the inherent vulnerability when running Docker containers, which run by default as root. Instead of running this container as root, we’ve created the www user, who has read/write access to the /var/www folder thanks to the COPY instruction that we are using with the --chown flag to copy the application folder’s permissions.

#### Finally, the EXPOSE command exposes a port in the container, 9000, for the php-fpm server. CMD specifies the command that should run once the container is created. Here, CMD specifies "php-fpm", which will start the server.

#### Save the file and exit your editor when you are finished making changes.
