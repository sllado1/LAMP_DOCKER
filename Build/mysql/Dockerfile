# Utilitza la darrera versió de la imatge oficial de MySQL com a base
FROM mysql:latest

# Canvia a l'usuari root per executar les comandes següents amb privilegis d'administrador
USER root

# Ajusta els permisos de la carpeta `/var/lib/mysql` perquè sigui accessible
# amb permisos de lectura i execució (755) per a l'usuari root i altres usuaris.
RUN chmod 755 /var/lib/mysql