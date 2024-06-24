
**whoami** =quien soy 

**id** = nos muestra los grupos

Fijarse esto es importante xq puede que haya un usuario en un grupo en el que se encuentre en riesgo y poder escalar privilegios

Cuando nos volvemos root y volvemos a ser un usuario normal y ponemos sudo su, se guarda un token privilege en el que por cierta cantidad de tiempo nos recuerda el pass

Los grupos existentes se alojan en el archivo **/etc/group

Con whitch se puede ver la ruta absoluta de cada binario, eje whitch whoami: **/usr/bin/whoami

Osea hay binarios (comandos) que usan una ruta relativa

`echo $PATH
`/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/local/games:/usr/games`

 | pipe es para tratar lo que se esta haciendo, por ejemplo: 

`/bin/cat /etc/group | grep "root"`

#grep sirve para filtrar, en este caso filtra la palabra root y si vamos a ir a seleccionarla le agregamos *-n* que nos muestra la linea en donde se encuentra

Hay otra variante de whitch que es **command -v**

El directorio personal se puede ver con *echo $HOME* 

ver los tipos de shell existentes:
#cat /etc/shells
`/bin/sh
`/usr/bin/sh
`/bin/bash
`/usr/bin/bash
`/bin/rbash
`/usr/bin/rbash
`/bin/dash
`/usr/bin/dash
`/usr/bin/pwsh
`/opt/microsoft/powershell/7/pwsh
`/usr/bin/tmux
`/usr/bin/screen
`/bin/zsh
`/usr/bin/zsh`

Se pueden cambiar de shell simplemente nombrandolas

---

