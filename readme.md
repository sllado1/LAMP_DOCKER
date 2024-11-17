# Projecte Docker

Aquest projecte utilitza Docker per a la gestió dels contenidors necessaris per depurar una aplicació web. A continuació es detallen els fitxers Dockerfile i docker-compose.yml que s'utilitzen en aquest projecte.

## Dockerfile

El Dockerfile és un fitxer de text que conté les instruccions per crear una imatge Docker. 

### Dockerfile PHP
#### Utilitza la imatge base oficial de PHP 8.2 amb Apache preconfigurat.
FROM php:8.2-apache

#### Activa el mòdul `mod_rewrite` d'Apache.
#### Aquest mòdul permet utilitzar regles de reescriptura d'URL, sovint necessàries per a frameworks web.
RUN a2enmod rewrite

#### Modifica el fitxer de configuració `php.ini` per mostrar els errors.
#### Això activa la visualització d'errors PHP, útil per a entorns de desenvolupament.
RUN echo "display_errors = On" >> /usr/local/etc/php/php.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/php.ini

#### Canvia el `DocumentRoot` d'Apache per apuntar a la carpeta `public`.
#### Això assegura que la carpeta arrel del lloc web és `public`, que és una pràctica habitual per a seguretat i organització.
RUN sed -i "s|/var/www/html|/var/www/html|g" /etc/apache2/sites-available/000-default.conf

#### Habilita l'ús de fitxers `.htaccess` dins de la carpeta `/var/www/html`.
#### Això permet que es defineixin regles de configuració específiques per a cada directori amb `.htaccess`, 
#### incloent regles de reescriptura, permisos d'accés, etc.
RUN sed -i "s|</VirtualHost>|\t<Directory /var/www/html>\n\t    Options Indexes FollowSymLinks\n\t    AllowOverride All\n\t    Require all granted\n</Directory>\n </VirtualHost>|g" /etc/apache2/sites-available/000-default.conf

#### Instal·la les extensions PHP necessàries per connectar-se a una base de dades MySQL.
#### `mysqli`, `pdo`, i `pdo_mysql` són extensiones que permeten la connexió i manipulació de bases de dades MySQL amb PHP.
RUN apt-get update && \
    docker-php-ext-install mysqli pdo pdo_mysql

### Dockerfile MYSQL 
#### Utilitza la darrera versió de la imatge oficial de MySQL com a base
FROM mysql:latest

#### Canvia a l'usuari root per executar les comandes següents amb privilegis d'administrador
USER root

#### Ajusta els permisos de la carpeta `/var/lib/mysql` perquè sigui accessible
#### amb permisos de lectura i execució (755) per a l'usuari root i altres usuaris.
RUN chmod 755 /var/lib/mysql

## docker-compose.yml

El fitxer docker-compose.yml s'utilitza per definir i executar aplicacions multi-contenidor. Aquí teniu un exemple bàsic:


## Conclusió

Aquest projecte utilitza Docker per simplificar la gestió de dependències i l'execució de l'aplicació en diferents entorns. Els fitxers Dockerfile i docker-compose.yml proporcionen una manera fàcil de construir i executar els contenidors necessaris.