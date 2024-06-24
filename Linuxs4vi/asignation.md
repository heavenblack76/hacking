### Gestionando usuarios

```
#mkdir pepe
#useradd pepe -S /bin/bash -d /home/pepe
```
#### Se crea un usuario, alque se le asigna una shell y un directorio personal

```
cat /etc/passwd | grep pepe

pepe:x:1001:1003::/home/pepe:/bin/bash
```

#### Asignacion de contrasenia:

```
#passwd pepe
```

#### Asignacion del grupo:

```
#chgrp pepe pepe
```

#### Asignacion de usuario:

```
#chown pepe pepe
```

#### Se simplifica con:

```
#chown pepe:pepe pepe
```

#### Creamos un nuevo grupo:

```
#groupadd Alumnos
```

#### Podemos ver el identificador con:

```
#cat  /etc/group | grep Alumnos
```

#### Ahora podemos agregar al grupo Alumnos al usuario pepe:

```
#usermod -a -G Alumnos pepe
```

#### Le decimos que no pueden leer ni ejecutar un fichero que creamos

```
#chmod o-rx prueva
```

#### Pero quiero que el grupo alumnos pueda leer y ejecutar el fichero prueba

```
#chgrp Alumnos prueba
```
