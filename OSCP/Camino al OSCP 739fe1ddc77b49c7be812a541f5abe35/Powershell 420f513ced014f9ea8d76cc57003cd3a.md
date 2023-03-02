# Powershell

## Ayuda

`PS C:\WINDOWS\system32> Get-Help Get-Help`

`PS C:\WINDOWS\system32> Get-Command -Verb out,edit,export -Noun *file*`

## Variables

`PS C:\WINDOWS\system32> **$firstVar = "OffSec"**`

Tipos de dato

```powershell
PS C:\WINDOWS\system32>$myString = "ABC123!@#"

PS C:\WINDOWS\system32>$myString
ABC123!@#

PS C:\WINDOWS\system32>$myString.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     TrueString                          System.Object
PS C:\WINDOWS\system32>$myInt = 1903

PS C:\WINDOWS\system32>$myInt
1903

PS C:\WINDOWS\system32>$myInt.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     TrueInt32                           System.ValueType
PS C:\WINDOWS\system32> $a = 100.5

PS C:\WINDOWS\system32> $a.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Double                          System.ValueType
PS C:\WINDOWS\system32> $b = 5000000000

PS C:\WINDOWS\system32> $b.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Int64                           System.ValueType
PS C:\WINDOWS\system32> $array1="blue","black","yellow","white","orange"

PS C:\WINDOWS\system32> $array1.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Object[]                        System.Array
PS C:\WINDOWS\system32> $False.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Boolean                         System.ValueType

PS C:\WINDOWS\system32> $True.GetType()

IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     Boolean                         System.ValueType
```

## Cast ejemplo

```powershell
PS C:\WINDOWS\system32>[int]$b + $a
579
```

## Variables especiales

- **$Error** contains an array of error objects.
- **$Host** contains information about the current hosting application.
- **$Profile** contains the path to the current user profile for PowerShell.
- **$PID** contains the process ID of the current PowerShell session.
- **$PSUICulture** contains the UI culture or the regional language of the user interface.
- **$NULL** contains the value of NULL.
- **$False** contains the value of False.
- **$True** contains the value of True.

## Comparaciones

PERATOR TYPE	OPERATOR	DEFINITION
Equality	-eq	Equal
Equality	-ne	Not Equal
Equality	-gt	Greater than
Equality	-ge	Greater than or Equal to
Equality	-lt	Less than
Equality	-le	Less than or Equal to
Matching	-like	compares strings using regular expression
Matching	-notlike	compares strings using regular expression
Matching	-match	compares strings using regular expression
Matching	-notmatch	compares strings using regular expression
Containment	-contains	searches value to see if it exists or not
Containment	-in	searches value to see if it exists or not
Containment	-notcontains	searches value to see if it exists or not
Containment	-notin	searches value to see if it exists or not
Replacement	-replace	replaces part or all of the value
Comparison	-is	compares data types (not values)
Comparison	-isnot	compares data types (not values)

## Ciclos

```powershell
PS C:\WINDOWS\system32>for ($myVar=0; $myVar -lt 5; $myVar++)
{
  Write-Output $myVar;
}0
1
2
3
4
PS C:\WINDOWS\system32> foreach ($myLetter in $myArray)
{
  $myLetter
}
p
o
w
e
r
s
h
e
l
l
PS C:\WINDOWS\system32> while ($count -lt 5)
{
  $count
  $count++
}
0
1
2
3
4
PS C:\WINDOWS\system32> do
{
  $count;
  $count++;
} while ($count -lt 5)
0
1
2
3
4
```

💀**Durante una enumeración el atacante usará Get-Service para identificar posibles servicios vulnerables**

Ejemplos de filtrado y formateo

`PS C:\WINDOWS\system32> **Get-Service | Select-Object -Property DisplayName,ServiceType,StartType,Status | Sort-Object -Property Status -Descending**`

`PS C:\WINDOWS\system32> **Get-Service | Select-Object -Property DisplayName,ServiceType,StartType,Status | Sort-Object -Property Status -Descending | Where-Object StartType -EQ Automatic**`

`PS C:\WINDOWS\system32> **Get-Service | Select-Object -Property DisplayName,ServiceType,StartType,Status | Sort-Object -Property Status -Descending | Where-Object StartType -EQ Automatic | Format-List**`

`PS C:\WINDOWS\system32> **Get-Service | Select-Object -Property ServiceName,DisplayName,ServiceType,StartType,Status | Sort-Object -Property Status -Descending | Where-Object {$_.StartType -EQ "Automatic" -And $_.ServiceName -Match "^s"} | Format-Table**`

## Funciones

```
function <name>
{
  param ([type]$parameter1, [type]$parameter2)
  <actions>
}

function <name> ([type]$parameter1, [type]$parameter2)
{
  <actions>
}
```

```powershell
PS C:\Windows\system32>function Get-PSMultiplication
{
  param ([int]$num1, [int]$num2)
  return $num1*$num2
}

PS C:\Windows\system32>Get-PSMultiplication 2 10
20
```

## Scripts

Para la ejecución de scripts se requiere la siguiente linea

`PS C:\Users\vandelay> **powershell.exe -exec bypass C:\Users\vandelay\Desktop\computerInfo.ps1**`

## Ejercicios

Identificar si hay servicios con contraseñas almacenadas

![Untitled](Powershell%20420f513ced014f9ea8d76cc57003cd3a/Untitled.png)

se deben encontrar los valores DefaultDomainName" y "DefaultUserName”

Obtener el SID del admin

![Untitled](Powershell%20420f513ced014f9ea8d76cc57003cd3a/Untitled%201.png)

Identificar si hay un producto de AntiVirus ejecutandose

`Get-CimInstance -Namespace root/SecurityCenter2 -ClassName AntivirusProduct`

![Untitled](Powershell%20420f513ced014f9ea8d76cc57003cd3a/Untitled%202.png)

Ejemplo de filtrado de un proceso con el nombre svchost, un ID de sesión 2 y ordenados alfabéticamente

![Untitled](Powershell%20420f513ced014f9ea8d76cc57003cd3a/Untitled%203.png)

Ejemplo de contar el # de tareas programadas dentro de un path específico con estatus READY

`Get-ScheduledTask -TaskPath "\Microsoft\*"| Where-Object State -EQ "Ready" | measure`

Ejemplo para obtener información de un usuario

`Get-CimInstance -ClassName Win32_ComputerSystem`

Ejemplo de busquéda recursiva para identificar la palabra password dentro de los archivos listados en una ruta específica

`C:\Windows\System32> Get-ChildItem -Filter *.txt -Recurse -ErrorAction SilentlyContinue -Force | Get-Content | Select-String "password”`