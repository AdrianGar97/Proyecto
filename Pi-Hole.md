# Pi-hole

## ¿Qué es un Pi-hole?
Pi-Hole es un servidor DNS Cache diseñado para bloquear el siguiente tipo de contenido a nivel de red:
1. Webs fraudulentas que se dedican a instalar malware o a usar nuestro hardware para minar criptomoneda.
2. Bloquear las web que no queramos que visiten nuestros hijos o la gente conectada de nuestra red local. A modo de ejemplo existen filtros que bloquearán la totalidad de páginas pornográficas, páginas relacionadas con el juego online, etc.
3. La totalidad de publicidad que aparece en páginas web y en aplicaciones de nuestros dispositivos.

## Instalar Pi-hole
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

## Configurar Pi-hole
### Fijar una contraseña
Primero deberemos fijar una contraseña de administrador para acceder al panel de administración. Para ello ejecutamos el siguiente comando:
```
pihole -a -p
```

![Alt text](capturas/Pi-hole/14.png?raw=true "Optional Title")

Después de ejecutar el comando definimos nuestra contraseña y reiniciamos la Raspberry.

### Acceder al panel de administrador
Podremos administrar Pi-hole desde cualquier equipo conectado a nuestra red accediendo mediante un navegador. <br>
Para acceder solo es necesario ingresar en el navegador la siguiente URL:
```
http://ip-servidor/admin/
```

![Alt text](capturas/Pi-hole/9.png?raw=true "Optional Title")

### Actualizar las listas para filtrar y bloquear anuncios
En la ubicación /etc/cron.d/pihole hay configurado un cronjob que actualiza nuestras listas de bloqueo de forma automática.

Si alguna vez queremos realizar una actualización manual tenemos dos opciones.

La primera de ellas es ejecutar el siguiente comando en la terminal de nuestro dispositvo con Pi-hole:
```
pihole -g
```
La segunda opción es accediendo al panel de administración web. En el panel de administración pulsamos sobre el menú Tools. Cuando se despliegue el submenú pulsamos en Update Gravity. Finalmente pulsamos en la opción Update Lists y se actualizaran la totalidad de listas a que estamos suscritos.

![Alt text](capturas/Pi-hole/10.png?raw=true "Optional Title")

### Añadir filtros o listas adicionales
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