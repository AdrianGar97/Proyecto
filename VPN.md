# VPN

## Elementos necesarios
1. Rapberry Pi.
2. Fuente de alimentación.
3. Tarjeta SD con el SO Raspbian lite.
4. Caja para Raspberry (Opcional).

## ¿Qué es una VPN?
Las VPN (Virtual Private Network) o Red Privada Virtual en español son un tipo de red en el que se crea una extensión de una red privada para su acceso desde Internet, es como la red local que tienes en casa o en la oficina pero sobre Internet.

Gracias a una conexión VPN, podemos establecer contacto con máquinas que estén alojadas en nuestra red local -u otras redes locales- de forma totalmente segura, ya que la conexión que se establece entre ambas máquinas viaja totalmente cifrada, es como si desde nuestro equipo conectado a Internet estableciésemos un túnel privado y seguro hasta nuestro hogar o hasta nuestra oficina, con el que podremos comunicarnos sin temer que nuestros datos sean vulnerables.

## Instalación de PiVPN
Utilizaremos el comando curl para instalar la aplicación, ejecutamos el siguiente comando:
```
curl -L https://install.pivpn.io | bash
```
Tras instalarse todas las dependencias necesarias nos aparecerá la pantalla que nos indicará que vamos a instalar:

*img1*

Escogeremos nuetra interfaz de red, en mi caso la "wlan0":

*img2*

Ahora nos toca confirmar que la dirección IP estática que nos indica es la correcta:

*img3*

Nos advertirá que podría haber un conflicto IP en nuestro router, pulsamos "OK" y seguimos:

*img4*

Seleccionamos el perfil que vamos a usar para instalar la VPN:

*img5*

Pondremos que nuestro servidor se mantenga siempre actualizado:

*img6*

Ahora seleccionaremos el protocolo, es recomendable utilizar siempre el UDP:

*img7*

Ahora introduciremos el puerto necesario para conectarnos con nuestra VPN, en este caso por defecto es 1194:

*img8*

Toca elegir el tipo de cifrado que queremos en nuestro VPN, escogemos 2048 para tener mejor rendimiento:

*img9*

A continuación podemos agregar nuestros datos para crear el certificado para la conexión VPN, aunque no es obligatorio:

*img10*

espués de haber confirmado los datos, nos sale otro mensaje advirtiendo que va a generar una key Diffie-Hellman:

*img11*

Una vez haya terminado el proceso, seleccinaremos la forma con la cual nos conectaremos a nuestra VPN, sea por IP o por DNS Pública. Yo escojo la IP pública:

*img12*

Seleccionamos el proveedor DNS principal para nuestro VPN:

*img13*

Y la instalación ya ha finalizado, ahora reiniciamos el sistema:

*img14*

Ahora crearemos nuestro usuario con el siguiente comando:
```
pivpn add
```

El archivo creado se encuentra en "/home/NUESTRO_USUARIO/ovpns" y será el que usemos para nuestro cliente.

## Cliente OpenVPN
Tras instalar nuestro cliente deberemos dirigirnos a "C:/Archivos de programa/OpenVPN/config" y copiar allí el archivo de cliente creado. Para ello podemos conectarnos mediante FTP a nuestra raspberry y copipar el archivo.

Para ejecutar el programa damos al botón "Inicio de Windows - Barra de Programas - OpenVPN - OpenVPN GUI".

*img15*

Vamos a la barra de tareas, nos saldrá un icono con un candado hacemos "botón segundario - Connect".

*img16*

Introducimos la contraseña configurada para nuestro cliente y listo.

*img17*