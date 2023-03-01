## Trabajando con Shells

Tipos

- Shells comunes
- Shells en Linux
- Shells en Windows
- Creación de shells personalizados

Hay una amplia variedad de shells utilizados por todo tipo de consumidores. Algunos shells son ideales para usuarios de Windows, mientras que otros son ideales para usuarios avanzados de macOS. Debido a que los shells se usan constantemente en el campo de la seguridad de la información, comprender qué son, cómo se usan y cómo generar nuestros propios shells es una habilidad muy importante. Si bien este tema se centrará en la perspectiva del atacante, los defensores también podrán identificar patrones y características aquí.

## Common Shells

Esta unidad de aprendizaje cubre los siguientes objetivos de aprendizaje:

- Definir qué es un shell
- Listar los shells del sistema
- Usar shells de Netcat
- Comprender los shells de enlace
- Comprender los shells inversos

Antes de trabajar con shells, necesitamos comprender qué es un shell. Comenzaremos cubriendo el concepto, identificando los shells comunes y luego revisando dos tipos de shells remotos (enlace e inverso).

## ¿Qué es un Shell?

Un shell es la interfaz de usuario de un sistema informático. Puede ser una Interfaz de Línea de Comandos (CLI), que es una interfaz de usuario que solo permite escribir, o una Interfaz Gráfica de Usuario (GUI), que es una interfaz de usuario que tiene ventanas visuales y se puede interactuar con ella usando un mouse. Independientemente de qué interfaz use un sistema, se le conoce como shell. Es la capa externa del sistema informático con la que un usuario puede interactuar.

## Shells del sistema

Existen una variedad de shells que se pueden usar en un ordenador, dependiendo del sistema. Simplificaremos esto con una descripción de algunos de los shells de Windows y 'Nix más comunes.

Los shells comunes usados en los sistemas operativos de Windows son el símbolo del sistema (Command Prompt), CMD y PowerShell. El símbolo del sistema es una terminal que permite a los usuarios escribir comandos para manipular el sistema. Fue creado antes de Windows 95, pero se ha mantenido como una opción disponible en versiones más recientes. Sin embargo, las limitaciones del terminal CMD llevaron a la creación de otro terminal con una interfaz de línea de comandos (CLI) que ahora se incluye en todos los sistemas operativos de Windows actuales: PowerShell. La extensa funcionalidad de PowerShell permite el control completo de un sistema Windows.

Hay muchos terminales 'Nix disponibles en el lado de Linux, por lo que solo destacaremos algunos de los shells más comunes.

El primer shell disponible en los sistemas 'Nix fue sh, llamado el Bourne Shell. Luego, el Bourne Again Shell o bash, siguió siendo uno de los shells más utilizados hasta la fecha.

Los shells menos comunes son el tcsh, que es compatible con versiones anteriores del shell csh y se encuentra en sistemas basados en BSD. Debido a que el lenguaje de estilo C Shell (csh) se parece al lenguaje de programación C, se consideró más legible en el momento de su creación. Finalmente, el Korn Shell o ksh es compatible con versiones anteriores de csh y bash.

Recientemente, el Z Shell o zsh se incorporó como el shell predeterminado en Kali. Se trata de un shell sh extendido y contiene características de bash, csh y tcsh.

Esto puede ser trivial para los usuarios regulares, pero como profesional de la seguridad, el conocimiento de los diferentes shells es vital para identificar qué tipo de entorno de shell ejecuta un sistema.

Ahora que hemos cubierto algunos de los shells del sistema, pasemos a los shells de Netcat.

## Shells en Linux
## Objetivos de Aprendizaje:
- Conectar a un puerto TCP/UDP con Netcat
- Escuchar en un puerto TCP/UDP con Netcat
- Obtener acceso remoto a un host con Netcat
- Ejecutar un bind shell de Netcat
- Ejecutar un reverse shell de Netcat
- Netcat vs. Socat
- Socat reverse shells
- Shells SSH

## Linux Shells
Después de cubrir Netcat, examinaremos las conexiones Socat y SSH y podremos poner este conocimiento en práctica en las secciones de ejercicios.

Connecting to a TCP/UDP Port

Como se mencionó anteriormente, Netcat puede ejecutarse en modo cliente o servidor. Primero, revisemos el modo cliente.

Podemos usar el modo cliente para conectarnos a cualquier puerto TCP/UDP, lo que nos permite:

- Verificar si un puerto está abierto o cerrado.
- Leer un banner del servicio que escucha en un puerto.
- Conectarnos manualmente a un servicio de red.

Para comprender mejor cómo usar Netcat, demos una demostración rápida de cómo funciona. Comenzaremos ejecutando `nc` para verificar si el puerto TCP 110 (el servicio de correo POP3) está abierto en una máquina del laboratorio. Suministraremos varios argumentos: `-n` para saltar la resolución de nombres DNS, `-v` para agregar algo de verbosidad, la dirección IP de destino y el número de puerto de destino.

```bash
kali@kali:~$ nc -nv 10.11.0.22 110
(UNKNOWN) [10.11.0.22] 110 (pop3) open
+OK POP3 server lab ready <00003.1277944@lab>
```
El Listado 1 nos dice varias cosas. Primero, la conexión TCP a 10.11.0.22 en el puerto 110 (10.11.0.22:110 en la nomenclatura estándar) tuvo éxito, por lo que Netcat informa que el puerto remoto está abierto. A continuación, el servidor respondió a nuestra conexión "hablando con nosotros", imprimiendo el mensaje de bienvenida del servidor y esperando la cadena de inicio de sesión de nuestra parte. Este es el comportamiento estándar de los servicios POP3.
```bash
kali@kali:~$ nc -nv 10.11.0.22 110
(UNKNOWN) [10.11.0.22] 110 (pop3) open
+OK POP3 server lab ready <00004.1546827@lab>
USER offsec
+OK offsec welcome here
PASS offsec
-ERR unable to lock mailbox
quit
+OK POP3 server lab signing off.
```
