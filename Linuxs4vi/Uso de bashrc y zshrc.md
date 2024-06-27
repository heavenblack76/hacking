

#### para saber que tipo de shell estoy usando:

```
echo $SHELL
```

#### uso de delimitadores #awk #cut

```
hostname -I                  
192.168.0.82 172.17.0.1 


hostname -I | awk '{print$1}'

192.168.0.82

```

el output sera en mi caso mi ip primera, en caso de que tenga 3 imprimira por pantalla la primera


```
hostname -I | cut -d ' ' -f 1 

192.168.0.82

```

~ -d = delimitador
~ ' ' = como hay un espacio se espasea entre las comillas
~ -f 1 = captura el filed (campo) 1 

#### tambien se puede hacer un string dentro de un script, like:

```
echo "Tu IP privada es: hostname -I | cut -d ' ' -f 1"

Tu IP privada es: hostname -I | cut -d ' ' -f 1

```

#### Para que tome el comando hay que indicarle al sistema que es un string:

```
echo "Tu IP privada es: $(hostname -I | cut -d ' ' -f 1)"

Tu IP privada es: 192.168.0.82

```

#### Tambien se puede con #awk 

```
echo "Tu IP privada es: $(hostname -I | awk '{print$1}')"

Tu IP privada es: 192.168.0.82

```

[Para no poner (/home/heavenblack) lo mas facil es poner (~/) toma el directorio padre del usuario]




En mi caso yo opero con una ZSH, por tanto mi archivo de configuración corresponde al ‘~/.**zshrc**‘. Recuerda que en caso de usar una BASH tu archivo de configuración debería estar situado en ‘**~/.bashrc**‘

Por aquí te dejo algunos enlaces de interés por si quieres entender un poco mejor para qué sirven estos archivos:

- ¿Qué es Bashrc en Linux?: [https://www.compuhoy.com/que-es-bashrc-en-linux/](https://www.compuhoy.com/que-es-bashrc-en-linux/)
- ¿Por qué deberías usar ZSH?: [https://respontodo.com/que-es-zsh-y-por-que-deberia-usarlo-en-lugar-de-bash/](https://respontodo.com/que-es-zsh-y-por-que-deberia-usarlo-en-lugar-de-bash/)

Mi recomendación personal es que utilicéis ZSH en vez de BASH, desde que investiguéis un poco diferencias entre ambas entenderéis por qué merece la pena.
