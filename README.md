---
title: "Proyecto"
author: [Adrián García Fortún]
subject: "Markdown"
keywords: [Markdown, README]
lang: "en"
toc-own-page: "true"
---

# **PROYECTO INTEGRADO**

![Alt text](capturas/logo.png?raw=true "Logo")

## **Índice**
1. ¿Qué es una Raspberry Pi?
2. Componentes báscios
3. Primeros pasos
4. VPN
5. NAS
6. Openmediavault (Caso práctico)
7. Aplicacion Web
8. Pi-hole
9. Emuladores

## **1. ¿Qué es una Raspberry Pi?**
Es un ordenador del tamaño de una tarjeta de crédito. Fue lanzado en 2006 por la Fundación Raspberry Pi con el objeto de estimular la enseñanza de informática en las escuelas de todo el mundo.

Podemos considerar que es un ordenador completo, con la excepción de que no incluye el cable de alimentación, la caja ni el disco duro, para el que se utiliza una tarjeta de memoria SD. Además necesitaremos teclado, ratón y la antena wifi (en el que caso de que no tenga y no podamos conectar por cable). Además un monitor, aunque como verremos más adelante no es del todo necesarios, al igual que el raton y el teclado.

## **2. Componentes báscios**
* CPU ARM Cortex-A72 de 64 bits con cuatro núcleos a 1,5 GHz
* 1 GB, 2 GB o 4 GB de SDRAM LPDDR4
* Gigabit Ethernet
* Red inalámbrica 802.11ac de doble banda
* Bluetooth 5.0
* Dos puertos USB 3.0 y dos puertos USB 2.0
* Soporte de doble monitor , en resoluciones de hasta 4K.

![Alt text](capturas/raspberry.png?raw=true "Raspberry")

## **3. Primeros pasos**
### Sistema Operativo
Para la instalación del SO nos descargaremos la herremienta que nos proporcina Raspberry desde su página ofical para crear unidades SD listas para insertar en nuestra Raspberry.

![Alt text](capturas/primerospasos/1.png?raw=true "Imager")

Su uso es muy sencillo, lo primero es seleccionar el SO a instalar.

![Alt text](capturas/primerospasos/2.png?raw=true "Selección SO")

Tras esto seleccionamos la tarjeta SD donde se instalará.

![Alt text](capturas/primerospasos/3.png?raw=true "Selección SD")

Y esperamos a que termine la instalación.

![Alt text](capturas/primerospasos/4.png?raw=true "Instalación")

### Habilitar SSH
Ahora sin extraer la terjeta SD nos dirigimos a ella y creamos un archivo ssh vacío (un archivo sin extrensión).

![Alt text](capturas/primerospasos/5.png?raw=true "Crear SSH")

También editaremos el fichero "config.txt" para poder acceder por VNC más adelante, descomentando la siguiente línea.

![Alt text](capturas/primerospasos/6.png?raw=true "Descomentar línea VNC")

### Habilitar VNC
Accedemos por SSH a nuestra raspberry, el usuario será "pi" y la contraseña por defecto "raspberry".

Ejecutamos el siguiente comando.
```
sudo raspi-config
```

En la opción "Interfacing options" podremos habilitar el VNC.

![Alt text](capturas/primerospasos/7.png?raw=true "Interfacing options")
![Alt text](capturas/primerospasos/8.png?raw=true "Habilitar VNC")

### Configuración final
Accedemos mediante VNC con nuestra ip y lo primero que veremos será una pestaña en la que editaremos la localización, la contraseña de la raspberry, nuestra red wifi (hasta ahora ibamos conectados mediante cable) y actualizaremos el sistema.

![Alt text](capturas/primerospasos/9.png?raw=true "Conectar por VNC")
![Alt text](capturas/primerospasos/10.png?raw=true "Configuración inicial")

Por último en la consola ejecutamos el siguiente comando.
```
sudo nano /etc/dhcpcd.conf
```

Y agregamos las siguientes líneas para tener una IP fija en nuestra raspberry.
```
interface wlan0
static ip_address=192.168.22.5/24
static routers=192.168.22.1
static domain_name_servers=192.168.22.1
```

