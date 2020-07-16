---
title: "Como Instalar Python en Windows 10 en 3 Sencillos Pasos."
date: 2020-07-06T18:17:40-05:00
description: "En tan solo 3 sencillos pasos, aprende a instalar python 3 en Windows 10."
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- python
- windows
series:
-
categories:
- Python
- Tutorial
image: /images/posts/python-logo.png
---

## Paso 1: Descargar Python

El primer paso es descargar descargar python del sitio oficial: <https://www.python.org/>

Solo secciona la opción de "Download" en la página principal, el stitio por defecto debe detectar que estás utlizando Windows y te ofrecerá el instalador que requieres.

![Descarga Python](/images/posts/descarga-python-windows.jpg "Descarga Python del sitio oficial")

## Paso 2: Ejecuta la instalación

El siguiente paso es obvio, ejecuta la instalación.

Es importante que durante la instalación selecciones que deseas instalar PIP y presiona "Next"

![Selecciona pip](/images/posts/selecciona-pip.jpg "Selecciona pip")

En la siguiente pantalla deja los parametros por default y presiona "Install".

A mi en lo personal me gusta que la instalación de Python esté en C:\Python\<<version\>> donde la versión en este caso es la 3.8.

![Presiona Install](/images/posts/presiona-install-python.jpg)

## Paso 3: Verifica la instalación

Para verificar que todo haya ido conforme a lo planeado y que tienes Python instalado de manera correcta solo abre una ventana de comando y escribe 

```cmd
python --version
```

![Version de Python](/images/posts/verifica-instalacion-python.jpg)

Por último verifica que se haya instalado correctamente PIP.

```cmd
pip
```

![pip instalado](/images/posts/verifica-instalacion-pip.jpg)

pip tiene que regresar la ayuda, algo parecido a la imágen anterior. Si recibes un mensaje de error diciendo que no se encuentra instalado, solo ejecuta la instalación de nuevo y selecciona "Reparar"

Eso es todo por hoy, espero que no hayas tenido problemas al instalar python.

Siguente tutorial será crear tu primer programa en #Python
