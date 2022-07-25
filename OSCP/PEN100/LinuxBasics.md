# Linux
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
```shell script
head -n 3 config.php
tail -n 3 log.txt
```
