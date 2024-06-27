## Instalamos tmux

### Nos movemos a Desktop

```
apt install tmux 
sudo pacman -S tmux
```

#### Pero primero fijate si esta instalado con:

```
apt search tmux
                               tmux terminal multiplexer
pacman search tmux
```

#### Se puede iniciar con

```
tmux 

tmux new -s nopor (se crea un espacio de trabajo llamado 'nopor')
```

### Para agregarle colores y otras boludeces 

~ browser oh my tmux github (gpakosz)

$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .

$$ (instalarlo tanto en root como usr, asi se aplica en ambas) $$

### Comandos: 

	[no voy a poner tmux porque ya me muevo bastante bien dentro de zsh]

ps -faux (lista los procesos del sistema)

y lo que sigue es falopa