FROM php:8.2-apache
# Activa el mòdul mod_rewrite d'Apache
#RUN a2enmod rewrite
# Modifica el php.ini per mostrar els errors
RUN echo "display_errors = On" >> /usr/local/etc/php/php.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/php.ini
# Canvia el documentroot a public
RUN sed -i "s|/var/www/html|/var/www/html/public|g" /etc/apache2/sites-available/000-default.conf
# Habilita l'ús de fitxers .htaccess a la carpeta /var/www/html
#RUN sed -i "s|</VirtualHost>|\t<Directory /var/www/html>\n\t    Options Indexes FollowSymLinks\n\t    AllowOverride All\n\t    Require all granted\n</Directory>\n </VirtualHost>|g" /etc/apache2/sites-available/000-default.conf
# Instal·la les extensions necessaries per connectar-se amb una bdd mysql
RUN apt-get update && \
    docker-php-ext-install mysqli pdo pdo_mysql