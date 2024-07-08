### bandit0

ssh bandit0@bandit.labs.overthewire.org -p 2220
pass bandit0

No me funciona crtl+l y es porque estoy usando un kitty

``echo $term 
`export TERM=xterm```

Ahora si puedo hacer crtl + l y borrar pantalla

---------------------------------------------------

#### si listamos vemos un readme que nos dice:

*Congratulations on your first steps into the bandit game!!
Please make sure you have read the rules at https://overthewire.org/rules/
If you are following a course, workshop, walthrough or other educational activity,
please inform the instructor about the rules as well and encourage them to
contribute to the OverTheWire community so we can keep these games free!*

The password you are looking for is: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If``

------------------------------------------------------------------------

#### Ahora nos tenemos que convertir en bandit1

listamos los usuarios dentro de /etc/passwd

```
sshpass -p 'ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If'ssh bandit0@bandit.labs.overthewire.org -p 2220
```

#### Leer un fichero con simbolo


``
```
`cat - [wrong!]
cat * [tampoco]
cat /home/bandit1/-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx

too

cat ./-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx

echo $(pwd)
/home/bandit1

echo $(pwd)/-
/home/bandit1/-

cat $(pwd)/-
263JGJPfgU6LtdEvgfWU1XP5yac29mFx

grep -r "\w" 2>/dev/null
``
grep -r "\w" 2>/dev/null | tail -n 1 (en el caso de que sea la ultima lienea)
````


`grep -r "\w" 2>/dev/null | tail -n1 | awk  '{print $2}' FS=":"`

se queda con la primera linea en el segundo argumento y partiendo del delimitador : 
entonces toma solo el pass

`grep -r "\w" 2>/dev/null | tail -n 1 | cut -d ':' -f 2`

basicamente lo mismo

`grep -r "\w" 2>/dev/null | tail -n 1 | tr ':' ' ' | awk '{print $2}'`

tr es para sustituir y se queda con el segundo argumento 

`grep -r "\w" 2>/dev/null | tail -n 1 | tr ':' ' ' | awk 'NF{print $NF}'`

toma la ultima linea

#### conectandose a bandit2

------------------------------------------------------------------------

`ssh bandait2@bandit `
263JGJPfgU6LtdEvgfWU1XP5yac29mFx

### Leer el fichero con espacios

```
	ls

spaces in the files

	cat /home/bandit2/s(tabulamos y sale)
	cat 'spaces in the files'
	MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
	cat s* 	
```

------------------------------------------------------------------------

### ssh bandit3@bandit.labs.overthewire.org -p 2220

```
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere/
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ls -la
total 12
drwxr-xr-x 2 root    root    4096 Jun 20 04:07 .
drwxr-xr-x 3 root    root    4096 Jun 20 04:07 ..
-rw-r----- 1 bandit4 bandit3   33 Jun 20 04:07 ...Hiding-From-You
bandit3@bandit:~/inhere$ cat /home/bandit3/inhere/...Hiding-From-You 
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
bandit3@bandit:~/inhere$ 
```

#### Se puede ver que tipo de archivo es o incluso se lo puede listar like this:

```
find .

bandit3@bandit:~$ find .
.
./.bashrc
./inhere
./inhere/...Hiding-From-You
./.profile
./.bash_logout
```

#file

```
bandit3@bandit:~/inhere$ file ...Hiding-From-You 
...Hiding-From-You: ASCII text
```

#xargs 

```
 find . -type f | grep "Hiding" | xargs cat
  2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
```

#grep 

##### No muestres los archivos bash y profile
```
find . -type f | grep -vE "bashrc|profile"
```

-------------------------------------------------------------------

### ssh bandit4@bandit.labs.overthewire.org -p 2220
2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ

The password for the next level is stored in the only human-readable file in the **inhere** directory. Tip: if your terminal is messed up, try the “reset” command.

La contraseña para el siguiente nivel se almacena en el único archivo legible por humanos en el directorio de aquí. Sugerir: si su terminal está desordenado, pruebe el comando de Ereset. 

```
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere/
bandit4@bandit:~/inhere$ find .
.
./-file08
./-file06
./-file09
./-file01
./-file03
./-file04
./-file00
./-file07
./-file05
./-file02
bandit4@bandit:~/inhere$ ls -la
total 48
drwxr-xr-x 2 root    root    4096 Jun 20 04:07 .
drwxr-xr-x 3 root    root    4096 Jun 20 04:07 ..
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file00
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file01
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file02
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file03
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file04
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file05
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file06
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file07
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file08
-rw-r----- 1 bandit5 bandit4   33 Jun 20 04:07 -file09
```

#### aparentemente son todos legibles pero ...

```
bandit4@bandit:~/inhere$ find . -type f | grep "\-file"
./-file08
./-file06
./-file09
./-file01
./-file03
./-file04
./-file00
./-file07
./-file05
./-file02
```

#### Ahora vemos que tipo de archivo es 

```
bandit4@bandit:~/inhere$ find . -type f | grep "\-file" | xargs file
./-file08: data
./-file06: data
./-file09: data
./-file01: data
./-file03: data
./-file04: data
./-file00: data
./-file07: ASCII text
./-file05: data
./-file02: data
```

#### en base a los magic numbers nos detecta que es un txt
#### El file07 es ascii, si le hacemos un cat:

```
bandit4@bandit:~/inhere$ find . -type f | grep "\-file07" | xargs cat 
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
```

------

### ssh bandit5@bandit.labs.overthewire.org -p 2220
4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw

#### Búsquedas precisas de archivos [1-2]

*The password for the next level is stored in a file somewhere under the **inhere** directory and has all of the following properties:

- human-readable
- 1033 bytes in size
- not executable*

La contraseña para el siguiente nivel se almacena en un archivo en algún lugar bajo el directorio de aquí y tiene todas las siguientes propiedades: 
    humanes-legisibles
    1033 bytes de tamaño
    no ejecutable

