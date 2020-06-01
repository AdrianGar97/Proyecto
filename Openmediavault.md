# Open Media Vault (OMV)

## ¿Qué es OMV?
OMV es un conocido sistema operativo Linux que, por un lado, funciona como un sistema operativo alternativo al que suelen traer los servidores NAS comerciales, y por otro lado nos permite montar fácilmente nuestro propio servidor NAS con todas las funciones y características que nos ofrecería cualquier NAS, como, por ejemplo, administración vía web, servidor FTP, servidor SAMBA, cliente Bittorrent, DLNA, gestión avanzada de usuarios, sistemas de control y monitorización, SSH, Rsync y NFS, entre otros servicios.

## Instalar OMV
La instalación es muy sencilla, solo necesitaremos ejecutar el siguiente comando en nuestra Raspberry con SO Raspbian, la instalación no suele durar más de 30 min, al finalizar puede que cambie nuestra IP, por lo que será necesario utilizar algún programa que detecte los dispositivos conectados a nuestro router para conectar de nuevo.
```
wget -O - https://github.com/OpenMediaVault-Plugin-Developers/installScript/raw/master/install | sudo bash
```

## Configuración
Para acceder al gestor web nos dirigimos a "nuestra-ip" en un navegador, el usuario por defecto será "admin" y la contraseña "openmediavault".

![Alt text](capturas/Openmediavault/1.png?raw=true "Optional Title")

Lo primero que deberemos hacer es cambiar la contraseña y definir una IP estática.
Para la contraseña nos didigirmos a "opcines generales - contraseña del adminisstrador web", la cambiamos y salvamos.

![Alt text](capturas/Openmediavault/2.png?raw=true "Optional Title")

Para definir una IP estática nos dirigimos a "red - interfaces" seleccionamos la que estamos usando, en mi caso la "wlan0", pulsamos en editar y configuramos la IP estática.

![Alt text](capturas/Openmediavault/3.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/4.png?raw=true "Optional Title")

También podemos cambiar el puerto por el que conectamos en "opcines generales - admin. web" para así poder istalar otro servidor web sin problemas.

![Alt text](capturas/Openmediavault/Puerto.png?raw=true "Optional Title")

## Agregar almacenamiento
Para agregar un dispositivo de almacenamiento nos dirigimos a "almacenamiento - sistema de archivos" y pulsamos en crear.

![Alt text](capturas/Openmediavault/5.png?raw=true "Optional Title")

Seleccionamos el disco a utilizar, en mi caso un pen drive, y lo formateamos en "EXT4", pulsamos en "Ok" y esperamos a que nos muestre el mensaje de "Se ha creado satisfactoriamente el sistema de archivos"

![Alt text](capturas/Openmediavault/6.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/7.png?raw=true "Optional Title")

A continuación montamos el disco y aplicamos los cambios.

![Alt text](capturas/Openmediavault/8.png?raw=true "Optional Title")

## Samba
Ahora nos dirigimos a "SMB/CIS - compartidos" y creamos la carpeta compartida que utilizaremos.

![Alt text](capturas/Openmediavault/9.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/10.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/11.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/12.png?raw=true "Optional Title")

Ahora volvemos a "opciones", habilitamos el servidor y salvamos y aplicamos los cambios.

![Alt text](capturas/Openmediavault/13.png?raw=true "Optional Title")

Creamos en "usuarios" el usuario que utilizaremos y le damos permisos en "carpetas compartidas" dentro de la carpeta compartida que queremos.

![Alt text](capturas/Openmediavault/18.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/19.png?raw=true "Optional Title")

## Acceso
Nos dirigimos a la pestaña "Red" de Windows y esperamos a que aparezca nuestra Raspberry.

![Alt text](capturas/Openmediavault/14.png?raw=true "Optional Title")

Entramos con el usuario creado para Samba.

![Alt text](capturas/Openmediavault/15.png?raw=true "Optional Title")

Y dentro de la carpeta compartida probamos si podemos meter archivos.

![Alt text](capturas/Openmediavault/16.png?raw=true "Optional Title")
![Alt text](capturas/Openmediavault/17.png?raw=true "Optional Title")