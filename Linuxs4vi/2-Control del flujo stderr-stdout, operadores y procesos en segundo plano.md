### Para ejecutar comando en conjunto ### : &&. ; || 

*'punto y coma* se ejecuta sin importar lo que pase antes

`whoami; ls; pwd`

*and &&* si y solo si el primer comando da exitoso

`whoami && ls && cd .. && touch file`

{para ver el codigo de estado de un comando "echo $? si da 0 quiere decir que es exitoso}

*or ||* se ejecuta de todas formas

`whoam || ls || cd .. || pwd`

Los errores se pueden pasar directamente al 2>/dev/null osea al agujero negro

Tambien si da error porque el comando esta bien pero el programa no:

`cat /etc/host >/dev/null 2>&1`

y no se vuelve a ver; o sino:

`cat /etc/host &>/dev/null`

Funciona a la hora de abrir un programa, como ser:

`wireshark &>/dev/null & disown`   lo acabo de probar con obsidian y de ahora en mas abro las apps asi

Y si le agregas un disown entonces no vas a tener problemas para cerrarlo
