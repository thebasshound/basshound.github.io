# Python básico

Ejemplo

```python
kali@kali:~$cat pythonsample.py
#!/usr/bin/python
print("Scripting is fun!")
```

Existen diferencias importantes entre versiones de Python

Python es muy permisivo para el tratamiento de tipos de datos

Ejemplo útil para de manejo de cadenas

```python
kali@kali:~$cat tagslice3.py
#!/usr/bin/python

tag = '<a href="https://www.offensive-security.com/blog">Blog</a>'
start = "http"
end = "\">"
url = tag[tag.index(start):tag.index(end)]

print(url)

kali@kali:~$python tagslice3.py
https://www.offensive-security.com/blog
```

Booleano, entero, flotante y cadena son los tipos de dato que se manejan, en caso de requerir hacer una conversión entre tipos de datos usaremos el siguiente ejemplo:

```python
kali@kali:~$cat castTest.py
#!/usr/bin/python

numA = "86"
numB = "20"

print(type(numA))
print(type(numB))

newValue = int(numA) + int(numB)
print(newValue)
print(type(newValue))
print(type(str(newValue)))

kali@kali:~$python castTest.py
<class 'str'>
<class 'str'>
106<class 'int'><class 'str'>
```

## Listas y diccionarios

ejemplo lista

```python
kali@kali:~$cat listTest3.py
#!/usr/bin/python

fruitList = ["apple", "banana", "orange"]
fruitList.append("mango")

print(fruitList)

kali@kali:~$python listTest3.py
["apple", "banana", "orange","mango"]

kali@kali:~$ cat listTest5.py
#!/usr/bin/python

fruitList = ["apple", "banana", "orange"]
print(len(fruitList))

kali@kali:~$ python listTest5.py
3
```

ejemplo de diccionario

```python
kali@kali:~$cat dictTest.py
#!/usr/bin/python

theOne = {
        "firstName":"Thomas",
        "lastName":"Anderson",
        "occupation":"Programmer"
        }

theOne["company"] = "MetaCortex"

print(theOne)theOne["occupation"] = "Superhero"

print(theOne)

kali@kali:~$./dictTest.py
{'firstName': 'Thomas', 'lastName': 'Anderson','occupation': 'Programmer', 'company': 'MetaCortex'}
{'firstName': 'Thomas', 'lastName': 'Anderson','occupation': 'Superhero', 'company': 'MetaCortex'}
```

## Ciclos, condiciones y entradas de usuario

ejemplo for

```python
kali@kali:~$cat forLoop.py
#!/usr/bin/python

for i in range(10):
    print(i)

kali@kali:~$./forLoop.py
0
1
2
3
4
5
6
7
8
9
```

ejemplo while

```python
i = 0
while i < 10:
    print(i)
    i += 1
```

ejemplo recorrido diccionario

```python
for key in guts.keys():
    print(key + ": " + guts[key])
```

ejemplo de if, else y elif

```python
if numApples > 100:
    print("That's a lot of apples!")
elif numApples > 50:
    print("That's a very good amount of apples")
elif numApples > 30:
    print("That's a moderate amount of apples")
else:
    print("Running low on apples!")
```

ejemplo entrada de datos

```python
name = input("Please enter your name: ")
age =int(input("Please enter your age: "))
```

## Funciones y archivos

ejemplo lectura de archivos

```python
f = open("data.txt", "r")

for line in f:
    print(line)
```

ejemplo escritura

```python
myData = "I'm sample data to be written to a file"

f = open("data.txt", "a")

f.write(myData)
```

ejemplo de función

```python
#!/usr/bin/python

def hello():
    print("Hi there!")

hello()
```

ejemplo función con argumentos

```python
#!/usr/bin/python

def addNums(numA, numB):
    answer = numA + numB
    return answer

x = addNums(5, 7)

print(x)
```

ejemplo(best practice) de función para abrir y trabajar con el contenido de un archivo

```python
#!/usr/bin/python

def storeFile(file):
    f = open(file, 'r')
    contents = f.read()
    f.close()
    return contents

# Variable to store the filename
fileVar = "notes.txt"

contents = storeFile(fileVar)
print(contents)
```

Ejemplo del uso de **import** con el módulo ***requests***

```python
#!/usr/bin/python

import requests

r = requests.get('https://www.offensive-security.com/offsec/game-hacking-intro/')
print(r.status_code)
print(r.text)
```

### Sockets

```python
#!/usr/bin/python

import socket

s = socket.socket(socket.AF_INET,socket.SOCK_STREAM)

s.connect(("192.168.50.101", 9999))

print(s.recv(1024))
s.close()
```

el móudlo socket permite la comunicación entre dos equipos, el parametro AF_INET indica que se usara una ip de v4 y SOCK_STREAM indica que sera una conexión TCP mediante un puerto. 1024 será el tamaño del búffer, idealmente es la cantidad recomendada.

### WebSpider.py

```python
#!/usr/bin/python3
import requests

URL = "http://192.168.50.101/"
urlList = []
isFollowed = {}

def checkUrlList(URL):
   if URL in urlList:
       return True
   else:
       return False

def isFollowedCheck(URL):
   for entry in isFollowed.keys():
       if URL != entry:
           return False
       else:
           if isFollowed[URL] == "yes":
               return True
           else:
               return False

urlList.append(URL)

for URL in urlList:
   if isFollowedCheck(URL) != True:
       page = requests.get(URL)
       isFollowed[URL] = "yes"

       start = "http"
       for line in page.text.split("\n"):
           if "http" in line:
               if "192.168.50.101" in line:
                   if "\">" in line:
                       end = "\">"
                   else:
                       end = "\" "
                   sliced = line[line.index(start):line.index(end)]
                   if "\"" in sliced:
                       end = "\""
                       parsedURL = sliced[sliced.index(start):sliced.index(end)]
                   else:
                       parsedURL = sliced
                   if checkUrlList(parsedURL) == False:
                       urlList.append(parsedURL)
                       isFollowed[parsedURL] = "no"

for URL in urlList:
   print(URL)
```