[https://www.ionos.es/digitalguide/servidores/configuracion/comando-linux-find/]

#find 

#### quiero buscar archivos humanamente legibles

```
find . -type f -readable
```

#### Tambien que tenga capacidad de ejecutar

```
find . -type f -readable -executable 
```

#### Pero yo quiero los que no son ejecutables

```
find . -type f ! -executable
```

#### Tambien quiero que me lo filtres por size

```
find . -type f ! -executable -size 1033c
```

#### Ahora si, que tipo de archivo es el que filtra?

```
find . -type f ! -executable -size 1033c | xargs file

./inhere/maybehere07/.file2: ASCII text, with very long lines (1000)

```

#### Y lo leemos

```
find . -type f ! -executable -size 1033c | xargs cat

HWasnPhtq9AVKe0dmk45nxy20cvUa6EG
```

------

###  Búsquedas precisas de archivos [2-2]
#### ssh bandit6@bandit.labs.overthewire.org -p 2220
HWasnPhtq9AVKe0dmk45nxy20cvUa6EG

*The password for the next level is stored **somewhere on the server** and has all of the following properties:

- owned by user bandit7
- owned by group bandit6
- 33 bytes in size*

*La contraseña para el siguiente nivel se almacena en algún lugar del servidor y tiene todas las siguientes propiedades: 
    propiedad de la bandida de usuario7
    propiedad del grupo bandido6
    33 bytes en tamaño*


$$ lo primero que se me ocurre es buscar desde la raiz con find / e ir agregandole los filtros como -type f -size 33c y para el usr & group falleci $$

```
find / -type f -size 33c 2>/dev/null

pero son unos cuantos archivos
```

#### filtro por usr y grupo

```
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null 

/var/lib/dpkg/info/bandit7.password

```

* que grande "find" *

#### Lo leemos y pim pam pum 

```
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null | xargs cat
```

```
morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj
```

----

###  Métodos de filtrado de datos [1-2]

*ssh bandit7@bandit.labs.overthewire.org -p 2220*

$ The password for the next level is stored in the file **data.txt** next to the word **millionth** $
"La contraseña para el siguiente nivel se almacena en el archivo data.txt junto a la palabra millonésima "

$$ lo primero que se me ocurre es esto
find / -type f -name 'millonth' 2>/dev/null | xargs ls -l | cat data.txt
pero no puedo filtrar 

### pero resulta que asi no era ###
$$


#### Era tan facil como esto

```
cat data.txt | grep "millonth"
```
```
millionth	dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

>for i in $(seq 1 20); do echo "[+] Iteracion numero $i" ; done
>[+] Iteracion numero 1
[+] Iteracion numero 2
[+] Iteracion numero 3
[+] Iteracion numero 4
[+] Iteracion numero 5
[+] Iteracion numero 6
[+] Iteracion numero 7
[+] Iteracion numero 8
[+] Iteracion numero 9
[+] Iteracion numero 10
[+] Iteracion numero 11
[+] Iteracion numero 12
[+] Iteracion numero 13
[+] Iteracion numero 14
[+] Iteracion numero 15
[+] Iteracion numero 16
[+] Iteracion numero 17
[+] Iteracion numero 18
[+] Iteracion numero 19
[+] Iteracion numero 20

#### Se puede filtrar de diferente forma en cuanto a los espacios

```
cat data.txt | grep "millonth" | awk 'NF{print $NF}'

Y el vago se saltea los espcios como un campion
```

#### Tambien se puede reversear e imprimirlo por pantalla

```
cat data.txt | grep "millonth" | rev

cEe7gLMksWoR9eOFbNfw0Um4iQFzvwfd	htnoillim
```

#### elegimos el primer argumento y lo aislamos

```
cat data.txt | grep "millonth" | rev | cut -d ' ' -f 1
```


#awk / no era asi pero funciono

```
 cat data.txt | grep "millionth" | rev | awk 'NF{print $1}'
cEe7gLMksWoR9eOFbNfw0Um4iQFzvwfd
```

#xargs / te permite compactar y despues con cut se puede imprimir 

```
cat data.txt | grep "millionth" | rev | xargs | cut -d ' ' -f 1

cEe7gLMksWoR9eOFbNfw0Um4iQFzvwfd
```

#### Ahora nos ponemas mas hincha pelotas

```
cat data.txt | grep "millonth" | xargs | tr ' ' '\n'

millionth
dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc
```

[los especios se convierten en saltos de linea]

#### me quiero quedar con la ultima linea #tail

```
cat data.txt | grep "millionth" | xargs | tr ' ' '\n' | tail -n 1
```

https://www.shortcutfoo.com/app/dojos/awk/cheatsheet
https://bl831.als.lbl.gov/~gmeigs/scripting_help/awk_cheat_sheet.pdf

------
## bandit 8

# Método de filtrado de datos [2-2]

[The password for the next level is stored in the file **data.txt** and is the only line of text that occurs only once]

[La contraseña para el siguiente nivel se almacena en el archivo data.txt y es la única línea de texto que se produce sólo una vez]

#sort
#### Con sort lo ordena alfabeticamente

```
sort data.txt
```

#uniq

#### Con uniq nos filtra por la unica linea que se produce una vez

```
sort data.txt | uniq -u

4CKMh1JI91bUIZZPXDqGanal4xvAg0JM

```
https://www.ibidemgroup.com/edu/tutorial-sort-linux-unix/
https://victorhckinthefreeworld.com/2021/10/21/el-comando-uniq-de-gnu/
------------------

## bandit 9 ->10

# Interpretación de archivos binarios

[The password for the next level is stored in the file **data.txt** in one of the few human-readable strings, preceded by several ‘=’ characters.]

[La contraseña para el siguiente nivel se almacena en el archivo data.txt en una de las pocas cadenas legibles por humanos, precedida por varios caracteres. ]

#strings

#### Con string podemos imprimir lo legible

```
 strings data.txt | grep "==="

[========== the
T%========== passwordG
}========== ist"
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey

```

#tail 
#### Ahora podemos filtrar con tail para las ultimas lineas

```
bandit9@bandit:~$ strings data.txt | grep "===" | tail -n 1 
========== FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
tail -n 1
tail -n 2
```

#head
#### Tambien podemos con head las primeras lineas

```
head -n 1
head -n 2
```

#### Filtramos por el ultimo argumento

```
bandit9@bandit:~$ strings data.txt | grep "===" | tail -n 1 | awk 'NF{print $NF}'

FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey
```

--------

# Codificación y decodificación en base64
## Bandit 10 ->11


[The password for the next level is stored in the file **data.txt**, which contains base64 encoded data]

[      La contraseña para el siguiente nivel se almacena en el archivo data.txt, que contiene base64 datos codificados      ]

#base64
#### Convertimos una cadena a base4

```
echo "esto es una prueba" | base64 -w 0
ZXN0byBlcyB1bmEgcHJ1ZWJhCg
```

#### Para volver a leerla la decodificamos

```
echo "ZXN0byBlcyB1bmEgcHJ1ZWJhCg" | base64 -d
esto es una prueba
```

#### Entonces leemos y decodificamos

```
cat data.txt | base64 -d
The password is dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr
```

--------------------------

# Cifrado césar y uso de tr para la traducción de caracteres

## Bandit 11 ->12

[The password for the next level is stored in the file **data.txt**, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions]

[La contraseña para el siguiente nivel se almacena en el archivo data.txt, donde todas las letras minúsculas (a-z) y mayúsculas (A-Z) han sido rotadas por 13 posiciones ]

[https://rot13.com/]

{parte facil}
#### Poniendolo en la pag, se rotan 13 posiciones y chimpun

```
The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

{parte dificil}

#tr
#### Sustitucion con tr

```
cat data.txt | tr '[G-ZA-Fg-za-f]' '[T-ZA-St-za-s]'

The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

##### esto seria mas facil [que poronga la puta madre]

```
cat data.txt | tr '[A-Za-z]' '[N-ZA-Mn-za-m]'

The password is 7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4
```

// no habia parte facil //

--------------
# Creamos un descompresor recursivo automático de archivos en Bash
## Bandit 12 ->13

[The password for the next level is stored in the file **data.txt**, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)]

[La contraseña para el siguiente nivel se almacena en el archivo data.txt, que es un hexdump de un archivo que ha sido comprimido repetidamente. Para este nivel puede ser útil crear un directorio debajo de /tmp en el que pueda trabajar. Utilice mkdir con un nombre de directorio difícil de adivinar. O mejor, use el comando "mktemp -d". A continuación, copie el archivo de datos usando cp, y renombrarlo usando mv (leer las manpages). ]


#### Vamos a crear un archivo en nuestra maquina y pegarle el contenido de data.txt

#xxd

#### Se le puede pasar el comando xxd a un fichero y verlo en hexadecimal

```
cat /etc/hosts | xxd -ps | xargs | tr -d ' '
```

*xxd -ps = convierte todo a hexadecimal y elimina aquello que no lo es

*xargs = compila

*tr -d ' ' = elimina los espacios

```
2320537461746963207461626c65206c6f6f6b757020666f7220686f73746e616d65732e0a232053656520686f73747328352920666f722064657461696c732e0a
```

#### Ahora se puede hacer el proceso inverso

```
echo "2320537461746963207461626c65206c6f6f6b757020666f7220686f73746e616d65732e0a232053656520686f73747328352920666f722064657461696c732e0a" | xxd -ps -r
```

*-r = es de reversing

#sponge

#### Utilizamos un redireccionamiento

```
touch file | echo "Hola esto es una prueba" > file
cat file | awk '{print $3}' | sponge file
```

#### vemos el reversing y lo volvemos a meter en data con sponge 

```
cat data | xxd -r | sponge data
```

#### Entonces si le hacemos un file vemos que tipo de archivo se ha convertido  

  ```
  data: gzip compressed data, was "data2.bin", last modified: Thu Jun 20 04:06:52 2024, max compression, from Unix, original size modulo 2^32 577
```

[list of file signatures {wikipedia}]

#### change name descomprimimos

```
mv data data.gz
```

#gunzip (descomprimir)

```
7z l data.gz

7-Zip [64] 17.05 : Copyright (c) 1999-2021 Igor Pavlov : 2017-08-28
p7zip Version 17.05 (locale=en_US.UTF-8,Utf16=on,HugeFiles=on,64 bits,4 CPUs x64)

Scanning the drive for archives:
1 file, 610 bytes (1 KiB)

Listing archive: data.gz

--
Path = data.gz
Type = gzip
Headers Size = 20

   Date      Time    Attr         Size   Compressed  Name
------------------- ----- ------------ ------------  ------------------------
2024-06-20 01:06:52 .....          577          610  data2.bin
------------------- ----- ------------ ------------  ------------------------
2024-06-20 01:06:52                577          610  1 files
precmd: vcs_info: function definition file not found                  
```

#7z 

#### Con '7z x' se puede descomprimir cualquier comprimido independientemente de la extencions 

```
7z x data / y hay otro archivo 
```

#### Es una puta mamushka

#rm

```
rm *
rm {data3,data5} se puede borrar en conjunto / no me funciono pero bueno otro dia

```

#### Creamos un script descompresor.sh


```
#!/bin/bash

function ctrl_c(){

echo -e "\n\n[!] Saliendo .... \n"
exit 1

}

#crtl+c

trap ctrl_c INT

first_file_name="data.gz"
#sleep 10 espera 10 segundos

decompressed_file_name="$(7z l data.gz | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
#echo -e "\n[+] El primer archivo es $first_file_name"
#echo -e "\n[+] El siguiente archivo es $decompressed_file_name"

7z x $first_file_name &>/dev/null

while [ $decompressed_file_name ]; do
  echo -e "\n[+] Nuevo archivo descomprimido: $decompressed_file_name"
  7z x $decompressed_file_name &>/dev/null
  decompressed_file_name="$(7z l $decompressed_file_name 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"

done

```

#echo$? codigo de estado

#### Finalmente nos descomprime todos los archivos y se puede leer el ultimo archivo, la cncha de tu madre

```
The password is FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

 -------------------

# Manejo de pares de claves y conexiones SSH
## Bandit 13 ->14

[The password for the next level is stored in **/etc/bandit_pass/bandit14 and can only be read by user bandit14**. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. **Note:** **localhost** is a hostname that refers to the machine you are working on]

[La contraseña para el siguiente nivel se almacena en /etc/bandit-pass/bandit14 y sólo puede ser leído por el bandido del usuario14. Para este nivel, no obtendrás la siguiente contraseña, pero obtienes una clave privada de SSH que se puede utilizar para iniciar sesión en el siguiente nivel. Nota: localhost es un nombre de host que se refiere a la máquina en la que está trabajando ]

```
cat: /etc/bandit_pass/bandit14: Permission denied
bandit13@bandit:~$ cat /etc/bandit_pass/bandit14 | xargs ls -l
cat: /etc/bandit_pass/bandit14: Permission denied
total 4
-rw-r----- 1 bandit14 bandit13 1679 Jun 20 04:06 sshkey.private


```

#### Instalamos openssh y habilitamos el servicio

```
sudo systemctl start sshd (el cual es un demonio)
```

{para conectarse a otro usuario con ssh debemos proporcionar la clave de ssh, en caso de tenerla, pero si no la tenemos entra en juego ssh-keygen}

#ssh-keygen

```
ssh-keygen

Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/heavenblack/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/heavenblack/.ssh/id_ed25519
Your public key has been saved in /home/heavenblack/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:WZWn5bkr05/i2qjW2/gB5/LNiRReZpnPw13hUGk3iGM heavenblack@archlinux
The key's randomart image is:
+--[ED25519 256]--+
|            o....|
|           E..++.|
|          o .*.oo|
|         o  . = +|
|        S  . o O.|
|            = B.+|
|          .. * ++|
|         . .@.B +|
|        ...==X.*.|
+----[SHA256]-----+

ls

  id_ed25519  󰌆 id_ed25519.pub 
```

#### Ahora tenemos 2 pares de claves 'id_ed25519.pub' [publica] 'id_ed25519' [privada]

*si quiero acceder al sistema sin proporcionar pass*

```
cp id_ed25519.pub authorized_keys
```
*convertimos la keypub a una clave autorizada no va  a pedir pass*

[si quiero ingresar a un objetivo por ssh y me pide un pass, deberia meter mi keypub dentro del directorio /home/usr/.ssh]

[tambien hacer que la keyprivate volverla autorizada, para que cualquiera que disponga de esa clave se pueda conectar]

[deberia profundizar mas en el tema]

#### Utilizamos el fichero sshkey.private para obtener acceso a bandit 14

```
ssh -i sshkey.private bandit14@localhost -p 2220
```

```
 cat /etc/bandit_pass/bandit14 
 
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

Los pares de claves SSH son dos claves criptográficamente seguras que pueden usarse para autenticar a un cliente a un servidor SSH. Cada par de claves está compuesto por una **clave pública** y una **clave privada**.

El **cliente** mantiene la **clave privada** y debe mantenerla en absoluto secreto. Poner en riesgo la clave privada puede permitir a un atacante no autorizado iniciar sesión en los servidores que están configurados con la clave pública asociada sin autenticación adicional. Como medida de precaución adicional, es recomendable cifrar la clave en el disco con una frase de contraseña.

La **clave pública** asociada puede compartirse libremente sin ninguna consecuencia negativa. La clave pública puede usarse para cifrar mensajes que **sólo la clave privada puede descifrar**. Esta propiedad se emplea como forma de autenticación mediante el uso del par de claves.

----------

# Uso de netcat para realizar conexiones
## Bandit 14 -> 15


[The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.]

[La contraseña para el siguiente nivel se puede recuperar enviando la contraseña del nivel actual al puerto 30000 en localhost. ]

***Netcat** es una herramienta de línea de comandos que sirve para escribir y leer datos en la red. Para la transmisión de datos, Netcat usa los protocolos de red TCP/IP y UDP. La herramienta proviene originalmente del mundo de Unix; desde entonces, se ha expandido a todas las plataformas.

Gracias a su universalidad, a Netcat se la llama “**la navaja suiza del TCP/IP**”. Puede utilizarse, por ejemplo, para diagnosticar errores y problemas que afecten a la funcionalidad y la seguridad de una red. Netcat también puede escanear puertos, hacer streaming de datos o simplemente transferirlos. Además, permite configurar servidores de chat y de web e iniciar consultas por correo. Este software minimalista, desarrollado a mediados de los 90, puede operar en modo servidor y cliente.*


#### Nos conectamos con netcat

```
nc localhost 3000

le pasamos el pass anterior
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS

devuelve el pass
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

[ver puertos abiertos 
[+]
>netstat -nat

>ss -nltp
>
>cat /proc/net/tcp]

 ```
 cat /proc/net/tcp
```

![[Shot-2024-07-02-193110.png]]

*alguno de esos son los puertos en hexadecimal*

```
 echo "19C8 C3B4 BB16 C0EC C976 9A56 C446 869C BB1C 9922" | sort -u
```

#### Con python podemos ver desde hexadecimal

#python3 al pasarle el 0x al principio de los caracteres devuelve el puerto

```
python3

>>> 0x19c8
6600
>>> 0xc976
51574
```

#### Iterando sobre un output

```
  echo "19C8 C3B4 BB16 C0EC C976 9A56 C446 869C BB1C 9922" | while read line; do echo "Estamos leyendo la linea $line"; done

Estamos leyendo la linea 19C8 C3B4 BB16 C0EC C976 9A56 C446 869C BB1C 9922
```

```
for line in $(echo "19C8 C3B4 BB16 C0EC C976 9A56 C446 869C BB1C 9922"); do echo "[+] Mostrando el contenido $line"; done

[+] Mostrando el contenido 19C8
[+] Mostrando el contenido C3B4
[+] Mostrando el contenido BB16
[+] Mostrando el contenido C0EC
[+] Mostrando el contenido C976
[+] Mostrando el contenido 9A56
[+] Mostrando el contenido C446
[+] Mostrando el contenido 869C
[+] Mostrando el contenido BB1C
[+] Mostrando el contenido 9922
```

#### Ahora hay que pasarlo a legile

```
echo "19C8 C3B4 BB16 C0EC C976 9A56 C446 869C BB1C 9922" | sort -u | while read line; do echo "[+] Puerto $line -> $(echo "obase=10 ibase=16 $line" | bc) -OPEN" ; done
```

#### no me quedo ni ahi 

#lsof
#### que servicio corre por ese puerto?

```
lsof -i:6600

COMMAND PID        USER FD   TYPE DEVICE SIZE/OFF NODE NAME
mpd     531 heavenblack 9u  IPv4   8209      0t0  TCP localhost:mshvlm (LISTEN)
```
 --------
# Uso de conexiones encriptadas

## bandit 15 ->16

[The password for the next level can be retrieved by submitting the password of the current level to **port 30001 on localhost** using SSL encryption.

**Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…**]

[La contraseña para el siguiente nivel se puede recuperar enviando la contraseña del nivel actual al puerto 30001 en localhost usando cifrado SSL. 
Nota útil: Obstándote a "HEARTBEATING" y "Leer R BLOCK"? Utilice -igneof y lea la sección COMMANDS CONNECTED en la página de manía. Junto a la R y el comando "B" también ]

#wich se utiliza para listar el path absoluto de un binario

{crtl+shift+h= filtra desde la kitty}

#### nos conectamos con ncat al localhst por el -p 3001

```
ncat -ssl 127.0.0.1 30001

le paso el pass actual

8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo

y devuelve

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

```

-------
# Creando nuestros propios escáneres en Bash
## bandit 17->18

[The credentials for the next level can be retrieved by submitting the password of the current level to **a port on localhost in the range 31000 to 32000**. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.]

[Las credenciales para el siguiente nivel se pueden recuperar presentando la contraseña del nivel actual a un puerto en localhost en el rango de 31000 a 32000. Primero averiguar cuál de estos puertos tiene un servidor escuchando. Entonces averíde cuál de esos habla SSL y cuál no. Sólo hay 1 servidor que dará las siguientes credenciales, los otros simplemente le enviarán de vuelta lo que le envíes. ]

#### nvim portscanner.sh

```
 echo '' > /dev/tcp/127.0.0.1/22                                                                                    
zsh: no such file or directory: /dev/tcp/127.0.0.1/22

bash

'' > /dev/tcp/127.0.0.1/
```

[[kill %%  para ver si hay tareas en segundo plano]]

#### Ahora quedaria algo masomos asi 

```
(echo '' > /dev/tcp/127.0.0.1/22) 2>/dev/null && echo "[+] El puerto esta abierto" || echo "[+] El puerto esta cerrado"
```

#### script

```
#!/bin/bash
#
function crtl_c(){
	echo -e "\n\n[!] Saliendo ...\n"
	exit 1
#
}
#
## crtl+c
trap crtl_c INT	

for port in $(seq 1 655535); do
	(echo '' > /dev/tcp/127.0.0.1/$port) 2>/dev/null && echo "[+] $port -OPEN" &
done; wait
```

#### lineas; se torna mas rapido porque espera 1 segundo

```
timeout 1 bash -c "ping -c 1 192.168.0.1" && echo "[+] El host esta activo" || echo "[+] El host esta muerto"
```

#### si se lo metemos al script

```
#!/bin/bash
#
function crtl_c(){
	echo -e "\n\n[!] Saliendo ...\n"
# ahora no vemos el cursor
tput cnorm;	exit 1
}
#ocultando el cursor
tput civis
## crtl+c
trap crtl_c INT	

for port in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 192.168.0.1" && echo "[+] El host esta activo" || echo "[+] El host esta muerto"
done 
# recuperando el cursor
tput cnorm
```

[de esta forma me da una secuencia anlizando 1 host nada mas]

#### Ahora si lo muestra de forma secuencial 

```
#!/bin/bash
#
function crtl_c(){
	echo -e "\n\n[!] Saliendo ...\n"
	exit 1
#
}
#
## crtl+c
trap crtl_c INT	

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 192.168.0.$i 2>/dev/null" && echo "[+] El host esta activo" || echo "[+] El host esta muerto"
done 
```

#### s4vitar lo hizo mas eficaz 

```
#!/bin/bash
#
function crtl_c(){
	echo -e "\n\n[!] Saliendo ...\n"
	exit 1
#
}
#
## crtl+c
trap crtl_c INT	

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 192.168.0.$i 2>/dev/null" && echo "[+] 192.168.0.$i -ACTIVE"
done


PING 192.168.0.1 (192.168.0.1) 56(84) bytes of data.
64 bytes from 192.168.0.1: icmp_seq=1 ttl=64 time=7.23 ms

--- 192.168.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 7.234/7.234/7.234/0.000 ms
[+] 192.168.0.1 -ACTIVE
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.

```

#### le voy agregar un || para los que estan cerrados


```
#!/bin/bash

function crtl_c(){
	echo -e "\n\n[!] Saliendo ...\n"
	exit 1
}
## crtl+c
trap crtl_c INT	

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 192.168.0.$i 2>/dev/null" && echo "[+] 192.168.0.$i -ACTIVE" || echo "[+] 192.168.0.$i -CLOSE"
done


PING 192.168.0.1 (192.168.0.1) 56(84) bytes of data.
64 bytes from 192.168.0.1: icmp_seq=1 ttl=64 time=3.80 ms

--- 192.168.0.1 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 3.795/3.795/3.795/0.000 ms
[+] 192.168.0.1 -ACTIVE
PING 192.168.0.2 (192.168.0.2) 56(84) bytes of data.
[+] 192.168.0.2 -CLOSE
PING 192.168.0.3 (192.168.0.3) 56(84) bytes of data.
[+] 192.168.0.3 -CLOSE


[!] Saliendo ...

```

[Efectivamente quedo ma mejor]

#### aca se muestran solo los que estan activos; tirando de lineas

```
#!/bin/bash

function crtl_c(){
	echo -e "\n\n[!] Saliendo ...\n"
	exit 1
}
## crtl+c
trap crtl_c INT	

for i in $(seq 1 254); do
	timeout 1 bash -c "ping -c 1 192.168.0.$i 2>/dev/null" && echo "[+] 192.168.0.$i -ACTIVE" &
done; wait


```

#mktemp 
#### nos metemos en /tmp y creamos un directorio temporal de trabajo con mktemp -d

```
cd /tmp
mktemp -d
y nos devuelve un directorio en el cual nos tenemos que meter
cd ajsdnsakdn
touch portscan.sh
chmod +x portscan.sh
nano portscan.sh
```

```
#!/bin/bash

function (){
	echo "\n\n[+] Saliendo .... \n"
	exit 1
}
#crtl_c 
trap crtl_c INT

for port in $(seq 31000 32000); do
	(echo '' > /dev/tcp/127.0.0.1$port 2>/dev/null) && echo "[+] $port -OPEN"
done
```

#### tuve que tirar de nmap porque el script me trae problemas

```
$ nmap -T5 -n -p 31000-32000 127.0.0.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-07-03 17:37 UTC
Nmap scan report for 127.0.0.1
Host is up (0.00016s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

```


#netcat
#### ahora que detecta los puertos que estan abiertos intentamos conectarnos con netcat

```
ncat --ssl 127.0.0.1 31790

le pasamos el pass

kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx

si engancha el output es una privatekey

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----



```

#id_rsa
#### una vez obtenida lo guardamos en un fichero id_rsa 

[las claves privadas tienen como privilegios (600)]

`chmod 600 id_rsa`

#### lo utilizamos como archivo de identidad para conectarnos como bandit17

`ssh -i id_rsa bandit17@localhost`

#### entramos, y si listamos el path de los pass las vemos a todas

`cat /etc/bandit_pass/bandit17`

#### nos devuelve el output

``EReVavePLFHtFlFsjn3hyzMlvSuSAcRD``

--------------------------------

# Detección de diferencias entre archivos

## bandit17 ->18

[There are 2 files in the homedirectory: **passwords.old and passwords.new**. The password for the next level is in **passwords.new** and is the only line that has been changed between **passwords.old and passwords.new**

**NOTE: if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19**]

[Hay 2 archivos en el directorio de inicio: contraseñas.old y contraseñas.news. La contraseña para el siguiente nivel está en contraseñas.new y es la única línea que ha sido cambiada entre contraseñas.old y contraseñas.new NOTA: si has resuelto este nivel y verás "Byebye" cuando intentas iniciar sesión en bandido18, esto está relacionado con el siguiente nivel, bandit19  ]

#### verificamos si los 2 archivos tienen la misma cantidad de lineas

```
wc -l *

 100 passwords.new
 100 passwords.old
 200 total

```

#### efectivamente

#### listamos las 5 primeras lineas del primer archivo

```
cat passwords.news | head -n 5

GBSpdAt8lIKFlWIDrmxLHVygNMn4IAZ5
h0aoGAzIcWZmf768LOdOiGAaJs7sVlCK
pArborKSqov71YY0FMTXoc5FbABEY5tK
PuJhQuNoEXQ66BrN2uqbEaEIPCIO3zBF
VH3q1dN6f8XMyvnanQuzWNZIp2pXOTg9

bandit17@bandit:~$ cat passwords.old | head -n 5

GBSpdAt8lIKFlWIDrmxLHVygNMn4IAZ5
h0aoGAzIcWZmf768LOdOiGAaJs7sVlCK
pArborKSqov71YY0FMTXoc5FbABEY5tK
PuJhQuNoEXQ66BrN2uqbEaEIPCIO3zBF
VH3q1dN6f8XMyvnanQuzWNZIp2pXOTg9

```

#### se supone que son iguales

#diff

https://eltallerdelbit.com/comando-diff-ejemplos/
#### vemos las diferencias que hay 

```
diff passwords.news passwords.old 

42c42
< FtePUTiLiwPzjIFw2T7o57oBS4zUvPpg     /se saco
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO     /se puso

```

#### esta seria el pass x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO pero te expulsa 

-------

# Ejecución de comandos por SSH

## bandit18 ->19

[The password for the next level is stored in a file **readme** in the homedirectory. Unfortunately, someone has modified **.bashrc** to log you out when you log in with SSH.]

[La contraseña para el siguiente nivel se almacena en un rema de archivo en el directorio de inicios. Desafortunadamente, alguien ha modificado .bashrc para cerrar sesión cuando inicias sesión con SSH. ]

#### como el muy hijo de puta nos expulsa, podemos inyectar un comando 

```
ssh bandit18@bandit.labs.overthewire.org -p 2220 *whoami*
                         _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
*bandit18*

```

#### deja que me conecte y pude inyectar el comando

#### ahora puedo forzar a que me de una bash antes que me eche

```
ssh bandit18@bandit.labs.overthewire.org -p 2220 bash
                 _                     _ _ _   
                        | |__   __ _ _ __   __| (_) |_ 
                        | '_ \ / _` | '_ \ / _` | | __|
                        | |_) | (_| | | | | (_| | | |_ 
                        |_.__/ \__,_|_| |_|\__,_|_|\__|
                                                       

                      This is an OverTheWire game server. 
            More information on http://www.overthewire.org/wargames

bandit18@bandit.labs.overthewire.org's password: 
whoami
bandit18

```

#### listo, entre, ahora me dice que el pass esta alojado en README

```
 ls
readme
cat readme
cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8

```

#### efectivamente es el pass

------
# Abusando de privilegio SUID para migrar de usuario
## bandit19 ->20


[To gain access to the next level, you should use the setuid binary in the homedirectory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.]

[Para tener acceso al siguiente nivel, debe utilizar el binario setuid en el homedirectory. Ejecutarlo sin argumentos para averiguar cómo usarlo. La contraseña para este nivel se puede encontrar en el lugar habitual (/etc/bandit-pass), después de haber utilizado el binario setuid. ]

#### aparece un binario suid compilado de 32bits

```
file bandit20-do 
bandit20-do: setuid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=368cd8ac4633fabdf3f4fb1c47a250634d6a8347, for GNU/Linux 3.2.0, not stripped
```

#### se puede correr el binario 

```
./bandit20-do whoami
bandit20

```

#### si leo dentro de la ruta 

```
./bandit20-do cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

#### era bastante intuitivo i have the pass 

```
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

#### bueno s4vi lo hizo de otra manera

```
./bandit20-do sh
whoami
bandit20
cat /etc/bandit_pass/bandit20
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

-----

# Jugando con conexiones

## bandit 20 ->21

[There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think]

[Hay un binario setuid en el homedirectory que hace lo siguiente: hace una conexión con el localhost en el puerto que especifica como argumento de la línea de comando. Luego lee una línea de texto de la conexión y la compara con la contraseña en el nivel anterior (bandata20). Si la contraseña es correcta, transmitirá la contraseña para el siguiente nivel (bandata21). 
NOTA: Trate de conectarte a tu propio demonio de red para ver si funciona como crees ]

https://blog.desdelinux.net/usando-netcat-algunos-comandos-practicos/

-------------------------------------------------------------------------------------------------------------------

# Jugando con conexiones

## bandit 20 ->21

[There is a setuid binary in the homedirectory that does the following: it makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

**NOTE:** Try connecting to your own network daemon to see if it works as you think]

[Hay un binario setuid en el homedirectory que hace lo siguiente: hace una conexión con el localhost en el puerto que especifica como argumento de la línea de comando. Luego lee una línea de texto de la conexión y la compara con la contraseña en el nivel anterior (bandata20). Si la contraseña es correcta, transmitirá la contraseña para el siguiente nivel (bandata21). 
NOTA: Trate de conectarte a tu propio demonio de red para ver si funciona como crees ]

https://blog.desdelinux.net/usando-netcat-algunos-comandos-practicos/

#### Nos encontramos con un binario el cual tiene permisos setuid pero requiere de una conexion a algun puerto

```
ls
suconnect

bandit20@bandit:~$ ls -l
total 16
-rwsr-x--- 1 bandit21 bandit20 15604 Jun 20 04:07 suconnect

bandit20@bandit:~$ ./suconnect 
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.

bandit20@bandit:~$ ./suconnect 22
Read: SSH-2.0-OpenSSH_9.6p1
ERROR: This doesn't match the current password!
bandit20@bandit:~$ 
```

#### si le paso el password de bandit 20 corriendo por el puerto 22 quizas enganche

```
./suconnect ssh -i '0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO' bandit21@localhost -p 22
Read: SSH-2.0-OpenSSH_9.6p1
ERROR: This doesn't match the current password!
bandit20@bandit:~$ 
```

#### fallo, pero s4vi hace otra cosa

#### abre otra terminal y se conecta a bandit20

```
ssh bandit20@bandit.labs.overthewire.org -p 2220
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

#### En la misma se pone en escucha con netcat por un puerto random

```
nc -lnvp 2020
```

#### desde la otra terminal logeado con bandit20 ejecuto el binario pasandole el puerto 2020

```
./suconnect 2020
```

#### me recibe la coneccion pero si coloco otra cosa que no sea el pass me echa, asiq pongo el pass

```
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
```

#### me devuelve el pass de bandit 21

```
bandit20@bandit:~$ ls

suconnect

bandit20@bandit:~$ nc -lnvp 2020
Listening on 0.0.0.0 2020
Connection received on 127.0.0.1 46958
0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

#### password

```
EeoULMCra2q0dSkYj561DX7s1CpBuOBt
```

----------
#cron
# Abusando de tareas Cron [1-3]
## bandit21 ->22

[A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.]

	[Un programa se está ejecutando automáticamente a intervalos regulares de cron, el programador de trabajo basado en el tiempo. Mire en /etc/cron.d/ para la configuración y vea qué comando está siendo ejecutado. ]

*Cron es un administrador de tareas de Linux que permite ejecutar comandos en un momento determinado, por ejemplo, cada minuto, día, semana o mes. Si queremos trabajar con cron, podemos hacerlo a través del comando **crontab**.*

#### nos dice que miremos en  /etc/cron.d 

```
bandit21@bandit:~$ cd /etc/cron.d
```

#### encontre varios archivos que me dejan verlos

```
bandit21@bandit:/etc/cron.d$ ls
cronjob_bandit22  cronjob_bandit24  otw-tmp-dir
cronjob_bandit23  e2scrub_all       sysstat
```

#### supongo que sera el cronjob_bandit22

```
bandit21@bandit:/etc/cron.d$ ls -l cronjob_bandit22
-rw-r--r-- 1 root root 120 Jun 20 04:07 cronjob_bandit22

```

#### vemos que permisos tiene

```
bandit21@bandit:/etc/cron.d$ ls -l cronjob_bandit22
-rw-r--r-- 1 root root 120 Jun 20 04:07 cronjob_bandit22
```

#### le hacemos un cat

```
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

#### apunta a un archivo el cual es una tarea crontab asi que si lo cateamos

```
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

#### apunta a un fichero el cual se ejecuta constantemente con permisos 644 (podemos leerlo)

```
bandit21@bandit:/etc/cron.d$ ls -l /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
-rw-r--r-- 1 bandit22 bandit22 33 Jul  4 19:40 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

#### sin mas lo cateamos y pim pam pum password

```
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv

tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q
```

---------------------

# Abusando de tareas Cron [2-3]
## bandit22 ->23

[A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.]

[Un programa se está ejecutando automáticamente a intervalos regulares de cron, el programador de trabajo basado en el tiempo. Mire en /etc/cron.d/ para la configuración y vea qué comando está siendo ejecutado. 
NOTA: Mirar los guiones de concha escritos escritos por otras personas es una habilidad muy útil. El guión de este nivel se hace intencionalmente fácil de leer. Si usted está teniendo problemas para entender lo que hace, trate de ejecutarlo para ver la información de depuración que imprime. ]

https://blog.desdelinux.net/cron-crontab-explicados/
#### Bueno siguiendo los pasos anteriores llegue hasta aca

```
bandit22@bandit:~$ cd /etc/cron.d

bandit22@bandit:/etc/cron.d$ ls

cronjob_bandit22  cronjob_bandit24  otw-tmp-dir
cronjob_bandit23  e2scrub_all       sysstat

bandit22@bandit:/etc/cron.d$ ls cronjob_bandit23 

cronjob_bandit23

bandit22@bandit:/etc/cron.d$ ls -l cronjob_bandit23

-rw-r--r-- 1 root root 122 Jun 20 04:07 cronjob_bandit23

bandit22@bandit:/etc/cron.d$ cat cronjob_bandit23

@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null       
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null

bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh 

#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget

```

{@reboot quiere decir que cuando se reinicie el equipo va ejecutar esta tarea @reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null }

#### explicacion del script


#### myname es el output del whoami/ y es bandit23

```
myname=$(whoami)
```

#### mytarget es el hash del nombre bandit en md5sum y el cut es para quedarse con solo el hash del output

```
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
```

#### osea = 

```
 echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```

#### hace una copia del hash y lo envia  por las siguientes rutas

```
echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
```

#### Entonces si cateo ambas rutas?

```
bandit22@bandit:/etc/cron.d$ cd /etc/bandit_pass/
bandit22@bandit:/etc/bandit_pass$ ls
bandit0   bandit13  bandit18  bandit22  bandit27  bandit31  bandit6
bandit1   bandit14  bandit19  bandit23  bandit28  bandit32  bandit7
bandit10  bandit15  bandit2   bandit24  bandit29  bandit33  bandit8
bandit11  bandit16  bandit20  bandit25  bandit3   bandit4   bandit9
bandit12  bandit17  bandit21  bandit26  bandit30  bandit5
bandit22@bandit:/etc/bandit_pass$ cat bandit23
cat: bandit23: Permission denied
bandit22@bandit:/etc/bandit_pass$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349

```

#### obtenemos el password

```
0Zf11ioIjMVN551jX3CmStKLYqjk54Ga

-----------------------------------------------------------------------------------------------------------------------------

## bandit 23->24

**AVISO**: OverTheWire ha estado realizando una serie de cambios, comunicaros que ahora en vez de la ruta ‘**/var/spool/bandit24/**‘ tenéis que emplear la ruta ‘**/var/spool/bandit24/foo**‘, por lo demás, todo sigue igual.

Y con esta clase ¡concluimos las tareas Cron!

Esta parte deciros que es fundamental, sobre todo de cara a los módulos de Hacking que vamos a estar tocando en la academia, pues en muchas de las ocasiones veremos cómo nos será necesario no sólo identificar qué tareas se ejecutan en el sistema a intervalos regulares de tiempo, sino también saber cómo poder abusar de estas para elevar nuestros privilegios.

¡Pero lo bueno se hace esperar!, lo primero es lo primero, tenemos que cimentar las bases y no construir la casa por el tejado.

[A program is running automatically at regular intervals from **cron**, the time-based job scheduler. Look in **/etc/cron.d/** for the configuration and see what command is being executed.

**NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

**NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…]

[Un programa se está ejecutando automáticamente a intervalos regulares de cron, el programador de trabajo basado en el tiempo. Mire en /etc/cron.d/ para la configuración y vea qué comando está siendo ejecutado. 
NOTA: Este nivel requiere que crees tu propio primer guión. Este es un paso muy grande y deberías estar orgulloso de ti mismo cuando venciste a este nivel.

NOTA 2: Ten en cuenta que tu script de shell se elimina una vez ejecutado, por lo que puede querer mantener una copia alrededor de... ]

#### se esta ejecutando un script en

```
bandit23@bandit:/etc/cron.d$ cat cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:/etc/cron.d$ 
```

#### vamos a verlo

```
bandit23@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit24.sh 
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

#### si cateamos el archivo, permiso denegado

```
 cat /var/spool/bandit24/foo/
cat: /var/spool/bandit24/foo/: Permission denied

# tenemos permiso w-x

ls -l /var/spool/bandit24
total 4
drwxrwx-wx 17 root bandit24 4096 Jul  7 19:40 foo
```

#stat ve info sobre el directrio vg:

(bandit23@bandit:/var/spool$ stat bandit24/
  File: bandit24/
  Size: 4096      	Blocks: 8          IO Block: 4096   directory
Device: 259,1	Inode: 539613      Links: 3
Access: (0550/dr-xr-x---)  Uid: (11024/bandit24)   Gid: (11023/bandit23)
Access: 2024-07-07 12:53:08.431240985 +0000
Modify: 2024-06-20 04:07:07.700114670 +0000
Change: 2024-06-20 04:07:07.710114709 +0000
 Birth: 2024-06-20 04:07:07.700114670 +0000)

#### creamos un direcrotio temporal 

```
dir_name=$(mktemp -d)  se guarda la variable y queda mas comodo

echo $dir_name {no me funciono} bah

```

#### creamos un script

```
#!/bin/bash


cp /etc/bandit_pass/bandit24 > /tmp/tmp.Tv8ii0lAK3/bandit24_password.log
chmod +x /tmp/tmp.Tv8ii0lAK3/bandit24_password.log
```

#### ahora fuera le pasamos permisos a otros

```
chmod o+wx /tmp/tmp.Tv8ii0lAK3
```

#### creamos una copia y chekear si se crearon los permisos

```
cp script.sh /var/spool/bandit24/example
```

#watch

```
watch -n 1 ls -l   (esto va monitorear cada segundo lo que pasa en el directorio)
```

#### a s4vi no le funciono y saco el cp por el cat para hacer un verbose

```
#!/bin/bash


cat /etc/bandit_pass/bandit24 > /tmp/tmp.Tv8ii0lAK3/bandit24_password.log
chmod +x /tmp/tmp.Tv8ii0lAK3/bandit24_password.log
```

#### nuevamente copiamos el script

```
cp script.sh /var/spool/bandit24/testing
```

#### esperamos para ver si se ejecutan los comandos

```
watch -n 1 ls -l
```

 


```
--------------------------------------------------------------------------------------------------------------------------------------------
