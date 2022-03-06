---
title: "Que Es AppImage Y Como Usarla"
date: 2022-03-06T14:02:29-06:00
description: AppImage provee una manera fácil y sencilla de compartir archivos "binarios" en Linux.
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- linux
- escritorio
series:
-
categories:
- Escritorio
image: images/posts/appimage/appimage-logo.png
---

Por ejemplo en Windows puedes compartir archivos ".exe" que se pueden ejecutar en cualquier otra computadora, utilizando Windows por supuesto. Pues bueno, en Linux ahora existe este formato llamado AppImage que permite compartir applicaciones en un solo archivo.

Una de las venatajas es que no tienes que instalar nada utilizando "apt", "pacman" o "yum". Todo lo necesario para ejecutar la applicación ya viene incluido en el archivo ".AppImage".

Para darte un ejemplo, en este momento estoy escribiendo este artículo utilizando [Mark Text](https://marktext.app/) el cual lo descargas en AppImage.

## ¿Cómo ejecutar un AppImage?

### Utilizando la Terminal

La manera más sencilla de ejecutar un AppImage es a través de la terminal:

1. Abres la terminal

2. Te cambias al directorio donde guardaste el archivo .AppImage

3. Cambias los permisos de ejecución con el siguiente comando:
   
   ```bash
   chmod +x nombre_del_archivo.AppImage
   ```

4. Ejecutas el archivo con:
   
   ```bash
   ./nombre_del_archivo.AppImage
   ```

### Utilizando el Administrador de Archivos

La otra manera es utilizando el manejador de archivos.

1. Abre el manejador de archivos

2. Navega al directorio donde descargaste el archivo

3. Da clic derecho en el archivo y selecciona propiedades
   ![imagen aqui](/images/posts/appimage/appimage-cambiar-propiedades.png)

4. Selecciona la pestaña de "permisos"

5. Da clic en "Permitir ejecuar el archivo como un programa"
   ![](/images/posts/appimage/appimage-permitir-ehjecutar-archivo.png)

6. Da doble clic al archivo para ejecutarlo.

## ¿Cómo agregar un AppImage al menú de mi escritorio favorito?

La manera más sencilla que yo encontré es utlizando una herramienta que se llama "[appimaged](https://github.com/probonopd/go-appimage/blob/master/src/appimaged/README.md)". 

[appimaged](https://github.com/probonopd/go-appimage/blob/master/src/appimaged/README.md) es un "demonio" que monitorea el siguiente conjunto de directorios en tu computadora buscando archivos "AppImage":

- /usr/local/bin
- /opt
- ~/Applications
- ~/.local/bin
- $PATH, que frecuentemente incluye /bin, /sbin, /usr/bin, /usr/sbin, /usr/local/bin, /usr/local/sbin, y otros más.

### Como instalar appimaged

El Primer paso es crear una carpeta o directorio que se llame "Applications" debajo de nuestro directorio personal, después descargamos el archivo, le cambiamos los permisos de ejecución

```bash
mkdir -p ~/Applications
wget -c https://github.com/$(wget -q https://github.com/probonopd/go-appimage/releases -O - | grep "appimaged-.*-x86_64.AppImage" | head -n 1 | cut -d '"' -f 2) -P ~/Applications/
chmod +x ~/Applications/appimaged-*.AppImage
```

y por úlitmo lo ejecutamos:

```bash
~/Applications/appimaged-*.AppImage
```

### Como DESINSTALAR appimaged

Si por alguna razón deseas o necesitas **desinstalar** "appimaged" entonces ejecuta lo siguiente en la terminal:

```bash
systemctl --user disable appimaged.service || true
systemctl --user stop appimaged.service || true
rm ~/.config/systemd/user/appimaged.service
rm ~/.local/share/applications/appimagekit*.desktop
rm ~/Applications/appimaged-*-x86_64.AppImage
```

Bueno, pues ya lo sabes, cada que descargues una AppImage guárdala en la carpeta de "Applications", una vez ahí "appimaged" encontrará el nuevo archivo y lo mostrará en el menú de tu escritorio.

Si te ha gustado y te ha servido este artículo no dudes en compartirlo con tus amigos en las redes sociales utilizando los botones al final de este.

Suerte y hasta la próxima.