## **4. VPN**
### ¿Qué es una VPN?
Las VPN (Virtual Private Network) o Red Privada Virtual en español son un tipo de red en el que se crea una extensión de una red privada para su acceso desde Internet, es como la red local que tienes en casa o en la oficina pero sobre Internet.

Gracias a una conexión VPN, podemos establecer contacto con máquinas que estén alojadas en nuestra red local -u otras redes locales- de forma totalmente segura, ya que la conexión que se establece entre ambas máquinas viaja totalmente cifrada, es como si desde nuestro equipo conectado a Internet estableciésemos un túnel privado y seguro hasta nuestro hogar o hasta nuestra oficina, con el que podremos comunicarnos sin temer que nuestros datos sean vulnerables.

### Instalación de PiVPN
Utilizaremos el comando curl para instalar la aplicación, ejecutamos el siguiente comando:
```
curl -L https://install.pivpn.io | bash
```
Tras instalarse todas las dependencias necesarias nos aparecerá la pantalla que nos indicará que vamos a instalar:

![Alt text](capturas/VPN/1.png?raw=true "Inicio instalación")

Ahora nos toca confirmar que la dirección IP estática que nos indica es la correcta:

![Alt text](capturas/VPN/3.png?raw=true "IP")

Seleccionamos el perfil que vamos a usar para instalar la VPN:

![Alt text](capturas/VPN/5.png?raw=true "Perfil")

Pondremos que nuestro servidor se mantenga siempre actualizado:

![Alt text](capturas/VPN/6.png?raw=true "Actualizaciones")

Ahora seleccionaremos el protocolo, es recomendable utilizar siempre el UDP:

![Alt text](capturas/VPN/7.png?raw=true "Protocolo")

Ahora introduciremos el puerto necesario para conectarnos con nuestra VPN, en este caso por defecto es 1194:

![Alt text](capturas/VPN/8.png?raw=true "Puerto")

Toca elegir el tipo de cifrado que queremos en nuestro VPN:

![Alt text](capturas/VPN/9.png?raw=true "Cifrado")

Después de haber confirmado los datos, nos sale otro mensaje advirtiendo que va a generar una key:

![Alt text](capturas/VPN/11.png?raw=true "Key")

Una vez haya terminado el proceso, seleccinaremos la forma con la cual nos conectaremos a nuestra VPN, sea por IP o por DNS Pública. Yo escojo la IP pública:

![Alt text](capturas/VPN/12.png?raw=true "IP Publica")

Seleccionamos el proveedor DNS principal para nuestro VPN:

![Alt text](capturas/VPN/13.png?raw=true "DNS")

Y la instalación ya ha finalizado, ahora reiniciamos el sistema:

![Alt text](capturas/VPN/14.png?raw=true "Finalizado")

Ahora crearemos nuestro usuario con el siguiente comando:
```
pivpn add
```

El archivo creado se encuentra en "/home/NUESTRO_USUARIO/ovpns" y será el que usemos para nuestro cliente.

### Cliente OpenVPN
Tras instalar nuestro cliente deberemos dirigirnos a "C:/Archivos de programa/OpenVPN/config" y copiar allí el archivo de cliente creado. Para ello podemos conectarnos mediante FTP a nuestra raspberry y copipar el archivo.

Para ejecutar el programa damos al botón "Inicio de Windows - Barra de Programas - OpenVPN - OpenVPN Connect".

![Alt text](capturas/VPN/15.png?raw=true "Abrir programa")

Vamos a la barra de tareas, nos saldrá un icono con un candado hacemos "botón segundario - Connect".

![Alt text](capturas/VPN/16.png?raw=true "Conectar")

Introducimos la contraseña configurada para nuestro cliente y listo.

![Alt text](capturas/VPN/17.png?raw=true "Contraseña")

## **5. NAS**
### ¿Qué es un NAS?
Un sistema NAS es un dispositivo de almacenamiento conectado a una red que permite almacenar y recuperar los datos en un punto centralizado para usuarios autorizados de la red y multiplicidad de clientes. Los dispositivos NAS son flexibles y expandibles; esto lo que implica es que a medida que vaya necesitando más capacidad de almacenamiento, podrá añadirla a lo que ya tiene. Un dispositivo NAS es como tener una nube privada en la oficina. Es más veloz, menos caro y ofrece todos los beneficios de una nube pública dentro del emplazamiento, lo que le proporciona todo el control.

