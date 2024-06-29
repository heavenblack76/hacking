### Con el querido "find" podemos buscar lo que sea, incluso a tu hermana

```
find / -name 'yoursis' 2>/dev/null | xargs ls -l
```

#Find buscara desde la raiz #/  el (-name) 'yoursis' y todo error que aparezca, lo tira al pozo ciego | el resultado #xargs lo listara 

#### De la misma manera se pueden buscar directorios con el parametro *-type d* o ficheros con *-type f*

```
find / -name 'yoursis' -type d 2>/dev/null | xargs ls -l
```

#### filtro por escritura:

```
find /home/ -name -writable 'yoursis' -type f 2>/dev/null | xargs ls -l

```

#### Ahora supongamos que quiero buscar pero no me acuerdo porque soy un pelotudo, ergo, empieza con "dex" entonces :

```
find / -name dex\* 2>/dev/null
```

#### Lo mismo con "la_puta_de_tu_hermana.txt"

```
find / -name \*de_tu_\* 
```

#### Es maravilloso

```
find / -name \*de_tu_herm\*.txt 
```


---

## ¿Por qué usar los comandos find y locate en Linux?

Los nuevos usuarios de Linux suelen decir que les confunde la ubicación de sus archivos en un servidor. Esto puede deberse a que la mayoría de la gente está acostumbrada a operar con Windows o MacOS, que tienen un diseño de directorios más claro y fácil de usar.

Aunque hay algo de verdad en esto, Linux da a los usuarios más opciones para buscar archivos usando ciertos comandos. Además de la búsqueda basada en filtros comunes, también es posible encontrar archivos por permisos de usuario, tamaño, marcas de tiempo, etc.

Lo que es genial es que, una vez que entiendes los comandos, buscar archivos en tu plataforma Linux es increíblemente fácil.

Para hacer eso, utilizaremos los comandos **find** y **locate** en Linux.

