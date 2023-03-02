# Introducción a Linux

# Que es?

Sistema operativo de código abierto(**Open Source**), permite la libre distribución y modificacion.
El **Kernel** es el principal componente del sistema operativo y el núcleo que brinda la interfaz entre el hardware y los procesos.
Un kernel monolitico es un archivo binario estatico que corre en una sola direccion(address), **GNU** es un proyecto de **Linus Torvalds** para generar su propio kernel monolitico.
El termino GNU/Linux se refiere al sistema operativo GNU que utliza el kernel de linux. La integración de Kernel, S.O., utilidades de sistema y paquetes de software, se conoce como **distribución**.

## Historia

Uno de los sistemas inspirados por Unix fue **Minix** desarrollado por **Andrew S. Tanenbaum**.

## Kali

Distribución Linux basada en **Debian** creada en 2013 por Offensive security con los siguientes propósitos:

- Pruebas de penetración
- Investigación de seguridad
- Cómputo Forense
- Ingeniería Inversa

### Tipos de Herramientas que incluye

- Recolección de información
- Análisis de vulnerabilidades
- Ataques inalambricos
- Ataques de aplicaciones web
- Herramientas de explotación
- Ataques de contraseñas
- Hacking de hardware
- Sniffing y spoofing

## Comandos básicos

Una te las formas mas comunes de interactuar con un sistema linuz es mediante la **linea de comandos** una interfaz de texto conocida como *terminal*.
También conocida como **shell** o consola es un programa que procesa comandos y regresa salidas(outputs).
Alguna de las shells mas importantes son **sh, Bash(default), ksh, zsh**

### Listado

- pwd {Muestra el directorio actual}
- cd {cambia de directorio}
- ../../ {Dos movimientos hacia atras entre directorios(rutas relativas)}
- ~ {referencia al directorio home}
- ls {listado de directorios}

Para utilizar opciones o argumentos con una comando se utiliza el simbolo(-)

- ls -la {listado de archivos en formato largo incluyendo ocultos}

Directorio raíz **/**
Los archivos ocultos iniciacon con un **.**

### Lectura del contenido

- cat {muestra el contenido de un archivo, la opcion -n muestra las lineas}
- more {muestra archivos de texto a lo largo de una pantalla(los corta si son muy grandes)}
- less {version mas completa de more, la opcion +F mucestra cambios en un archivo(útil par ver logs)}
- head {permite imprimir el top N de lineas de un archivo}
- tail {muestras las ultimas N lineas }
ejemplos:

```bash
head -n 3 config.php
tail -n 3 log.txt
```

## Manuales y ayuda

El comando man o la opcion -h —help existe en la mayoría de los comandos o programas disponibles.

ejemplos:

```bash
more -h
man more
man -k '^passwd$' ##busqueda de comandos que contengan la exp reg passwd
```

## Linux Filesystem

FHS Filesystem Hierarchy Standard es el estándar que define el propósito de la estructura de carpetas de un sistema Linux.

/bin/: Programas básicos

/boot/: archivos del Kernel y otros necesarios para el arranque del sistema

/dev/: archivos de dispositivos

/etc/: archivos de configuración

/home/: carpeta personal

/lib/: librerías básicas

/media/: dispositivos removibles(CD, USB, etc)

/mnt/ or /mount/: Puntos temporales de montaje

/opt/: aplicaciones extra de terceros

/root/: carpeta personal del root

/run/: información volátil que no persiste despues de un reinicio

/sbin/: programas del sistema

/srv/: datos usados por el servidor

/tmp/: archivos temporales

/usr/: aplicaciones

/var/: datos de los servicios como logs, colas, cache, etc

/proc/ y /sys/ propias de cada sistema

## Entorno

Las variables en la consola se asignan con (=). Para conocer las variables de entorno se usa el comando **set** 

El comando **history** nos devuleve el historial de comandos de un usario de acuerdo al bash que utilice.

```bash
kali@kali:~$history
example_command1
example_command2
example_command3
...

kali@kali:~$cat ~/.zsh_history
example_command1
example_command2
example_command3
...
```

El comando alias permite crear un sobrenombre para algun comando con argumentos

```bash
kali@kali:~$ alias ll='ls -la'
kali@kali:~$ ll
total 40
drwxrwxrwt  10 root   wheel   320 Apr 24 13:19 .
drwxr-xr-x   6 root   wheel   192 Mar 18 20:10 ..
-rw-r--r--@  1 kali   wheel     0 Apr 21 19:56 MozillaUpdateLock-2656FF1E876E9973
-rw-r--r--   1 kali   wheel    10 Apr 21 15:18 a
```

unalias deshace el alias seleccionado. Para identificar los alias q han sido asignados se puede usar el comando `vim ~/.bashrc`  dependiendo el shell q se este usando.

### Información del sistema

Una forma de conocer la version durante la enumeración de un sistema linux es

```bash
kali@kali:~$ cat /etc/issue
Kali GNU/Linux Rolling \n \l
```

Tambien el comando `uname -a` nos dará mas información sobre la versión 

## Administración de archivos

El comando touch crea un archivo en blanco

```bash
touch newfile
ls -l
```

Otros comandos de administración son

rm - elimina archivos

mkdir - Crea directorio, -p para crear un arbol de directorios

rmdir - Elimina directorio

mv - mover archivo o renombrar si se hace en la misma ruta

cp - copiar archivos

ln - crea un link hacia otro archivo. default hard link(persiste), -s softlink

Ej `ln -s ~/original.txt symlink.txt`

![Untitled](Introduccio%CC%81n%20a%20Linux%203790c1122a164da7b0fda5f0f1c66d38/Untitled.png)

El asterisco(*) puede usarse para representar cero y mas caracteres y ser usado con los comandos anteriores para afectar mas de un archivo a la vez.

```bash
kali@kali:~$ls -l a*
-rw-r--r--  1 root  wheel    515 Jun  6  2020 afpovertcp.cfg
lrwxr-xr-x  1 root  wheel     15 Mar  9 20:49 aliases -> postfix/aliases
-rw-r-----  1 root  wheel  16384 Jun  6  2020 aliases.db
-rw-r--r--  1 root  wheel   1051 Jun  6  2020 asl.conf
-rw-r--r--  1 root  wheel    149 Oct 30 08:55 auto_home
-rw-r--r--  1 root  wheel    195 Oct 30 08:55 auto_master
-rw-r--r--  1 root  wheel   1935 Oct 30 08:55 autofs.conf
...
```

ejemplo real: `ls *[0-9].pdf` listar archivos q contengan al final un numero y con extension pdf.

### Búsqueda de archivos

Los comandos find, locate y which sirven para este fin. Siendo locate el mas rapido que almacena con una tarea automatizada un base de datos ubicada en locate.db

```bash
kali@kali:~$echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

kali@kali:~$which sbd/usr/bin/sbd
```

Por otro lado find trabaja de manera jerarquica y busca recursivamente, tiene argumentos como -name, -iname, -size, etc

ej: **`find / -name texto.txt -type f`**

busca de manera recursiva desde root / el archivo text.txt

**`find / -user kali -size 1M -type f -name "*.txt"`**

Busca todos los .txt del usuario kali q pesan mas de 1Mb