Los sistemas NAS son ideales para las pequeñas y medianas empresas:
1. Funcionamiento sencillo.
2. Coste más reducido.
3. Copias de seguridad sencillas para que siempre estén accesibles cuando las necesite.
4. Buena centralización del almacenamiento de datos de una forma segura y fiable.

### Preparando el SO
Ahora deberemos preparar nuestro SO. Para ello habilitaremos el SSH y cambiaremos el nombre del host para poder tener un acceso más fácil, por ejemplo, podemos ponerle de nombre "nas". Por último comprobamos que todo está actualizado con:
```
sudo apt update && sudo apt -y upgrade
```
Por último reiniciamos el SO.

### Agregar el almacenamiento
Lo primero será conectar nuestros discos duros y utilizar el comando "lsblk" para comprobar que los detecta. En este caso nos debería de salir "mmcblko" que sería nuestra SD y "sda" y "sdb" que serían los discos duros.

Ahora debemos particionar las unidades para que Raspbian entienda cómo almacenar datos en ellas. Para ello usamos el comando:
```
sudo fdisk /dev/sda
```
Presionamos "n" para crear la nueva partición y a continuación "p" para que sea primaria. Ahora nos hará una serie de preguntas, solo deberemos presionar "enter" hasta que aparezca "Creando una nueva partición". Ahora pulsamos "w" para escribir los cambios y repetimos el paso con el segundo disco duro:
```
sudo fdisk /dev/sdb
```

Ahora vamos a crear una RAID 1, así si uno de los discos falla, nuestro NAS seguirá funcionando y al reemplazar el disco roto reconstruirá la información de nuevo.
Primero instalamos el administrador de RAIDS:
```
sudo apt install mdadm
```
Y a continuación le indicamos que cree el RAID 1:
```
sudo mdadm --create --verbose /dev/md0 --level=mirror --raid-devices=2 /dev/sda1 /dev/sdb1
```

Ahora ambos discos se verán como uno solo. Así que formateamos y montamos la nueva unidad virtual:
```
sudo mkdir -p /mnt/raid1
sudo mkfs.ext4 /dev/md0
sudo mount /dev/md0 /mnt/raid1/
ls -l /mnt/raid1
```
Ahora deberemos asegurarnos que la unidad está montada cada vez que arranque para ello hacemos lo siguiente:
```
sudo nano /etc/fstab
```
Y agregamos la siguiente línea:
```
/dev/md0 /mnt/raid1/ ext4 defaults,noatime 0 1
```
Guardamos y ejecutamos el siguiente comando para que se inicie correctamente durante el arranque:
```
sudo mdadm --detail --scan | sudo tee -a
/etc/mdadm/mdadm.conf
```
Reiniciamos y todo listo para funcionar.

### Samba
Utilizaremos el protocolo Samba para compartir los archivos en red. Al no venir instalado deberemos hacer con el siguiente comando:
```
sudo apt install samba samba-common-bin
```
Ahora crearemos el directorio y daremos acceso a todos los usuarios:
```
sudo mkdir /mnt/raid1/shared
sudo chmod -R 777 /mnt/raid1/shared
```
Ahora compartiremos el directorio en red editando el siguiente archivo:
```
sudo nano /etc/samba/smb.conf
```
Agregaremos las siguientes líneas al final:
```
[shared]
path=/mnt/raid1/shared
writeable=Yes
create mask=0777
directory mask=0777
public=no
```
Por último reiniciamos Samba:
```
sudo systemctl restart smbd
```

Ahora daremos acceso a los archivos compartidos necesitamos ejecutar un comando que nos pedirá una contraseña para Samba:
```
sudo smbpasswd -a pi
```
De esta manera generamos una contraseña con la que el usuario "pi" que es el por defecto de la Raspberry podrá acceder a los contenidos desde Windows, macOS u otros dispositivos Raspberry Pi.
Para crear usuarios adicionales ejecutamos los siguientes comandos:
```
sudo adduser username
sudo smbpasswd -a username
```

