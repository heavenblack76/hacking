
#### Creo un archivo con touch .sh le doy permisos de ejecucion

```
ip a | grep wlan0mon | tail -n 1 

			or
ip a | grep wlan0mon | awk 'NR==2'
```
*no me estaria funcionando*

#### Che me quiero quedar con el primer argumento, tomando como delimitador el slash:

```
ip a | grep wlan0mon | awk 'NR==2' | awk '{print $1}' FS="/"

Es lo mismo que:

	ip a | grep wlan0mon | awk 'NR==2' | awk '{print $1}' cut -d '/' -f 1

Convertir las barras a espacio

	ip a | grep wlan0mon | awk 'NR==2' | awk '{print $1}' | tr '/' ' ' | awk '{print $2}'

Ahora me quedo con el 2do argumento
```

#### Ahora ya creado el fichero .sh le introducimos, lo mismo que a tu mama

```
!#/bin/bash

	echo \n"[+] Esta es tu direccion IP privada -> $(ip a | grep wlan0mon | awk 'NR==2' | awk '{print $1}' FS="/")\n"


```

#### Saltos de linea

```
nvim -e fiechero.sh
```

#### Le podemos agregar colores al script con

```
!#/bin/bash

#Colours
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"

echo \n"[+] Esta es tu direccion IP privada -> $(ip a | grep wlan0mon | awk 'NR==2' | awk '{print $1}' FS="/")\n"
```

#### Ahora le quiero dar color a la palabra "privada"

```
${grayColor}privada${endColor}
```

