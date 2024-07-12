## ¡Nuestro primer proyecto de Bash!

## ¿Qué vamos a estar haciendo?

Vamos a adentrarnos en el maravilloso mundo del lenguaje de comandos Bash. Nuestro objetivo a partir de ahora consistirá en crear una serie de herramientas de utilidad a través de las cuales podremos profundizar mucho más en el scripting en Bash.

## ¿En qué consiste este primer proyecto?

Este primer proyecto va a consistir en crear un buscador, pero no un buscador cualquiera.

Los que me seguís en mis directos de Twitch, sabréis que tenemos un buscador de máquinas, ¿no?, concretamente este de aquí:

-  [https://htbmachines.github.io/](https://htbmachines.github.io/)

Este buscador nos permite filtrar por palabras clave de cara a las máquinas que resolvemos de la plataforma de HackTheBox. Podemos filtrar por un nombre de máquina, obtener información de la máquina en base a una IP dada, filtrar por el sistema operativo, filtrar por nombres de certificación, filtrar por la dificultad de una máquina e incluso por las técnicas vistas en cada una de ellas.

Digamos que es un buscador que lo tiene todo. Pero claro… es vía web, molaría que fuera desde consola, ¿verdad?, pues en eso va a consistir este primer proyecto. Vamos a crear una herramienta que nos actúe como buscador para representar los resultados en un formato estructurado que nos permita obtener rápidamente por consola lo que queremos.

Si aún así sigues sin entender bien cuál es el objetivo de este primer proyecto, ¡te recomendamos que continúes a la siguiente clase!, ya que ahí te lo explicaremos de forma más detallada.

# Scripting en Bash [1-6]

A modo de resumen, en esta clase hemos estado tocando los siguientes puntos:

- Creación de un menú con GETOPTS que nos permita alternar entre una serie de funciones existentes
- Creación de funciones
- Uso y empleo de condicionales en Bash
- Control del flujo del programa frente a Ctrl+C

#### creamos un directorio htbmachine dentro de /opt y lo hacemos como usuario comun

```
cd /opt
sudo su
mkdir htbmachine 
chown heavenblack:heavenblack htbmachine
exit 
heavenblack
touch htbmachine.sh
chmod +x !$
nvim htbmachine.sh
```

#### comenzamos con el script

```
#!/bin/bash

#Colours
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"


function (){

echo -e "\n\n${redColour}[!] Saliendo...${endColour}\n"
exit 1
}

#CRTL+C
trap ctrl_c INT 

sleep 10


```

#### lanzamos una peticion al link 

```
curl -X GET https://htbmachines.github.io/

	aca nos aparece el codigo html
```

#### si lo bateamos nos muestra un verbose de curl, si no lo queremos ver -s

	(paru -S js-beautify) en arch para que sea mas lindo el code en js
		(apt install js-beautify)

```
curl -X GET https://htbmachines.github.io/bundle.js | js-beautify | grep Tentacle

nos devuelve el codigo mas linfo y con el grep filtra por la palabra pero tarda
```

#### traemos el bundle 

```
curl -X GET https://htbmachines.github.io/bundle.js > bunle.js
```

#### ahora tenemos el bundle en local y podemos ver el cod mamejor

```
cat bundle.js | js-beautify
```

#### quiero que el output de !$ lo metas dentro de bundle pero sin que me borres lo anterior

```
cat bundle.js | js-beautify | sponge bundle.js
```

#### creamos un menu con parametros para la ayuda de la tool

```
#!/bin/bash

#Colours
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"

function (){

echo -e "\n\n${redColour}[!] Saliendo...${endColour}\n"
exit 1
}

#CRTL+C
trap ctrl_c INT 

function helpPanel(){
	echo -e "\n[+] Uso:"
}
function searchMachine(){
	machineName="$1"
	echo $"machineName"

}
#indicadores
declare -i parameter_counter=0

while getopts "m:h" arg; do   [habra solo dos parametros m de machine y h de ayuda]

	case $arg in    
	esac
		m) machineName=$OPTARG;$let parameter_counter+=1;;
		h);;         [para definir parametros finalizan en doble ";;"]
done

if [$parameter_counter -eq 1]; then
	searchMachine $machineName
else
	helpPanel
fi
#sleep 10

```

----------------------------------------------
# Scripting en Bash [2-6]

```
#!/bin/bash

#Colours
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"

function (){

echo -e "\n\n${redColour}[!] Saliendo...${endColour}\n"
exit 1
}

#CRTL+C
trap ctrl_c INT 

function helpPanel(){
	echo -e "\n${yellowColour}[+]${endColour}${grayColour}Uso:${endColour}"
	echo -e "\t${purpleColour}m${endColour}) Buscar por un nombre de maquina" #\t=tabulador
	echo -e "\th) Mostrar este panel de ayuda\n" #muestra el menu de ayuda
}
function searchMachine(){
	machineName="$1"
	echo $"machineName"

}
#indicadores
declare -i parameter_counter=0

while getopts "m:h" arg; do   [habra solo dos parametros m de machine y h de ayuda]

	case $arg in    
	esac
		m) machineName=$OPTARG;$let parameter_counter+=1;;
		h);;         [para definir parametros finalizan en doble ";;"]
done

if [$parameter_counter -eq 1]; then
	searchMachine $machineName
else
	helpPanel
fi
```