### Crear directorios de inicio
Para que cada usuarios tenga una carpeta donde guarde sus propios archivos individualmente podemos crearle un directorio dentro del RAID con el siguiente comando:
```
mkdir /mnt/raid1/shared/username
sudo chown -R username /mnt/raid1/shared/username
sudo chmod -R 700 /mnt/raid1/shared/username
```

## **6. Openmediavault**
### ¿Qué es OMV?
OMV es un conocido sistema operativo Linux que, por un lado, funciona como un sistema operativo alternativo al que suelen traer los servidores NAS comerciales, y por otro lado nos permite montar fácilmente nuestro propio servidor NAS con todas las funciones y características que nos ofrecería cualquier NAS, como, por ejemplo, administración vía web, servidor FTP, servidor SAMBA, cliente Bittorrent, DLNA, gestión avanzada de usuarios, sistemas de control y monitorización, SSH, Rsync y NFS, entre otros servicios.

### Instalar OMV
La instalación es muy sencilla, solo necesitaremos ejecutar el siguiente comando en nuestra Raspberry con SO Raspbian, la instalación no suele durar más de 30 min, al finalizar puede que cambie nuestra IP, por lo que será necesario utilizar algún programa que detecte los dispositivos conectados a nuestro router para conectar de nuevo.
```
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```

### Configuración
Para acceder al gestor web nos dirigimos a "nuestra-ip" en un navegador, el usuario por defecto será "admin" y la contraseña "openmediavault".

![Alt text](capturas/Openmediavault/1.png?raw=true "Login")

Lo primero que deberemos hacer es cambiar la contraseña y definir una IP estática.
Para la contraseña nos didigirmos a "opcines generales - contraseña del adminisstrador web", la cambiamos y salvamos.

![Alt text](capturas/Openmediavault/2.png?raw=true "Combiar contraseña")

Para definir una IP estática nos dirigimos a "red - interfaces" seleccionamos la que estamos usando, en mi caso la "wlan0", pulsamos en editar y configuramos la IP estática.

![Alt text](capturas/Openmediavault/3.png?raw=true "IP estática")
![Alt text](capturas/Openmediavault/4.png?raw=true "Confirmación")

También podemos cambiar el puerto por el que conectamos en "opcines generales - admin. web" para así poder istalar otro servidor web sin problemas.

![Alt text](capturas/Openmediavault/Puerto.png?raw=true "Puerto")

### Agregar almacenamiento
Para agregar un dispositivo de almacenamiento nos dirigimos a "almacenamiento - sistema de archivos" y pulsamos en crear.

![Alt text](capturas/Openmediavault/5.png?raw=true "Crear")

Seleccionamos el disco a utilizar, en mi caso un pen drive, y lo formateamos en "EXT4", pulsamos en "Ok" y esperamos a que nos muestre el mensaje de "Se ha creado satisfactoriamente el sistema de archivos"

![Alt text](capturas/Openmediavault/6.png?raw=true "Ok")
![Alt text](capturas/Openmediavault/7.png?raw=true "Finalizado")

A continuación montamos el disco y aplicamos los cambios.

![Alt text](capturas/Openmediavault/8.png?raw=true "Aplicar cambios")

### Samba
Ahora nos dirigimos a "SMB/CIS - compartidos" y creamos la carpeta compartida que utilizaremos.

![Alt text](capturas/Openmediavault/9.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/10.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/11.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/12.png?raw=true "Optional Title")

Ahora volvemos a "opciones", habilitamos el servidor y salvamos y aplicamos los cambios.

![Alt text](capturas/Openmediavault/13.png?raw=true "Habilitar servidor")

Creamos en "usuarios" el usuario que utilizaremos y le damos permisos en "carpetas compartidas" dentro de la carpeta compartida que queremos.

![Alt text](capturas/Openmediavault/18.png?raw=true "Crear usuario 1")
![Alt text](capturas/Openmediavault/19.png?raw=true "Crear usuario 2")

### Acceso
Nos dirigimos a la pestaña "Red" de Windows y esperamos a que aparezca nuestra Raspberry.

![Alt text](capturas/Openmediavault/14.png?raw=true "Red")

Entramos con el usuario creado para Samba.

![Alt text](capturas/Openmediavault/15.png?raw=true "Conectar")

Y dentro de la carpeta compartida probamos si podemos meter archivos.

