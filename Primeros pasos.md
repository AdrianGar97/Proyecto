# Primeros pasos

## Elementos necesarios
1. Rapberry Pi.
2. Fuente de alimentación.
3. Tarjeta SD.
4. Caja para Raspberry (Opcional).

## Sistema Operativo
En este caso vamos a utilizar el sistema operativo Raspbian, el cual podemos descargar desde la página oficial de Raspberry:
https://www.raspberrypi.org/downloads/raspbian/
A continuación lo montaremos en nuestra SD con la utilidad de "Discos" de Ubuntu (El SO que estoy utilizando junto con Raspbian en este proyecto) y dentro de esta seleccionamos la tarjeta SD y en "opciones adicionales de la partición" seleccionamos "Crear imagen de partición...". Seleccionamos el SO y aceptamos.

## IP estática
Tras instalar el SO y configurar lo básico (idioma, hora, contraseña,...) vamos a poner una IP fija para poder acceder mediante SSH y VNC y no tener que andar conectando un teclado, ratón y monitor todo el rato a la Raspberry.
Nos vamos a un terminal dentro de la Raspberry y ponemos el siguiente comando:
```
sudo nano /etc/dhcpcd.conf
```
Ahora dentro de ese archivo deberemos añadir las siguientes líneas para añadir nuestra IP fija:
```
interface wlan0
static ip_address=192.168.22.5/24
static routers=192.168.22.1
static domain_name_servers=192.168.22.1
```
## Habilitar SSH y VNC
Para habilitar los servicios de SSH y VNC nos vamos a una terminal. <br>
Para SSH hacemos:
```
sudo systemctl enable ssh
sudo systemctl start ssh
```
Para VNC:
```
sudo raspi-config
```
Nos saldrá una terminal, nos dirigimos a "Opciones avanzadas" y lo habilitamos.
## Acceder mediante SSH y VNC desde Ubuntu
Para acceder mediante SSH nos vamos a la terminal y escribimos el siguiente comando:
```
ssh pi@192.168.22.5
```
Aceptamos la conexión y escribimos la contraseña de la Raspberry. <br>
Para acceder mediante VNC, descargamos e instalamos VNC Viewer y ponemos la IP de la Raspberry (192.168.22.5). Al conectar nos pedirá usuario y contraseña, escribimos ambos y aceptamos para entrar.