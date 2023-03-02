# Windows

# Diferencias con Linux

Administrador VS root. Conocido como ***NT AUTHORITY\SYSTEM**,* es el rol que nos indica en Windows los privilegios mas altos.

FileSystem. Se usan rutas para indicar los device como C:/ D:/ etc

Paths. Windows utiliza (\) para separar directorios y no es case-sensitive.

/etc/config VS registry. Linux divide sus configuraciones en distintos archivos mientras que Windows los administra en el registry.

El comando `type` nos permite ver en consola el contenido de un archivo de texto

## Shells

Existen 3 programas diferentes para trabajar con la consola cmd, powershell y wmic

**cmd.exe** esta orientada a la creación y manipulación de archivos, navegación entre directorios y ejecución de scripts(batch files).

**powershell** diseñada para la automatización de procesos, soporta el lenguaje de scripting y un framework de administración de configuración

**wmic** soporta scripts de visual basic(VBscript) permite compartir información entre aplicaciones.

## Ayuda y comandos builtin

Para invocar la ayuda de un comando se usa `help` o `/?` 

[Comandos comunes](Windows%20230f619ac7ff4a678bf16be8b12d6f30/Comandos%20comunes%2036be3ebe49de4850b603eb9cf7542258.md)

`dir /A` muestra archivos ocultos

`dir /o:d /t:w` muestra y ordena los archivos por fecha de modificación

## Estructura de directorios

**\PerfLogs**: usualmente vacio, contiene datos de rendimiento del equipo(logs)

**\Program Files**: en un equipo de 32-bits es la única carpeta q contiene los programas de 32bits y en equipos de 64 almacena los programas de 64bits

**\Program Files (x86)**: aplica a sistemas de 64bits y sirve para almacenar programas de 32bits

**\ProgramData**: directorio oculto al que tienen acceso programas aun sin estar logueado.

**\Users**: se crea para cada usuario que se loguea en el equipo por lo menos una vez.

**\Users\Public**: directorio de acceso publico

**\Users\[UserName]\AppData**: directorio oculto que almacena los datos y configuraciones de las aplicaciones de cada usuario. Incluye 3 subcarpetas \Roaming, \Local, y LocalLow. La subcarpeta ../local/temp elimina su contenido despues de cada reincio.

**\Windows**: archivos del sistema operativo.

**\System, \System32, and \SysWOW64**: almacenan las DLLs.

## Información de sistema y variables de entorno

**systeminfo** devuelve información sobre el sistema

**set** muestra todas las variables de entorno, **setx** modifica una variable de manera permanente

**%variable%** muestra una variable en específico

[Variables de entorno](Windows%20230f619ac7ff4a678bf16be8b12d6f30/Variables%20de%20entorno%20db99cbde209e40fa92a394dbb269f181.md)

`setx path variable /m` permite agregar o modificar una variable permanente de sistema

para conocer cuales son los últimos parches instalados podemos buscar en la sección de hotfix del resultado de `systeminfo`

**Sysinternals** no viene por default y se puede descargar directo de microsoft, nos brinda una suite de herramientas utiles para obtener mas información del sistema como psinfo

## Búsqueda de archivos y texto

El comando `dir /s` nos permite realizar la búsqueda de un archivo `/p` nos permite pausar el output y el comando `tree` muestra el árbol de directorios. El comando forfiles también nos permite buscar entre directorios, ej:

```bash
C:\Users\Offsec>forfiles /P C:\Windows /S /M notepad.exe /c "cmd /c echo @PATH"
...
"C:\Windows\SysWOW64\notepad.exe"
ERROR: Access is denied for "C:\Windows\SysWOW64\Com\dmp\".
ERROR: Access is denied for "C:\Windows\SysWOW64\config\".
ERROR: Access is denied for "C:\Windows\SysWOW64\Configuration\".
ERROR: Access is denied for "C:\Windows\SysWOW64\FxsTmp\".
ERROR: Access is denied for "C:\Windows\SysWOW64\Msdtc\".
ERROR: Access is denied for "C:\Windows\SysWOW64\networklist\".
ERROR: Access is denied for "C:\Windows\SysWOW64\sru\".
ERROR: Access is denied for "C:\Windows\SysWOW64\Tasks\".
```

