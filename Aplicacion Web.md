# Aplicación Web

Vamos a levantar una aplicación web con Apache y MariaDB utilizando dos sencillos scripts. El primero de ellos levanta Apache mientras que el segundo levanta MariaDB.
```
#!/bin/bash

set -x

# Actualizamos los repositorios de raspberry
apt-get update

# Instalamos Apache2
apt-get install apache2 -y

# Instalamos los paquetes adicionales para apache 
apt-get install php libapache2-mod-php php-mysql -y 

# Creamos la carpeta adminer y entramos en ella
sudo mkdir /var/www/html/adminer
cd /var/www/html/adminer 

# Descargamos Adminer y lo movemos a la carpeta html cambiando el nombre
wget https://github.com/vrana/adminer/releases/download/v4.7.3/adminer-4.7.3-mysql.php
mv adminer-4.7.3-mysql.php index.php

# Para tener la base de datos en ambos lados
cd /var/www/html
rm -rf iaw-practica-lamp
git clone https://github.com/josejuansanchez/iaw-practica-lamp

# Le damos permisos
chown www-data:www-data * -R
cd iaw-practica-lamp/src

# Configuramos la red
sed -i 's/localhost/192.168.22.5/' config.php

# Movemos la aplicación a la raiz y borramos las dependedncias
cd /var/www/html
rm index.hmtl
mv iaw-practica-lamp/src/* .
rm -rf iaw-practica-lamp
```

```
#!/bin/bash

set -x

# Actualizamos los repositorios de raspberry
apt-get update

# Instalamos los paquetes adicionales para apache 
apt-get install php libapache2-mod-php php-mysql -y

# Instalamos MariaDB
apt-get install mariadb-server

# Configuramos la red
sed -i 's/127.0.0.1/0.0.0.0/' /etc/mysql/mariadb.conf.d/50-server.cnf 

# Configuramos la contraseña de MySQL
DB_ROOT_PASSWD=root
mysqladmin --user=root password "$DB_ROOT_PASSWD"

# Configuramos MySQL
DB_NAME=lamp_db
DB_USER=lamp_user
DB_PASSWD=lamp_user
mysql -u root -p$DB_ROOT_PASSWD <<< "DROP DATABASE IF EXISTS $DB_NAME;"
mysql -u root -p$DB_ROOT_PASSWD <<< "CREATE DATABASE $DB_NAME;"
mysql -u root -p$DB_ROOT_PASSWD <<< "GRANT ALL PRIVILEGES ON $DB_NAME.* TO $DB_USER@'%' IDENTIFIED BY '$DB_PASSWD';"
mysql -u root -p$DB_ROOT_PASSWD <<< "FLUSH PRIVILEGES;"

# Importamos la base de datos
cd /home/pi
rm -rf iaw-practica-lamp
git clone https://github.com/josejuansanchez/iaw-practica-lamp
mysql -u root -p$DB_ROOT_PASSWD <  /home/pi/iaw-practica-lamp/db/database.sql

# Reiniciamos MySQL
systemctl restart mysql
systemctl status mysql
```

Tras ejecutar ambos scrips tendremos que acceder a la ip del servidor donde tengamos alojado Apache y MariaDB, nos saldrá la siguiente ventana.

![Alt text](capturas/aplicacion/1.png?raw=true "IP-servidor")

Comprobamos que podemos añadir datos y se guardan correctamente.

![Alt text](capturas/aplicacion/2.png?raw=true "IP-servidor")
![Alt text](capturas/aplicacion/3.png?raw=true "IP-servidor")

Por último comprobvamos que podemos acceder a Adminer para gestionar las bases de datos que tenemos.

![Alt text](capturas/aplicacion/4.png?raw=true "IP-servidor")
![Alt text](capturas/aplicacion/5.png?raw=true "IP-servidor")