---
title: "Como Configurar Neovim Como VsCode"
date: 2020-12-06T20:55:47-06:00
description: El día de hoy vamos a instalar y a configurar Neovim para utilizarlo como IDE para programar en Python. Vamos a utilizar algunos "Plugins" para obtener las ventajas que cualquier otro IDE tiene, tales como validación de sintaxis y autocompletado.
draft: true
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
  - vim
  - neovim
  - python
  - ide
series:
  -
categories:
  - neovim
image: /images/posts/neovim-python/neovim-logo.png
---

## Introducción

<a href="https://neovim.io/" target="_blank">Neovim</a> es VIM pero mejorado. La documentación oficial lo define como:

> Neovim es un editor de texto basado en Vim diseñado para la extensibilidad y usabilidad, para fomentar nuevas aplicaciones y contribuciones.

## Instalación

### Prerequisitos

Como lo dice el título de este artículo, me estaré concentrando en configurar Neovim como IDE para python, entonces voy a asumir que ya tienes Python y Pip instalados en tu sistema.

#### Python, Pip y Pipenv

Para verificar que tengas Python instalado corre uno de los siguientes comandos en la consola:

```bash
$ python --version
$ python3 --version
```

El comando anterior deberá decirte la versión de Python instalada en tu sistema

Para verificar si Pip está instalado, solo ejecuta pip en la consola. El comando deberá regresar la "ayuda".

```bash
$ pip

Usage:
  pip <command> [options]

Commands:
  install                     Install packages.
  download                    Download packages.
  uninstall                   Uninstall packages.
  freeze                      Output installed packages in requirements format.
  list                        List installed packages.
  show                        Show information about installed packages.
  check                       Verify installed packages have compatible dependencies.
  config                      Manage local and global configuration.
  search                      Search PyPI for packages.
  cache                       Inspect and manage pip's wheel cache.
  wheel                       Build wheels from your requirements.
  hash                        Compute hashes of package archives.
  completion                  A helper command used for command completion.
  debug                       Show information useful for debugging.
  help                        Show help for commands.
```

#### Neovim

Lo primero que tenemos que hacer es instalar neovim y pyhton3-neovim

```bash
$ sudo apt-get install neovim python3-neovim
```

#### Node.js

#### Nerdfont (Tipo de Letra)

## Configuración

### Directorio nvim

### Archivos .vim

#### init.vim

#### general/settings.vim

### Plugins

#### Conquer of Completion (coc)

**Extensiones:**

1. Extensión para Python
2. Extensión para Snippets

#### Airline

#### Surround

#### Autopairs

#### Nerdtree

#### NerdCommenter

## Conclusión