`/P` indica el path desde donde empieza la búsqueda, `/S` indica que la búsqueda es recursiva, `/M` indica que buscamos y `/C` ejecuta algún comando

`find` sirve para buscar un archivo y no soporta expresiones regulares

`findstr` soporta expresiones regulares y  nos permite buscar texto dentro de los archivos

( `|` ) nos sirve como grep en linux

`sort /R` nos permite ordenar el contenido de un archivo de texto

ejemplos:

`findstr /n /r . <ruta> | find "101:"` enumera las líneas e indica las q empiezan con 101:

`comando | find /c /v "”`  numera las líneas de la salida

## Principios de seguridad

**SID** valor unico usado para identificar con seguridad la autenticacióon de un sistema

**net** permite administrar usuairios

**Access token** administra permisos

**ACL** listas de control de acceso

## Tipos de cuentas

**Administrator** es la cuenta con todos los permisos SID S-1-5-domain-500

**Guest** es una cuenta deshabilitada por default con restricciones SID S-1-5-32-546

**System** cuenta con todos los privilegios pero no disponible para loguearse

`net user /add Tristan greatpassword`

`net localgroup Administrators Tristan /add`

**UAC (user account control)** bajo el principio del menor privilegio es un control para evitar ataques atomatizados, algunos comando lanzan un prompt para validar la ejecución humana

💀**el comando runas y cmd /c es usado por los atacantes para elevar privilegios, funciona de manera similar a sudo. Ej `runas /user:usuario cmd`**

## Permisos NTFS

**Read**: permite a los usuarios ver el contenido de un archivo, también permite la ejecución de scripts

**Write**: Permite la escritura de un archivo, no permite eliminarlo pero si eliminar el contenido

**Read & Execute**: Permite al usuario leer un archivo y la ejecución de binarios

**Modify**: Permite leer y escribir un archivo, así como eliminarlo

**Full Control**: Control total

Dichos permisos también son aplicables a directorios donde un permiso de directorio será heredado hacía su contenido

Para la gestión de permisos usamos el comando `icacls` donde las opciones mas comunes serán `/T` para ejecutar la acción de manera recursiva, `/C` para forzar una operación, `/L` se usa en links para que solo afecte al link

La herramienta **accesschk.exe** de sysinternals  nos permite auditar la información relacionada con los permisos.

ejemplo: 

`icacls archivo /T / C`

`icacls archivo /grant usario:(*permiso*)`

## Procesos

Un proceso es el contendor en el cual un programa se ejecuta, mientras que un hilo(thread) es una instancia de ese programa.

Existen dos tipos de procesos de usuario y de kernel, donde el segundo tiene acceso directo al hardware.

Los procesos cuentan con herencia.

**smss.exe** es el administrador de sesión y el primer proceso de usuario que inicia con Windows

**winlogon.exe** ademas de autenticar usuarios, sirve para cargar los perfiles, esta en constante escucha del famoso `ctrl+alt+supr`. uno de sus hijos es **userinit.exe** que da el shell inicial y a su vez inicia a **explorer.exe** responsable del ambiente gráfico GUI.

**csrss.exe** client server runtime process es el responsable de varias tareas en segundo plano, inicia la secuencia de apagado y es el padre de cada cmd.exe

**wininit.exe** crucial para la ejecución de diversas tareas de inicio

`tasklist` lista las tareas en ejecución

```bash
C:\>tasklist /fi "USERNAME eq NT AUTHORITY\SYSTEM" /fi "STATUS eq running"

Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
csrss.exe                      788 Console                    1      5,448 K
NVDisplay.Container.exe       2892 Console                    1     46,768 K
rundll32.exe                  5388 Console                    1      7,280 K
...
```

muestra los procesos que son ejecutados en el contexto de system

`taskkill` permite detener un proceso

`wmic process get processid,parentprocessid,executablepath|find “PID”` permite identificar al proceso padre

`pslist -d` muestra los hilos que se estan ejecutando, se requiere de sysinternals `-t` muestra un árbol

`pssuspend` suspende un proceso y con -r lo reanuda

`listdlls -u` lista las dlls que están en ejecución es útil para identificar anomalías

nota: **para el primer uso de cualquier comando de sysinternals se requiere el parametro `/accepteula`**

## Registro (windows registry)

