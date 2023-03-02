# Bash Scripting

Para crear un script en bash se debe crear un archivo con extensión **.sh** y la cabecera **#!/bin/bash** además de tener permisos de ejecución:

```bash
kali@kali:~$cat ./hello-world.sh
#!/bin/bash
# Hello World Bash Script
echo "Hello World!"
```

La declaración de variables se hace con el simbolo de = y para invocar el valor se usa el simbolo $

Diferencia entre ‘ “ y **```**

```bash
kali@kali:~$user1="`whoami`"
kali@kali:~$echo $user1
kali
kali@kali:~$user2='`whoami`'
kali@kali:~$echo $user2
`whoami`
kali@kali:~$ user1=`whoami`
kali@kali:~$ echo $user1
kali
kali@kali:~$ user=$(whoami)
kali@kali:~$ echo $user
kali
```

Para realizar operaciones ariteméticas se usa doble (()) por ejemplo `echo $(($1 + 1))` muestra en pantalla la suma del primer argumento +1 

ejemplo de manejo de argumentos

```bash
kali@kali:~$cat arg.sh
#!/bin/bash
echo "There are $# arguments"
echo "The first two arguments are $1 and $2"
kali@kali:~$./arg.sh who goes there?
There are 3 arguments
The first two arguments are who and goes
```

**Nota**: Al asignar valores a las variables es importante **NO** dejar espacios en blanco. Ej `x=1` esta bien `x = 1` esta mal

## Variables especiales de bash

[Bash](Bash%20Scripting%207a717a6a13cf4145ae720448352fdae8/Bash%2039fb3bcb70f347faa9a6a288b785d578.md)

Ejemplo de lectura de archivos

```bash
kali@kali:~$cat input2.sh
#!/bin/bash
# Prompt the user for credentials
read -p 'Username: ' username
read -sp 'Password: ' password
echo "Thanks, your creds are as follows: " $username " and " $password

kali@kali:~$./input2.sh
Username:kali
Password:
Thanks, your creds are as follows:  kali  and  nothing2see!
```

## Condicionales

[Operadores](Bash%20Scripting%207a717a6a13cf4145ae720448352fdae8/Operadores%207cbdb7ba62454f509883e552e9259f76.md)

Ejemplo if-else

```bash
kali@kali:~$cat ./else.sh
#!/bin/bash
# else statement example

read -p "What is your age: " age

if [ $age -lt 18 ]
then
  echo "You might need parental permission to take this course!"
else
  echo "Welcome to the course!"
fi

kali@kali:~$./else.sh
What is your age:21
Welcome to the course!
```

Ejemplo elif

```bash
kali@kali:~$cat ./elif.sh
#!/bin/bash
# elif example

read -p "What is your age: " age

if [ $age -lt 18 ]
then
  echo "You might need parental permission to take this course!"
elif [ $age -gt 60 ]
then
  echo "Hats off to you, respect!"
else
  echo "Welcome to the course!"
fi

kali@kali:~$./elif.sh
What is your age:65
Hats off to you, respect!
```

## Ciclos

ejemplo for

```bash
kali@kali:~$for ip in $(seq 1 10); do echo 10.11.1.$ip; done
10.11.1.1
10.11.1.2
10.11.1.3
10.11.1.4
10.11.1.5
10.11.1.6
10.11.1.7
10.11.1.8
10.11.1.9
10.11.1.10
kali@kali:~$ for ip in {1..10}; do echo 10.11.1.$ip;done
10.11.1.1
10.11.1.2
10.11.1.3
10.11.1.4
10.11.1.5
10.11.1.6
10.11.1.7
10.11.1.8
10.11.1.9
10.11.1.10
```

ejemplo while

```bash
kali@kali:~$cat while2.sh
#!/bin/bash
# while loop example 2

counter=1

while [ $counter -le 10 ]
do
  echo "10.11.1.$counter"
  ((counter++))
done
```

lectura de un archivo como input con while

```bash
kali@kali:~$cat input5.sh
#!/bin/bash
file="poem.txt"
while read line
   echo $line
done < $file

kali@kali:~$./input5.sh
The secrets of wealth and the love of the muse,
But gladness is predictable by universal laws;
Awe! Awe! Awareness in life without autocracy! !
For my name is Heidi.
```

## Funciones

```bash
kali@kali:~$cat func.sh
#!/bin/bash
# function example

print_me () {
  echo "You have been printed!"
}

print_me

kali@kali:~$./func.sh
You have been printed!
```

Ejemplo con return, donde $ funciona como elemento donde se coloca el valor regresado

```bash
kali@kali:~$cat funcrvalue.sh
#!/bin/bash
# function return value example

return_me() {
  echo "Oh hello there, I'm returning a random value!"
  return $RANDOM
}

return_me

echo "The previous function returned a value of $?"

kali@kali:~$chmod +x ./funcrvalue.sh

kali@kali:~$./funcrvalue.sh
Oh hello there, I'm returning a random value!
The previous function returned a value of 198
```

ejemplo práctico

```bash
kali@kali:~$cat usage.sh
#!/bin/bash

display_usage() {
  cat << EOF
  usage: ./usage.sh name

  This script will check whether the named account exists.
  It will also check whether a home folder exists for the account.
EOF
}

if [[ $# != 1 ]]
then
  display_usage
else
  grep -q $1 /etc/passwd && echo "$1 found in /etc/passwd"
  if [ -d "/home/$1" ]
  then
    echo "The folder /home/$1 exists"
  fi
fi

kali@kali:~$./usage.sh
  usage: ./usage.sh name

  This script will check whether the named account exists.
  It will also check whether a home folder exists for the account.

kali@kali:~:./usage.sh kali
kali found in /etc/passwd
The folder /home/kali exists
```