![Alt text](capturas/Openmediavault/16.png?raw=true "Subir archivo 1")
![Alt text](capturas/Openmediavault/17.png?raw=true "Subir archivo 2")

## **7. Aplicacion Web**
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

![Alt text](capturas/aplicacion/2.png?raw=true "Añadir datos")
![Alt text](capturas/aplicacion/3.png?raw=true "Guardados")

Por último comprobvamos que podemos acceder a Adminer para gestionar las bases de datos que tenemos.

![Alt text](capturas/aplicacion/4.png?raw=true "Login gestor")
![Alt text](capturas/aplicacion/5.png?raw=true "Gestor")

## **8. Pi-hole**
### ¿Qué es un Pi-hole?
Pi-Hole es un servidor DNS Cache diseñado para bloquear el siguiente tipo de contenido a nivel de red:
1. Webs fraudulentas que se dedican a instalar malware o a usar nuestro hardware para minar criptomoneda.
2. Bloquear las web que no queramos que visiten nuestros hijos o la gente conectada de nuestra red local. A modo de ejemplo existen filtros que bloquearán la totalidad de páginas pornográficas, páginas relacionadas con el juego online, etc.
3. La totalidad de publicidad que aparece en páginas web y en aplicaciones de nuestros dispositivos.

### Instalar Pi-hole
Para instalar Pi-hole necesitaremos la herrmienta Curl, ya que nos descargaremos y ejecutaremos un script. Para asegurarnos que está instalado ejecutamos el siguiente comando en la terminal:
```
sudo apt-get install curl
```
A continuación descargamos e instalamos el script ejecutando el siguiente comando en la terminal:
```
curl -sSL https://install.pi-hole.net | sudo bash
```

![Alt text](capturas/Pi-hole/0.png?raw=true "Optional Title")

Tras ejecutar este comando nos aparecerá una pantalla que nos indicará que nuestro dispositivo se convertirá en un bloqueador de publicidad:

![Alt text](capturas/Pi-hole/1.png?raw=true "Optional Title")

Después se nos advertirá de que nuestra Raspberry Pi precisará de un IP estática para funcionar de manera adecuada.

Seguidamente se precisará que seleccionemos nuestra interfaz de red, seleccionaré "wlan0" en mi caso ya que estoy conectado a una red wifi:

![Alt text](capturas/Pi-hole/2.png?raw=true "Optional Title")

Ahora seleccionaremos el servidor DNS que vamos a utilizar, en mi caso usaré los servidores de OpenDNS:

![Alt text](capturas/Pi-hole/4.png?raw=true "Optional Title")

Seleccionamos los protocolos de internet que queremos filtrar, seleccionamos ambos:

![Alt text](capturas/Pi-hole/5.png?raw=true "Optional Title")

En la siguiente pantalla nos preguntará si queremos que la IP que nos muestra sea nuestra IP fija:

![Alt text](capturas/Pi-hole/6.png?raw=true "Optional Title")

Por último, seleccioaremos que instale la interfaz web y que nos guarde el registro log de la peticones DNS aceptadas:

![Alt text](capturas/Pi-hole/7.png?raw=true "Optional Title") 
![Alt text](capturas/Pi-hole/8.png?raw=true "Optional Title")

Una vez definidas las opciones de configuración se procederá a la instalación de los paquetes necesarios para que Pi-hole funcione de forma adecuada.

![Alt text](capturas/Pi-hole/3.png?raw=true "Optional Title")

### Configurar Pi-hole
#### Fijar una contraseña
Primero deberemos fijar una contraseña de administrador para acceder al panel de administración. Para ello ejecutamos el siguiente comando:
```
pihole -a -p
```

![Alt text](capturas/Pi-hole/14.png?raw=true "Optional Title")

Después de ejecutar el comando definimos nuestra contraseña y reiniciamos la Raspberry.

#### Acceder al panel de administrador
Podremos administrar Pi-hole desde cualquier equipo conectado a nuestra red accediendo mediante un navegador. <br>
Para acceder solo es necesario ingresar en el navegador la siguiente URL:
```
http://ip-servidor/admin/
```

![Alt text](capturas/Pi-hole/9.png?raw=true "Optional Title")

#### Actualizar las listas para filtrar y bloquear anuncios
En la ubicación /etc/cron.d/pihole hay configurado un cronjob que actualiza nuestras listas de bloqueo de forma automática.

