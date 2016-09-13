# Pull base image
FROM ubuntu:xenial

# Install php 7
RUN apt-get update &&\
	apt-get -y install apache2 php &&\
	apt-get clean && rm -rf /var/lib/apt/lists/*

# Replace Apache default page by phpinfo
RUN echo "<?php echo phpinfo();" >> /var/www/html/index.php && rm /var/www/html/index.html && chown -R dev:dev /var/www

# Setup PHP timezone
RUN echo date.timezone=Europe/Paris >> /etc/php5/apache2/conf.d/01-timezone.ini

# Install Composer
RUN cd $HOME &&\
    curl -sS https://getcomposer.org/installer | php &&\
    chmod +x composer.phar &&\
    mv composer.phar /usr/local/bin/composer

# Enable SSL and setup a testing SSL virtualhost
COPY /etc/apache2/sites-available/000-default-ssl.conf /etc/apache2/sites-available/000-default-ssl.conf
RUN a2enmod ssl &&\
    gencert localhost &&\
    a2ensite 000-default-ssl

WORKDIR /var/www

# Expose http and https ports
EXPOSE 80 443