Desde una perspectiva de seguridad, el registro contiene información crítica del sistema. Se puede acceder a información de usuario y del sistema así como a que aplicaciones y servicios pueden ejecutarse en el sistema.

La unidad básica del registro se conoce como llave (key) y cuenta con una estructura jerárquica, misma que contiene valores(value) y cada uno cuenta con nombre (name), data type y data content.

**HKEY_CLASSES_ROOT (HKCR)**: Provee información relacionada con los tipos de datos y sus propiedades, las subllaves son usadas usualmente por el shell(cmd y COM).

**HKEY_CURRENT_CONFIG (HKCC)**: Provee información del hardware.

**HKEY_CURRENT_USER (HKCU)**: Infromación de las configuraciones de los usuarios.

**HKEY_LOCAL_MACHINE**: Información de dispositivos de entrada/salida.

**HKEY_PERFORMANCE_DATA**: Rendimiento del sistema.

**HKEY_USERS** Información por default asignada a nuevos usuarios.

 Para una mejor administración de llaves y valores se usa el concepto hive, cada hive tiene un archivo asociado: 

[Default hives](Windows%20230f619ac7ff4a678bf16be8b12d6f30/Default%20hives%2003ed6fa1852f499f9637c2b9f7e5913d.md)

Bajo los hives

**HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run** 

**HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\RunOnce**

**HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run** 

**HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce**

Se ejecutan acciones bajo la autenticación de algún usuario, Run Once elimina la acción una vez ejectutada.

Para la gestión de los registros desde cmd se usa el comando `reg` y desde la GUI usamos **regedit**

Ejemplos:

```bash
C:\>reg query hkcu\software\microsoft\windows\currentversion\runonce

C:\>reg query hkcu\software\microsoft\windows\currentversion\run

HKEY_CURRENT_USER\software\microsoft\windows\currentversion\run
    OneDrive    REG_SZ    "C:\Users\OffSec\AppData\Local\Microsoft\OneDrive\OneDrive.exe"
```

uso de `reg query` para buscar los registros run y runonce- REG_SZ indica que no hay un valor asignado y que este debe ser texto

```bash
C:\>reg delete hkcu\software\microsoft\windows\currentversion\run /va
Delete all values under the registry key HKEY_CURRENT_USER\software\microsoft\windows\currentversion\run (Yes/No)? yes
The operation completed successfully.
```

eliminado de registro, la opción `/va` remueve todos los valores de la llave

```bash
C:\>reg add hkcu\software\microsoft\windows\currentversion\run /v OneDrive /t REG_SZ /d "C:\Users\Offsec\AppData\Local\Microsoft\OneDrive\OneDrive.exe"
The operation completed successfully.
```

agregar registro, la opción /t indica el tipo de dato, /v nombrar el valor, /d los datos que contiene

Por últmo reg export y reg import, permite exportar e importar las llaves entre equipos.

💀**Es posible exportar el los hashes de la contraseña del registro, exportando  la llave de la SAM y de SYSTEM.**

## Tareas (Scheduled tasks)

Similar a cronjobs, el comando `schtasks` permite programar tareas repetitivas en Windows.

```bash
C:\>schtasks /create /sc weekly /d mon /tn runme /tr C:\runme.exe /st 09:00
SUCCESS: The scheduled task "runme" has successfully been created.
```

```
MINUTE:  1 - 1439 minutes.
    HOURLY:  1 - 23 hours.
    DAILY:   1 - 365 days.
    WEEKLY:  weeks 1 - 52.
    ONCE:    No modifiers.
    ONSTART: No modifiers.
    ONLOGON: No modifiers.
    ONIDLE:  No modifiers.
    MONTHLY: 1 - 12, or
             FIRST, SECOND, THIRD, FOURTH, LAST, LASTDAY.
```

## Utilidades de disco

```bash
C:\>fsutil fsinfo drives

Drives: C:\ F:\ Z:\
```

```bash
C:\>fsutil fsinfo drivetype C:
C: - Fixed Drive
```

Una utilidad de windows que permite ocultar malware son los ADS alternate data streams-

Ejemplos:

`echo fileTwo uses the 'test' stream > testStream.txt:offsec`

para listar `dir /r`  

para leer el contenido `more < offsecStream.txt:offsec`

adicional en sysinternarls hay dos herramientas utiles para el mismo fin `du` y `streams`