Si alguna vez queremos realizar una actualización manual tenemos dos opciones.

La primera de ellas es ejecutar el siguiente comando en la terminal de nuestro dispositvo con Pi-hole:
```
pihole -g
```
La segunda opción es accediendo al panel de administración web. En el panel de administración pulsamos sobre el menú Tools. Cuando se despliegue el submenú pulsamos en Update Gravity. Finalmente pulsamos en la opción Update Lists y se actualizaran la totalidad de listas a que estamos suscritos.

![Alt text](capturas/Pi-hole/10.png?raw=true "Optional Title")

#### Añadir filtros o listas adicionales
Los filtros de serie bloquean prácticamente la totalidad de los anuncios. Pero también se pueden agregar otro filtros buscando en algunas páginas cómo:
```
https://github.com/StevenBlack/hosts

https://discourse.pihole.net/t/i-concatenated-every-blocklist-i-could-find/5184/13

https://wally3k.github.io/

https://github.com/pihole/pihole/wiki/Customising-sources-for-ad-lists
```
No es recomendado añadir más filtros de los que son estrictamente necesarios ya que, cuando más filtros añadamos más carga de trabajo tendrá el dispositivo que tiene instalado Pi-hole.

Para añadir una lista, accedemos a uno de los enlaces y clicamos en raw. copiamos su URL.

![Alt text](capturas/Pi-hole/11.png?raw=true "Optional Title")
![Alt text](capturas/Pi-hole/15.png?raw=true "Optional Title")

Dentro de la interfaz web de Pi-hole vamos a Settings y dentro de a Block Lists.

Pegamos la URL y pulsamos en Save and Update.

![Alt text](capturas/Pi-hole/12.png?raw=true "Optional Title")

## **9. Emuladores**
![Alt text](capturas/emuladores.jpg?raw=true "VS Emuladores")

### RetroPie
Problablemente el más popular de todos. Utiliza Raspbian como base, así como EmulationStation y RetroArch. Se puede instalar como una distribución completa de Linux o puede instalarse sobre Raspbian para un entorno de escritorio Linux tradicional con RetroPie para emulación. La interfaz de EmulationStation ofrece un diseño atractivo e intuitivo para jugar, y RetroArch, garantizan un rendimiento óptimo.

#### Lista de emuladores
3do / Amiga / Amstrad CPC / Apple II / Atari 2600 / Atari 5200 and 8-bit series / Atari 7800 / Atari Jaguar / Atari Lynx / Atari ST/STE/TT/Falcon / CoCo / Colecovision / Commodore 64/VIC 20/PET / Daphne / Dragon 32 / Dreamcast / Famicom / Disk System / FinalBurn Alpha / GameCube / Game & Watch / Game Gear / Game Boy / Game Boy Color / Game Boy / Advance / Intellivision / Macintosh / MAME / Master System / Mega Drive/Genesis / MESS / MSX / Nintendo 64 / Nintendo DS / Nintendo Entertainment System / Neo Geo / Neo Geo Pocket / Neo Geo Pocket Color / Oric-1/Atmos / PC / PC-8800 PC Engine/TurboGrafx-16 / PC-FX / PSP / PlayStation 1 / PlayStation 2 / ResidualVM / SAM Coupé / Saturn / ScummVM / Sega 32X / Sega CD / Sega / SG-1000 / Sharp X68000 / Super Nintendo / Entertainment System / TI-99/4A / TRS-80 / Vectrex / Videopac/Odyssey2 / Virtual Boy / Wii / WonderSwan / WonderSwan Color / Zmachine / ZX Spectrum

### Recalbox
También utiliza EmulationStation y RetroArch. Recalbox utiliza una pantalla de inicio personalizada, música de fondo y su pantalla principal. Es más reciente que RetroPie.

La principal diferencia entre RetroPie y Recalbox es la personalización. RetroPie ofrece una variedad de shaders personalizados, ajustes de emulación y más. Recalbox incluye shaders y scanlines, pero añadir sus propios sombreadores es un poco más complejo que añadirlos a RetroPie.

