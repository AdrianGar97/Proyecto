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

## **5. NAS**

## **6. Openmediavault**

## **7. Aplicacion Web**

## **8. Pi-hole**

## **9. Emuladores**