Una cosa importante a tener en cuenta es que usaremos un [VPS con Ubuntu](https://www.hostinger.es/servidor-vps) en esta guía. Dicho esto, los pasos también deberían funcionar para Debian, CentOS, o cualquier otra distribución de Linux. Si no sabes cómo conectarte a un VPS, puedes seguir [esta guía](https://www.hostinger.es/tutoriales/conectar-usando-terminal-putty-ssh/) antes de continuar.

## Usando el comando find en Linux

Comencemos explicando el comando **find** en Linux y cómo usarlo.

### La sintaxis básica

El comando **find** es el comando que más se utiliza para encontrar y filtrar archivos en Linux. El diseño básico de este comando es el siguiente:

find <startingdirectory> <options> <search term>

Comienza con la palabra clave **find**, que alerta a Linux de que lo que sigue se refiere a la búsqueda de un archivo. El argumento **<startingdirectory>** es el punto de origen de donde deseas iniciar la búsqueda. Puede ser reemplazado con varios argumento, incluyendo:

- **/ (slash)** – busca en todo el sistema.
- **. (punto)** – busca en la carpeta en la que estás trabajando actualmente (directorio actual).
- **~ (tilde)** – para buscar desde tu directorio home.

#### Consejo profesional

Para averiguar tu directorio actual, usa el comando **pwd**.

El segundo argumento **<options>** se usa para tu archivo. Este podría ser el nombre, tipo, fecha de creación del archivo, etc. El tercer argumento **<searchterm>** es donde se especificará el término de búsqueda relevante.

### Formas de utilizar el comando find en Linux

Echemos un vistazo a las diversas opciones que Linux proporciona a los usuarios:

### Búsqueda por nombre

Por supuesto, el método más común y obvio para buscar un archivo es usar su nombre. Para ejecutar una consulta de búsqueda simple usando el nombre del archivo, usa el comando **find** de la siguiente manera:

find . -name my-file

Usamos la opción **-name** y buscamos un archivo llamado **my-file**. Ten presente que comenzamos la búsqueda en nuestro directorio actual usando el argumento **. (punto)**.

Recuerda que el argumento **-name** busca términos distinguiendo entre mayúsculas y minúsculas en Linux. Si conoces el nombre del archivo, pero no estás seguro de su las mayúsculas y minúsculas, usa el comando find de esta manera:

find . -iname my-file

También puedes buscar todos los archivos sin una palabra clave en el nombre. Puedes hacer esto de dos maneras. El primer método implica el uso de la palabra clave **-not** de la siguiente manera:

find . -not -name my-file

En el segundo, podemos usar el signo de exclamación (**!**), aunque debe estar precedido por el identificador de escape (\) para que Linux sepa que este es parte del comando **find**.

find . \! -name my-file

También puedes buscar varios archivos con un formato común como **.txt**:

find . -name “*.txt”

Esto listará todos los archivos de texto comenzando con la carpeta actual.

Finalmente, si deseas buscar un determinado archivo por nombre y eliminarlo, usa el argumento **-delete** después del nombre del archivo:

find . -name my-file -delete

### Búsqueda por tipo

Linux permite a los usuarios listar toda la información basada en sus tipos. Hay varios filtros que puedes usar:

- **d** – directorio o carpeta
- **f** – archivo normal
- **l** – enlace simbólico
- **c** – dispositivos de caracteres
- **b** – dispositivos de bloque

Un ejemplo simple del uso del tipo de archivo para la búsqueda se puede ver a continuación:

find / -type d

Esto mostrará una lista de todos los directorios presentes en tu sistema de archivos, al haber comenzado la búsqueda desde nuestro directorio raíz con el símbolo de barra inclinada /.

También puedes concatenar las opciones **-type** y **-name** para hacer tus búsquedas aún más específicas:

find / -type f -name my-file

Esto buscará archivos llamados **my-file**, excluyendo directorios o enlaces.

### Búsqueda por fecha

Si quieres buscar archivos en función de su fecha de acceso y las registros de fecha de modificación, Linux te ofrece las herramientas para hacerlo. Hay 3 registros de tiempo de los cuales Linux realiza seguimiento en los archivos:

- **Tiempo de acceso (-atime)** – Fecha más reciente en que el archivo fue leído o escrito.
- **Tiempo de modificación (-mtime)** – Fecha más reciente en que se modificó el archivo.
- **Hora de cambio (-ctime)** – Fecha más reciente en que se actualizaron los metadatos del archivo.

Esta opción debe usarse con un número que especifica cuántos días pasaron desde que se accedió, modificó o cambió el archivo:

find / -atime 1

Este comando mostrara todos los archivos a los que se accedió hace un día desde el momento actual.

Podemos hacer que nuestras consultas sean más precisas agregando los signos **más (+)** y **menos (-)** precediendo al número de días. Por ejemplo:

find / -mtime +2

Esto listará todos los archivos que tienen un tiempo de modificación de más de dos días.

Para buscar todos los archivos cuyos metadatos se actualizaron hace menos de un día, ejecuta lo siguiente:

find / -ctime -1

Aunque no se usan seguido, hay algunos argumentos adicionales que también están relacionados con las búsquedas por fecha. El argumento **-mmin** busca archivos modificados en base a minutos. Se puede usar así:

find / -mmin -1

Además, el argumento **-newer** se puede usar para comparar la antigüedad de dos archivos y encontrar el más reciente.

find / -newer my-file

Obtendrás todos los archivos que han sido modificados hace menos tiempo que tu archivo.

### Búsqueda por tamaño

Linux te brinda la opción de buscar archivos según sus tamaños. La sintaxis básica para buscar archivos por tamaño es:

find <startingdirectory> -size <size-magnitude> <size-unit>

Puedes especificar las siguientes unidades de tamaño:

- **c** – bytes
- **k** – kilobytes
- **M** – megabytes
- **G** – gigabytes
- **b** – trozos de 512 bytes

Un ejemplo simple de cómo usar el comando **find** de Linux para los tamaños de archivo es el siguiente:

find / -size 10M

Esto buscará en tu sistema archivos que tengan exactamente 10 megabytes de tamaño. Al igual que cuando buscaste en función del tiempo, puedes filtrar aún más tus búsquedas con los signos más y menos:

find / -size +5G

El comando anterior listará todos los archivos de tu disco que tengan más de 5 Gigabytes de tamaño.

### Búsqueda por propiedad

Linux te da la capacidad de especificar tus búsquedas según la propiedad del archivo. Para buscar archivos de un determinado propietario, se debe ejecutar el siguiente comando:

find / -user john

Esto devolverá una lista de todos los archivos que posee el usuario llamado **john**. Similar a los nombres de usuario, también podemos buscar archivos a través de nombres de grupo:

find / -group classroom

### Búsqueda por permisos

Los usuarios pueden buscar archivos basados ​​en los permisos de los archivos con la opción **-perm**. Por ejemplo:

find / -perm 644

En Linux, **644** corresponde a permisos de lectura y escritura. Lo que significa que este comando buscará todos los archivos que solo tienen permisos de lectura y escritura. Puedes jugar con esta opción un poco más, así:

find / -perm -644

Al agregar un guión, se mostrarán todos los archivos que tengan al menos el permiso 644.

Puedes [leer más](http://linuxcommand.org/lc3_lts0090.php) (en inglés) sobre los permisos y los diversos códigos correspondientes a otros permisos de Linux.

### Otras opciones útiles

Además de todos estos métodos de búsqueda de archivos, hay otras opciones útiles que deberías recordar.

Por ejemplo, para buscar archivos y carpetas vacíos en tu sistema, usa lo siguiente:

find / -empty

Del mismo modo, para buscar todos los ejecutables guardados en tu disco, utiliza la opción **-exec**:

find / -exec

Para buscar archivos legibles, puedes ejecutar el siguiente comando:

find / -read

Como puedes ver, hay un montón de opciones disponibles para que los usuarios puedan adaptar sus consultas perfectamente de acuerdo a sus necesidades. Veamos ahora el otro comando que se puede usar para buscar archivos en Linux.

## Usando el comando locate en Linux

El comando **locate** es una alternativa útil, ya que es más rápido que **find** para realizar búsquedas. Eso se debe a que sólo escanea tu base de datos de Linux en lugar de todo el sistema. Además, la sintaxis es más fácil de escribir.

### Cómo instalar el paquete locate

Por defecto, Linux no viene con el comando **locate** preinstalado. Para obtener el paquete, ejecuta los siguientes comandos en tu terminal:

sudo apt-get update
sudo apt-get install mlocate

### La sintaxis básica

Ahora puedes usar el comando para buscar archivos usando esta sintaxis:

locate [my-file]

El comando **locate** estándar a veces puede mostrar archivos que han sido eliminados, si la base de datos no fue actualizada. La mejor solución es actualizar manualmente la base de datos ejecutando lo siguiente:

sudo updatedb

### Cómo usar el comando locate de Linux

Te compartiremos a continuación las aplicaciones más comunes del comando **locate** de Linux.

### Buscar por el nombre exacto del archivo

La sintaxis básica sólo te permite buscar archivos que contengan el término de búsqueda. Si quieres obtener el archivo con el nombre exacto, puedes utilizar la opción **-r** y añadir el símbolo de dólar (**$**) al final del término de búsqueda, por ejemplo:

locate -r my-file$

### Contar el número de archivos

Para saber cuántos archivos aparecen en el resultado de la búsqueda, introduce **-c** después del comando locate.

locate -c my-file

En lugar de listar todos los archivos, te dará el número total de ellos.

### Ignorar distinción entre mayúsculas y minúsculas

Usa **-i** en tu comando **locate** para ignorar la distinción entre mayúsculas y minúsculas. Por ejemplo:

locate -i my-file

Se mostrarán todos los archivos con este nombre, independientemente de las mayúsculas o minúsculas que se encuentren.

### Mostrar archivos existentes

Como hemos mencionado, el comando **locate** de Linux puede incluso mostrarte un archivo eliminado si no has actualizado la base de datos. Afortunadamente, puedes resolver esto usando la opción **-e**, así:

locate -e my-file

Al hacer esto, sólo obtendrás los archivos que existen en el momento de ejecutar el comando **locate**.

### Desactiva los errores durante la búsqueda

La opción **-q** evitará que cualquier error aparezca cuando se procese la búsqueda. Para hacer esto, simplemente introduce:

locate -q my-file

### Limitar el número de resultados de búsqueda

Si quieres limitar el número de resultados de la búsqueda, **-n <number>** funcionará. Sin embargo, recuerda que debes poner la opción al final de la línea de comandos. Echa un vistazo a este ejemplo:

locate my-file n 10

El script sólo mostrará los primeros 10 archivos que encuentre, incluso aunque haya más.
