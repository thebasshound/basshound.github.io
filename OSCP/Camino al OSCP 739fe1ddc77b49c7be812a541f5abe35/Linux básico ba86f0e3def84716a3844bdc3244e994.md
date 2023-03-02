# Linux básico

# Piping (|) y redirección

Existen 3 flujos de datos(data streams) en linux:

- STDIN (0) La entrada estándar alimentada en un programa
- STOUT(1) La salida que imprime un programa
- STERR (2) Los errores que se generan en algun programa

El simbolo(>) permite redirigir la salida de un comando hacia otro

EJ:

```bash
kali@kali:~$echo hello
hello

kali@kali:~$echo hello > file.txt

kali@kali:~$cat file.txt
hello
```

Los simbolos (>>) permiter agregar datos a un archivo existente

Por otro lado el simbolo (<) permite leer un archivo y usarlo como entrada para un comando

ej

```bash
kali@kali:~$wc -m < test.txt
89
```

Para el envio de errores a un archivo podemos utilizar el operador (2>)

Para usar el resultado de un comando y aplicarlo como entrada de otro con | 

```bash
kali@kali:~$ls -ltotal 32
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Desktop
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Documents
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Downloads
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Music
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Pictures
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Public
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Templates
drwxr-xr-x 2 kali kali 4096 Jun 23 15:09 Videos

kali@kali:~$ls -l | wc -l
9
```

En el path /dev/null se puede enviar toda aquella salida q no nos sea útil

## Manipulación de texto

Los comandos grep, sed, cut y awk sirven para la búsqueda y manipulación de texto. El uso de expresiones regulares permite realizar manipulaciones mas complejas. 

**grep** permite la búsqueda de texto de acuerdo a expresiones regulares en una salida de la terminal. **-r** permite q sea recursivo

**sed** es un editor de streams, útil para el intercambio  de letras o palabras

```bash
kali@kali:~$echo "I need to try hard" | sed 's/hard/harder/'
I need to try harder
```

**cut** permite extraer una sección de texto, la opción **-f** permite elegir un campo(columna) y **-d** indicar un delimitador.

```bash
kali@kali:~$cut -d ":" -f 1 /etc/passwd
root
daemon
bin
sys
sync
games
...
```

**awk** es un lenguaje de programación para el procesamiento de texto. La opción **-F** es un separador de campos y el subcomando **print** imprime el resultado

```bash
kali@kali:~$echo "hello::there::friend" | awk -F "::" '{print $1, $3}'
hello friend
```

Tip: con el comando `tr -d '\n'` se pueden eliminar los saltos de línea de una salida 

## Comparar archivos

