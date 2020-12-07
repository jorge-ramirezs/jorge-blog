---
title: "Como Instalar Bspwm Polybar en Ubuntu 20.10"
date: 2020-11-30T16:55:32-06:00
description: En este artículo te voy a explicar como instalar BSPWM y Polybar en Ubuntu 20.10.
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
  - bspwm
  - polybar
  - ubuntu
series:
  -
categories:
  - Window Manager
  - Manejador de Ventanas
image: /images/posts/bspwm-polybar-ubuntu/Ubuntu_bspwm_Blog.png
---

## En YouTube

### Parte 1: La instalación

{{< youtube s18VrzOns9o >}}

### Parte 2: La Configuración

{{< youtube 5qny4p3w_bE >}}

## Introducción

### ¿Qué es BSPWM?

Según la [documentación oficial](https://github.com/baskerville/bspwm):

> **bwspwm** es un administrador de ventanas en mosaico que representa las ventanas como las hojas de un árbol binario completo.
> Solo responde a eventos [ X Window System ](https://en.wikipedia.org/wiki/X_Window_System) y los mensajes que recibe en un conector dedicado.
> bspc es un programa que escribe mensajes en el socket de bspwm.
> bspwm no maneja ninguna entrada de teclado o puntero: se necesita un programa de terceros (por ejemplo,[ sxhkd ](https://github.com/baskerville/sxhkd)) para traducir eventos de teclado y puntero a invocaciones bspc.

## Instalar Paquetes

Afortunadamente Ubuntu ya tiene listos los paquetes que necesitamos instalar, así entonces solo necesitamos abrir una terminal y ejecutar el siguiente comando:

```bash
$ sudo apt-get install bspwm polybar suckless-tools
```

**Atención:** sxhkd es una dependencia de bspwm, así que no es necesario mencionarlo en el comando anterior, solo verifica que este paquete también se instale.

## Crear Archivos de Configuración

El siguiente paso es crear, o mejor dicho, copiar los archivos de configuración de ejemplo y modificarlos a nuestro gusto:

Primero vamos a crear los directorios que contendrán los archivos de configuración:

```bash
$ mkdir ~/.config/bspwm
$ mkdir ~/.config/sxhkd
$ mkdir ~/.config/polybar
```

El siguiente paso es copiar los archivos de ejemplo a nuestros nuevos directorios de configuración:

```bash
$ cp /usr/share/doc/bspwm/examples/bspwmrc ~/.config/bspwm/
$ cp /usr/share/doc/bspwm/examples/sxhkdrc ~/.config/sxhkd/
$ cp /usr/share/doc/polybar/config ~/.config/polybar/
```

## Modificar los archivos de configuración:

Una vez que tenemos los archivos de configuración en nuestro directorio, lo siguiente es modificarlos para poderlos utilizar.

### Primero modificamos el arcvhivo "sxhkdrc"

Por defecto, sxhkd utiliza la terminal "urxvt" que probablemente no tengas instalada, por lo tanto debemos indicarle que mejor utilize "gnome-terminal".

Para eso vamos a requerir un editor de textos como "nano", "vim", o incluso "gedit" puede funcionar, utliza el que más se acomode a tu gusto:

Para motivos de este tutorial y para no meternos en muchos problemas voy a utilizar "gedit" en el siguiente comando:

```bash
$ gedit ~/.config/sxhkd/sxhkdrc
```

Ahora sustituye "urxvt" por "gnome-terminal" (Sin las comillas), guarda los cambios con "CTRL+s" y cierra el archivo.

![sxhkdrc](/images/posts/bspwm-polybar-ubuntu/sxhkdrc1.png)

### Segundo, configuramos "Polybar"

A continuación vamos a configurar Polybar, aqui lo que debemos hacer es crear un archivo que es el que se va a encargar de ejecutar Polybar.

```bash
$ gedit ~/.config/polybar/launch.sh
```

Este comando abrirá una ventana en blanco porque se supone no tenemos este archivo, ahora copia lo siguiente y pegalo en gedit.

```txt
#!/usr/bin/env bash

# Terminate already running bar instances
killall -q polybar
# If all your bars have ipc enabled, you can also use
# polybar-msg cmd quit

# Launch bar1 and bar2
echo "---" | tee -a /tmp/polybar1.log /tmp/polybar2.log
polybar example >>/tmp/polybar1.log 2>&1 & disown

echo "Bars launched..."
```

Guarda el archivo y cierra "gedit".

![launch.sh](/images/posts/bspwm-polybar-ubuntu/launch_sh.png)

Ahora debemos asegurarnos de que el archivo que acabamos de crear sea ejectuable, para eso, corre el siguiente comando:

```bash
$ chmod +x ~/.config/polybar/launch.sh
```

### Por último, configuramos "bspwmrc"

Ahora lo que nos resta, es decirla e bspwm que ejecute Polybar cuando se inicie, para eso, modificamos el siguiente archivo:

```bash
$ gedit ~/.config/bspwm/bspwmrc
```

Agrega la siguiente línea:

```txt
$HOME/.config/polybar/launch.sh
```

![Agrega launch.sh en bspwmrc](/images/posts/bspwm-polybar-ubuntu/bspwmrc.png)

Guarda y Cierra el archivo.

## Salir de Sesión e Iniciar BSPWM.

Con lo anterior ya dejamos la configuración básica, ahora lo que toca es iniciar la sesión pero utilizando BSPWM.

Para eso primero cierra la sesión, luego en la pantalla de inicio, donde te pide tu nombre de usuario y contraseña, selecciona tu usuario y **da clic en el engrane que se encuentra en la parte inferior derecha de la pantalla y selecciona "bspwm"**

![GDM](/images/posts/bspwm-polybar-ubuntu/gdm.png)

## Configurar BSPWM

Cuando inicies la sesión deberás ver únicamente la pantalla en negro y la barra de estatus en la parte superior (Polybar)

![Pantalla inicial](/images/posts/bspwm-polybar-ubuntu/bspwm_inicial.png)

Para abrir una terminal presiona "Super + Enter", donde "Super" es la tecla de "Windows".

### Resolución de pantalla (Opcional)

Para configurar la resolución de tu pantalla es necesario hacerlo con el comando **"xrandr"** como se muestra en el siguiente ejemplo:

1. Verifica el nombre de la pantalla (Screen):

```bash
$ xrandr
```

![xrandr output](/images/posts/bspwm-polybar-ubuntu/xrandr-Screen-Name.png)

En esta imágen se ve la salida de mi maquina virtual que tiene un solo monitor con el nombre "Virtual'1", el asterisco identifica la resolución actual.

Si tu tienes más de un monitor conectado, verifica el nombre del dispositivo al que deseas cambiarle la resolución.

Ejemplo:
![xrandr output dos monitores](/images/posts/bspwm-polybar-ubuntu/xrandr-Screen-Name-2.png)

Ya que identificaste el nombre, para cambiarle la resolución utiliza lo siguiente:

```bash
$ xrandr -s 1920x1080 -r 60.00
```

En caso de cualquier duda puedes consultar el manual teclando "man xrandr" en la terminal.

Si la configuración que seleccionaste funcionó, presiona "Super + alt + r" para que BSPWM se reinicie, esto hará que la Polybar ocupe el ancho de la pantalla correctamente.

Ahora lo que toca es **editar el archivo ~/.config/bspwm/bspwmrc** para que cada vez que inicies BSPWM configure la pantalla correctamente.

```bash
$ gedit ~/.config/bspwm/bspwmrc
```

Y agrega el comando que utilizaste:

```txt
xrandr --output Virtual-1 --mode 1920x1080
```

Presiona "Ctrl + s" para grabar y luego "Super + w" para cerrar la ventana.

Si tienes dudas con las combinaciones de teclas, recuerda que toda la configuración está en **~/.config/sxhkd/sxhkdrc**

### Combinación de teclas importantes

Aquí te dejo algunas combinaciones de teclas que pueden ser de ayuda, recuerda que las puedes modificar y que puedes crear nuevas.

Para cambiar de area de trabajo, presiona

```
Super + 1
```

Donde "1" es el número de área de trabajo, en Polybar puedes ver que por default tienes 10, si te quieres mover al 10, presiona "Super + 0"

| Combinación     | Descripción                              |
| --------------- | ---------------------------------------- |
| Super + enter   | Abre la terminal                         |
| Super + espacio | Abre el menú (dmenu)                     |
| Super + ESC     | Carga la configuración de sxhkd de nuevo |
| Super + alt + q | Para **salir de BSPWM**                  |
| Super + alt + r | Para **reiniciar BSPWM**                 |

¿Dudas, preguntas o consejos? Puedes encontrarme en [ Twitter ](https://twitter.com/JorgeRS)
