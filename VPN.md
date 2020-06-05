# VPN

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