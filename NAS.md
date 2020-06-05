# NAS

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