#### Lista de emuladores
Arcade / Arcade Classics / Nintendo Entertainment System (NES) / Family Computer Disk System (FDS) / Super Nintendo Entertainment System (SNES) / Sega Master System / Sony PlayStation / Sega Genesis / Nintendo GameBoy / GameBoy Advance (GBA) / GameBoy Color (GBC) / Atari 7800 / Atari 2600 / PC Engine (CD) / Sega SG1000 / MXS 1/2/2+ / Nintendo 64 (N64) / Sega 32x / Sega CD / ScummVM / Game and Watch / Vectrex / Game Gear / Virtual Boy / Lynx / NeoGeo Pocket Color / Wonderswan Color / Neo Geo / Supergrafx / Odyssey 2 Videopac / Amstrad CPC / Atari ST / Sinclair ZX81 / Sinclair ZX Spectrum / PlayStation Portable (PSP) / Commodore 64 (C64)

### Lakka
Utiliza RetroArch y Libretro. Se trata de una interfaz que imita la de PlayStation 3 XrossMediaBar (XMB). Esta es la opción más robusta que encontrarás, con una variedad de opciones para la optimización de shaders, audio y video. A veces puede ser hasta demasiado.

#### Lista de emuladores
3DO / PlayStation / SNES/Super Famicom / Nintendo DS / Arcade / Game Boy/Game Boy Color / Sega Master System/Game Gear/Mega Drive/CD / Lynx / Neo Geo Pocket/Color / PC Engine/TurboGrafx 16 / PC-FX / Virtual Boy / WonderSwan/Color / Nintendo 64 / NES/Famicom / PSP / Atari 7800 / Atari 2600 / Game Boy Advance / Atari Jaguar

### Batocera
Batocera es un sistema operativo de emulación basado en Debian  y tiene varias similitudes con Recalbox. Es bastante fácil de configurar e instalar y proporciona a RetroArch la interfaz de EmulationStation.

#### Lista de emuladores
Amiga 500 / Amiga 500+ / Amiga 1200 / Amiga 4000 / Amiga CDTV / Amstrad CPC / Apple II / Atari 2600 / Atari 7800 / Atari ST / CaveStory / Commodore 64 / ColecoVision / Dreamcast / FDS (Family Disk System) / Final Burn Alpha / Game & Watch / Game Boy Color / Game Gear / Game Boy / Game Boy Advance / Intellivision / Lutro / Lynx / Mame / Master System / Megadrive / MSX 1-2-2+ / MSX1 / MSX2+ / Neo Geo / Neo Geo CD / Neo Geo Pocket B&W / Neo Geo Pocket Color / Nintendo / Nintendo 64 / Odyssey 2 / DOS / PC Engine / PC Engine CD / PlayStation / Playstation portable / PR Boom / Scumm VM / Sega 32 X / Sega CD / Sega SG 1000 / Super Nintendo / Supergrafx / Vectrex / Virtual Boy / Wonderswan B&W / Wonderswan Color / ZX Spectrum / ZX81

### ¿Cuál instalar?
#### Facilidad de uso
Recalbox y Batocera ganan por su instalación y configuración que son muy fáciles de hacer.

*Ganador: Recalbox/Batocera*

#### Personalización
RetroPie tiene una cantidad moderada, mientras que Lakka tiene demasiadas y los otros muy pocas.

*Ganador: RetroPie*

#### Listo para jugar
Lakka desde su primer arranque ni siquiera requiere la configuración de un mando. Es bastante fácil de usar, excepto por la sobrecarga de opciones de shaders.

*Ganador: Lakka*

#### Compatibilidad hardware
Lakka está disponible en una variedad de plataformas, desde Pi hasta WeTek Play 2.

*Ganador: Lakka*

#### Comunidad
RetroPie con su Wiki y GitHub es una opción fantástica y rica en recursos para cualquier usuario.

*Ganador: RetroPie*

#### Experiencia general
Su scraper es fenomenal y tiene más opciones de emulación que Recalbox. La interfaz de EmulationStation es superior a la de Lakka, y también contiene algunos títulos integrados. También encontramos el Kodi Media Center en RetroPie.

*Ganador: RetroPie*

#### Conclusión final
*Ideal para principiantes: Recalbox/Batocera*

*Ideal para usuarios experimentados: Lakka*

*Mejor en general: RetroPie*