**comm** compara dos archivos de texto el parámetro -n(donde es un #) permite suprimir n columnas al comparar. Donde 1 es la la diferencia del archivo 1, 2 del 2 y 3 las iguales

```bash
kali@kali:~$cat scan-a.txt
192.168.1.1
192.168.1.2
192.168.1.3
192.168.1.4
192.168.1.5

kali@kali:~$cat scan-b.txt
192.168.1.1
192.168.1.3
192.168.1.4
192.168.1.5
192.168.1.6

kali@kali:~$comm scan-a.txt scan-b.txt
								192.168.1.1
192.168.1.2
								192.168.1.3
								192.168.1.4
								192.168.1.5
			  192.168.1.6

kali@kali:~$comm -12 scan-a.txt scan-b.txt
192.168.1.1
192.168.1.3
192.168.1.4
192.168.1.5
```

**diff** es un comparador mas complejo con dos tipos de formatos de salida

```bash
kali@kali:~$diff -c scan-a.txt scan-b.txt
*** scan-a.txt	2018-02-07 14:46:21.557861848 -0700
--- scan-b.txt	2018-02-07 14:46:44.275002421 -0700
***************
*** 1,5 ****
  192.168.1.1
- 192.168.1.2
  192.168.1.3
  192.168.1.4
  192.168.1.5
--- 1,5 ----
  192.168.1.1
  192.168.1.3
  192.168.1.4
  192.168.1.5
+ 192.168.1.6

kali@kali:~$diff -u scan-a.txt scan-b.txt
--- scan-a.txt	2018-02-07 14:46:21.557861848 -0700
+++ scan-b.txt	2018-02-07 14:46:44.275002421 -0700
@@ -1,5 +1,5 @@
 192.168.1.1
-192.168.1.2
 192.168.1.3
 192.168.1.4
 192.168.1.5
+192.168.1.6
```

## Usuarios y grupos

El esquema mas común de almacenamiento de usuario es en

 `/etc/passwd` este archivo puede ser accedido por diversas aplicaciones y contiene el nombre y id de los usuarios

 `/etc/shadow` este archivo solo puede ser accedido por usuarios con altos privilegios y contiene la huella digital de la contraseña

ejemplo shadow:

`root**:**$6$pfiZTzNB1wav3OFG$GDwbvI44D7sBuX7Q.6LmNWx.RaU6nzxZWCCkkMNIXCkvANnNoYogV983NSLkG1cfpaW4mmyFuTOKkDf53hVkh/**:**18781**:**0**:**99999**:**7**:::**`

estructura **shadow:**

usuario **:** contraseña encriptada : ultimo cambio de contraseña(timestamp) : tiempo minimo en dias para cambio de contraseña : máximo de dias para cambio : indica el # de días para avisar antes al usuario de que debe cambiar la contraseña

ejemplo **passwd**:

`john**:**x**:**1002**:**1002**:**John Doe,,,**:**/home/john**:**/bin/bash`

nombre de usuario : x indica que la contraseña debe ser obtenida del archivo passwd : user ID (UID) : group ID (GID) : Campo opcional para comentarios, usualmente los datos del dueño de la cuenta : ruta del directorio home : ruta del bash usado

El usuario **UID = 0** identifica al root, este valor puede asignarse manualmente a otro usuario pero no es recomendado.

Los comando `usermod -L username` y `passwd -l username` permite el bloqueo de una cuenta asignando el simbolo `!` al inicio de hash de la contraseña en el archivo /etc/shadow. Tambien colocando -E en el octavo campo forzara la expiración de la cuenta. Por último cambio el shell en `passwd` por `/bin/false` o `/sbin/nologin` bloqueara el acceso. `usermod -s` permite cambiar el shell

```bash
root@kali:~#passwd --status jane
janeL 03/15/2021 0 99999 7 -1

root@kali:~#chage -l jane
Last password change                                    : Mar 15, 2021
Password expires                                        : never
Password inactive                                       : never
Account expires                                         : never
Minimum number of days between password change          : 0
Maximum number of days between password change          : 99999
Number of days of warning before password expires       : 7
```

```bash
root@kali:~#grep ^jane /etc/passwd
jane:x:5001:5001::/home/jane:/sbin/nologin
```

Los grupos permite a los administradores de sistemas una configuracion más flexible y granular, se almacenan en /etc/groups

`bluetooth**:**x**:**117**:**kali`

nombre del grupo : x (no se usa) : ID del grupo : usuario que pertenece al grupo.

## Contexto usuarios

El comando `sudo` permite a elevar privilegios a un usuario no privilegiado, el archivo de configuración se encuentra en `/etc/sudoers`

En este archivo se pueden configurar las aplicaciones que se permiten ejecutar con `sudo`, la opción `-l` lista los comandos permitidos por usuario. La opción `-i` permite iniciar una shell como root.

El comando `su` permite cambiar de usuario. Para ejecutar un comando como otro usuario podemos usar el comando `su -l usuario -c “comando”`

para añadir un usuario al grupo sudoers usamos el comando `sudo usermod -aG sudo usuario`

💀Durante la elevación de privilegios validar si algún usuario puede ejecutar sudo sin password dentro de /etc/sudoers o `sudo -l`

![Untitled](Linux%20ba%CC%81sico%20ba86f0e3def84716a3844bdc3244e994/Untitled.png)

## Permisos

Cada archivo cuenta con 3 categorias de permisos u usuario, g grupo y o otros.

Hay 3 tipos de permisos r lectura, w escritura y x ejecución

`ls -l` nos muestra los permisos dentreo de un directorio

`chown` nos permite alterar a quién pertenece un archivo por ejemplo **`u=rwx,g+rw,o-r`**  asigna al usuario permisos de lectura, escritura y ejecución, al grupo le agrega lectura y escritura y a otros le quita la lectura. Tambien con la letra a={permisos} se puede asignar a todos los mismo permisos.

`chmod` nos permite cambiar los permisos de un archivo, se puede simplificar el cambio usando los siguientes valores:

- 4 for read
- 2 for write
- 1 for execute
- 7 = 4 + 2 + 1 = read, write, and execute
- 6 = 4 + 2 = read and write
- 5 = 4 + 1 = read and execute
- 3 = 2 + 1 = write and execute

0 representa sin permisos

ejemplo: `chmod 754` = `rwxr-xr--`

         `chmod 640` = `rw-r-----`

Para cambiar todos los archivos y subcarpetas de manera recursiva se usa -R, ej: `chmod -R o=x /carpeta` cambiaria para todos los archivos el permiso de ejecución dentro de la carpeta

## Setuid y setgid

Permite adquirir los permisos de un grupo o usuario (s).

```bash
kali@kali:~$ls -la /usr/bin/passwd
-rw**s**r-xr-x 1 root root 63960 Feb  7  2020 /usr/bin/passwd
```

💀**los pentesters buscan este tipo de archivos para escalar privilegios**

### Sticky

Este bit suele asignarse en directorios a los que todos los usuarios tienen acceso, y permite evitar que un usuario pueda borrar archivos/directorios de otro usuario dentro de ese directorio, ya que todos tienen permiso de escritura

💀 `find / -perm 4644` es de utilidad para encontrar archivos con setuid activo

## Procesos

Es una instancia de un programa en ejecución. Cada que se ejecuta en la terminal un programa se inicia un nuevo proceso. El kernel los administra mediante un ID (**PID**).

Con el simbolo **&** podemos ejecutar tareas en segundo plano, una tarea (job) puede tener más de un proceso. `kali@kali:~$ **ping -c 400 localhost > ping_results.txt &**` con **ctrl+z** podemos dejar la consola en en segundo plano y para mostrar los procesos que estan en segundo plano usamos el comando `bg`. Ejemplo:

```bash
kali@kali:~$ping -c 400 localhost > ping_results.txt^Z
[1]+  Stopped                 ping -c 400 localhost > ping_results.txt

kali@kali:~$bg
[1]+ ping -c 400 localhost > ping_results.txt

kali@kali:~$
```

Por otro lado con `fg` podemos regresar la tarea al primer plano-

`ps` lista los procesos en ejecución

💀**Una de las primeras tareas una vez obtenido un acceso remoto es entender que software se esta ejecutando en la máquina victima, con la finalidad de recolectar información adicional para elevar privilegios y mantener el acceso.**

```bash
kali@kali:~$ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
root          1      0  0 10:18 ?        00:00:02 /sbin/init
root          2      0  0 10:18 ?        00:00:00 [kthreadd]
root          3      2  0 10:18 ?        00:00:00 [rcu_gp]
root          4      2  0 10:18 ?        00:00:00 [rcu_par_gp]
root          5      2  0 10:18 ?        00:00:00 [kworker/0:0-events]
root          6      2  0 10:18 ?        00:00:00 [kworker/0:0H-kblockd]
root          7      2  0 10:18 ?        00:00:00 [kworker/u256:0-events_unbound
root          8      2  0 10:18 ?        00:00:00 [mm_percpu_wq]
root          9      2  0 10:18 ?        00:00:00 [ksoftirqd/0]
root         10      2  0 10:18 ?        00:00:00 [rcu_sched]
...
```

-ef muestra todos los proceso y los muestra en formato completo

```bash
kali@kali:~$ps -fC leafpad
UID         PID   PPID  C STIME TTY          TIME CMD
kali1307    938  0 10:57 ?        00:00:00 leafpad
```

-fC busca el proceso ejecutado por un comando

```bash
kali@kali:~$kill 1307

kali@kali:~$ps aux | grep leafpad
kali       1313  0.0  0.0   6144   888 pts/0    S+   10:59   0:00 grep leafpad
```

`kill` permite detener un proceso

`ps aux | sort -rnk 4 | head -5` identifica los 5 procesos que consumen mas memoria

para forzar detener un proceso podemos usar `pkill -9 **proceso**`

## Monitoreo

Con el comando `tail` podemos monitorear un archivo en constante cambio. Usando la opcion `-f` se actualiza en tiempo real, la opción `-n X` nos muestra las ultimas X lineas.

`watch` tambien nos permite ver un archivo refrescado cada 2 segundos y con `-n`  podemos cambiar el intervalo

## Aplicaciones

APT es un conjunto de herramientas para la administración de paquetes y aplicaciones en sistemas Ubuntu y Debian. dpkg también sirve para instalar paquetes *.deb.

## Programación de tareas

`crontab` permite la ejecución de tareas programadas en /etc/cron.*

`crontab -e` muestra las tareas programadas de un usuario

💀**Una débil administración de permisos en cron permitiría una escalada de privilegios**

```bash
kali@kali:~$cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
5 0 * * * root /var/scripts/user_backups.sh
#
```

## Logs

```bash
kali@kali:~$who /var/log/wtmp | tail -5
robert   tty7         2021-07-03 09:46 (:0)
nichole  tty7         2021-07-03 11:53 (:0)
kali     tty7         2021-07-06 08:04 (:0)
robert   tty7         2021-07-06 15:09 (:0)
kali     tty7         2021-07-07 14:03 (:0)
```

muestra los usuarios que se han logueado al equipo

**journalctl** muestra los logs

el comando `systemctl get-default` nos devuleve el [run level](https://www.tecmint.com/change-runlevels-targets-in-systemd/) que este ejecutando la sesión

el comando `journalctl -u apache2` nos permite ver errores generados por una aplicación en especifico, en este ejemplo apache

## Administración de discos

`free -m` nos muestra la memoria disponible en el sistema

`df -h` (disk free) reporta el espacio disponible en un disco montado en el archivo system

`dd` copia un disco

`du -hs` determina el tamaño de archivos y directorios

`df -T` muestra el tipo de filesystem

`mount -t` muestra el filesystem sobre el que esta montada una ruta

`fdisk -l` nos da información sobre un disco, útil al colocar una USB

ejemplo de como montar una USB

```bash
kali@kali:~$sudo mkdir /mnt/usb

kali@kali:~$sudo mount /dev/sdb1 /mnt/usb

kali@kali:~$cd /mnt/usb

kali@kali:/mnt/usb$ls -la
total 2100
drwxr-xr-x 3 root root   4096 Dec 31  1969 .
drwxr-xr-x 3 root root   4096 Jul  8 11:12 ..
-rwxr-xr-x 1 root root 817959 Feb 16 10:45 IMG_3396.jpg
-rwxr-xr-x 1 root root 739123 Feb 16 10:45 IMG_3794.jpg
-rwxr-xr-x 1 root root 426757 Feb 16 10:45 IMG_5042.jpg
-rwxr-xr-x 1 root root 130553 Feb 18  2019 Info.hex
```

para desmontar una USB usaremos:

```
kali@kali:/mnt/usb$sudo umount /mnt/usb
umount: /mnt/usb:target is busy.

kali@kali:/mnt/usb$cd ~

kali@kali:~$sudo umount /mnt/usb
```

`du -h <dir> 2>/dev/null | grep '[0-9\.]\+G’` nos muestra las carpetas con mayor uso de espacio en disco

`sudo mount -o loop flag.img /mnt/flag` monta una imagen en una carpeta