# Primeros pasos 

## Sistema Operativo
Para la instalación del SO nos descargaremos la herremienta que nos proporcina Raspberry desde su página ofical para crear unidades SD listas para insertar en nuestra Raspberry.

![Alt text](capturas/primerospasos/1.png?raw=true "Optional Title")

Su uso es muy sencillo, lo primero es seleccionar el SO a instalar.

![Alt text](capturas/primerospasos/2.png?raw=true "Optional Title")

Tras esto seleccionamos la tarjeta SD donde se instalará.

![Alt text](capturas/primerospasos/3.png?raw=true "Optional Title")

Y esperamos a que termine la instalación.

![Alt text](capturas/primerospasos/4.png?raw=true "Optional Title")

## Habilitar SSH
Ahora sin extraer la terjeta SD nos dirigimos a ella y creamos un archivo ssh vacío (un archivo sin extrensión).

![Alt text](capturas/primerospasos/5.png?raw=true "Optional Title")

También editaremos el fichero "config.txt" para poder acceder por VNC más adelante, descomentando la siguiente línea.

![Alt text](capturas/primerospasos/6.png?raw=true "Optional Title")

## Habilitar VNC
Accedemos por SSH a nuestra raspberry, el usuario será "pi" y la contraseña por defecto "raspberry".

Ejecutamos el siguiente comando.
```
sudo raspi-config
```

En la opción "Interfacing options" podremos habilitar el VNC.

![Alt text](capturas/primerospasos/7.png?raw=true "Optional Title")
![Alt text](capturas/primerospasos/8.png?raw=true "Optional Title")

## Configuración final
Accedemos mediante VNC con nuestra ip y lo primero que veremos será una pestaña en la que editaremos la localización, la contraseña de la raspberry, nuestra red wifi (hasta ahora ibamos conectados mediante cable) y actualizaremos el sistema.

![Alt text](capturas/primerospasos/9.png?raw=true "Optional Title")
![Alt text](capturas/primerospasos/10.png?raw=true "Optional Title")

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