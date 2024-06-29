### ¿Qué es el Sticky Bit?

- **Sticky Bit**: Es un permiso que se aplica a los directorios y permite que solo el propietario de un archivo dentro de ese directorio (o el usuario root) pueda eliminar o renombrar ese archivo, incluso si otros usuarios tienen permisos de escritura en el directorio.

### Usos del Sticky Bit

El Sticky Bit se usa comúnmente en directorios de sistemas de archivos donde múltiples usuarios pueden escribir, como `/tmp`. Sin el Sticky Bit, cualquier usuario con permisos de escritura podría eliminar o renombrar archivos de otros usuarios en estos directorios.

### Cómo Verificar el Sticky Bit

Puedes verificar si el Sticky Bit está establecido en un directorio usando el comando `ls -ld`:


`ls -ld /tmp

La salida podría ser algo como esto:

`drwxrwxrwt 10 root root 4096 Jun 14 12:34 /tmp


Aquí, la `t` en lugar de la `x` en los permisos del directorio indica que el Sticky Bit está activo.

### Cómo Establecer el Sticky Bit

Para establecer el Sticky Bit en un directorio, puedes usar el comando `chmod` con el modo `+t`:

`sudo chmod +t /ruta/al/directorio`

### Cómo Remover el Sticky Bit

Para remover el Sticky Bit de un directorio, usa `chmod` con el modo `-t`:

`sudo chmod -t /ruta/al/directorio`

### Ejemplos

#### Establecer el Sticky Bit en `/shared`:

`sudo chmod +t /shared`

Ahora, solo los propietarios de los archivos dentro de `/shared` pueden eliminarlos o renombrarlos, además del usuario root.

#### Verificar los Permisos:

`ls -ld /shared`

La salida podría ser algo así:

`drwxrwxrwt 5 user group 4096 Jun 14 12:34 /shared`

El `t` en los permisos indica que el Sticky Bit está activo.

### Resumen

El Sticky Bit es una característica de permisos que mejora la seguridad en directorios compartidos, permitiendo que solo los propietarios de archivos puedan eliminarlos o renombrarlos. Se utiliza principalmente en directorios donde múltiples usuarios tienen permisos de escritura, como `/tmp`, para prevenir la eliminación accidental o maliciosa de archivos por parte de otros usuarios.
