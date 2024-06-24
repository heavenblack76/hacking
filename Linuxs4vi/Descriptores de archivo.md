`exec 3<> file` se crea un archivo 
`file file` detecta que tipo de archivo es 'file' en base a los primeros bytes "los magic numbers"

Entonces los descriptores funcionan redireccionando ejemplo, siguiendo con el codigo:

`whoami >&3
`cat file`
`root`


![[exec.drawio.png]]

Se pueden crear copias de que 5 > 7 ; 8 entonces al hacerle un cat tanto a 7 como a 8 tendran las mismas propiedades que 5

cerrando `exec 7>&0-`


```exec 5<> exmple
	whoami >&5
	cat whoami
	root
	exec 6>&5-   "se crea el archivo 6 y se cierra el 5"
	pwd >&6
	cat example     "muestra tanto el output del 5 como del 6"
	root
	/home/heave.../
```

---

Los descriptores de archivos son una parte fundamental del sistema operativo Unix y Linux. Son identificadores abstractos que el kernel utiliza para acceder a archivos y otros recursos de E/S (entrada/salida) como sockets y pipes. Cada descriptor de archivo es un número entero no negativo que representa un canal de comunicación abierto en un proceso.

### Tipos Comunes de Descriptores de Archivos

Por convención, los tres primeros descriptores de archivos son:
1. **Entrada estándar (stdin)**: Descriptor 0.
2. **Salida estándar (stdout)**: Descriptor 1.
3. **Error estándar (stderr)**: Descriptor 2.

### Usos de los Descriptores de Archivos

Los descriptores de archivos se utilizan para operaciones de lectura y escritura en archivos y otros recursos de E/S. Aquí hay algunos ejemplos de cómo se usan en la programación y en la línea de comandos:

### Ejemplos en la Línea de Comandos

#### Redirección de Salida

Redirigir la salida estándar de un comando a un archivo:

```bash
ls > output.txt
```

Redirigir la salida de error estándar a un archivo:

```bash
ls nonexistentfile 2> error.txt
```

Redirigir tanto la salida estándar como la de error a un archivo:

```bash
ls existingfile nonexistentfile > all_output.txt 2>&1
```

#### Uso de Descriptores de Archivos Personalizados

Puedes usar descriptores de archivos personalizados (más allá de 0, 1 y 2) para redirecciones más complejas:

```bash
# Abrir un nuevo descriptor de archivo 3 y redirigir la salida estándar a un archivo
exec 3> custom_output.txt

# Escribir algo en el descriptor 3
echo "Esto va a custom_output.txt" >&3

# Cerrar el descriptor 3
exec 3>&-
```

### Ejemplos en Programación

En lenguajes de programación como C, los descriptores de archivos se utilizan en llamadas al sistema para manipular archivos:

#### Abrir un Archivo

```c
int fd = open("archivo.txt", O_RDWR);
if (fd == -1) {
    perror("Error abriendo el archivo");
    exit(1);
}
```

#### Leer desde un Archivo

```c
char buffer[128];
ssize_t bytes_read = read(fd, buffer, sizeof(buffer));
if (bytes_read == -1) {
    perror("Error leyendo el archivo");
    close(fd);
    exit(1);
}
```

#### Escribir en un Archivo

```c
const char *data = "Hola, mundo!";
ssize_t bytes_written = write(fd, data, strlen(data));
if (bytes_written == -1) {
    perror("Error escribiendo en el archivo");
    close(fd);
    exit(1);
}
```

#### Cerrar un Archivo

```c
if (close(fd) == -1) {
    perror("Error cerrando el archivo");
    exit(1);
}
```

### Ventajas de los Descriptores de Archivos

- **Abstracción**: Los descriptores de archivos proporcionan una forma abstracta de manejar diferentes tipos de E/S sin preocuparse por los detalles específicos del dispositivo.
- **Flexibilidad**: Permiten redirigir la entrada y salida de comandos y programas de manera flexible.
- **Simplicidad**: Facilitan la escritura de programas que pueden trabajar con diferentes tipos de dispositivos de E/S de manera uniforme.

### Conclusión

Los descriptores de archivos son un concepto esencial en Unix y Linux para manejar operaciones de entrada y salida. Permiten redirecciones y manipulaciones flexibles y poderosas tanto en la línea de comandos como en la programación. Conocer cómo funcionan y cómo usarlos efectivamente es crucial para cualquier administrador de sistemas o desarrollador en estos entornos.