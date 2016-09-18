# Pull base image (Ubuntu Server 16.04)
FROM ubuntu:xenial

# Install php 7
RUN apt-get update &&\
	apt-get -y install apache2 php libapache2-mod-php curl &&\
	apt-get clean && rm -rf /var/lib/apt/lists/*

# Replace Apache default page by phpinfo
RUN echo "<?php echo phpinfo();" >> /var/www/html/index.php && rm /var/www/html/index.html && chown -R www-data:www-data /var/www

# Setup PHP timezone
#RUN echo date.timezone=Europe/Paris >> /etc/php5/apache2/conf.d/01-timezone.ini

# Install Composer
RUN cd $HOME &&\
    curl -sS https://getcomposer.org/installer | php &&\
    chmod +x composer.phar &&\
    mv composer.phar /usr/local/bin/composer

# Enable SSL and setup a testing SSL virtualhost
RUN ls -l /var/lock
ENV APACHE_RUN_USER=www-data \ 
	APACHE_RUN_GROUP=www-data \ 
	APACHE_LOG_DIR=/var/log/apache2 \ 
	APACHE_PID_FILE=/var/run/apache2.pid \ 
	APACHE_RUN_DIR=/var/run/apache2 \ 
	APACHE_LOCK_DIR=/var/lock \ 
	APACHE_SERVERADMIN=admin@localhost \ 
	APACHE_SERVERNAME=localhost \ 
	APACHE_SERVERALIAS=docker.localhost \ 
	APACHE_DOCUMENTROOT=/var/www
RUN cp /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/000-default-ssl.conf
RUN a2enmod ssl &&\
    a2ensite 000-default-ssl

WORKDIR /var/www

# Expose http and https ports
EXPOSE 80 443

# Start Apache
CMD ["/usr/sbin/apache2", "-D",  "FOREGROUND"]
