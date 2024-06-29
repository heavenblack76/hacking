Para introducir texto a un erchivo:

```touch file
	echo "esto es una prueba" > file
	cat file
	esto es una prueba
```

Pero que pasa cuando quiero pasar otra linea, si lo hago de la misma manera se sobre escribe entonces

	```echo "esta es la segunda prueba" >> file```

cada vez que le pones un >> se apila

para ver en que formato esta encriptado una linea:

`#cat /etc/login.defs | grep "ENCRYPT_METHOD`

en el fichero `/ect/shadow` se muestran las contrasenias

Para cambiear un grupo:

`#chgrp heavenblack root`

Pero si queremos cambiarle algunos detalles por ejempli:

`#chmod u-w,g+xr,o-x,o+r file`

------------------------------------------------------------------------------------------------------------------

### OCTAL 

| r   |  w  | x   |
| --- | :-: | --- |
| 4   |  2  | 1   |


