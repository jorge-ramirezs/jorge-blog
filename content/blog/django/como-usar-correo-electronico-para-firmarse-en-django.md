---
title: "Como Usar Correo Electrónico En Vez Nombre De Usuario Para Firmarse en Django"
date: 2022-10-29T15:31:06-05:00
description: Django por defecto tiene uno de los mejores sistemas de autenticación que conosco, la única desventaja es que no tiene la opción de utilizar la cuenta de correo electrónico para firmarse en vez de un usuario y contraseña, en este artículo te voy a enseñar la solución que yo utilizo y que me ha servido hasta hoy.
draft: false
hideToc: false
enableToc: true
enableTocContent: false
tocPosition: inner
tocLevels: ["h2", "h3", "h4"]
tags:
- django
- python
- desarrollo
series:
-
categories:
- Django
image: /images/posts/django-email-login/django-python.png
---
<!-- Content -->
# YouTube Video:

{{< youtube 111111111 >}}

# Instrucciones:

## 1. Instala Django

Obviamente lo primero que vamos a hacer es instalar Django, al día de hoy que estoy escribiendo este artículo la versión es 4.1.2.

Vamos entonces a crear un directorio, después creamos un ambiente virtual utilizando **pipenv**, instalamos django y creamos el proyecto.

```bash
$ mkdir my_project && cd my_project
$ pipenv install django~=4.1.2
$ pipenv shell
(my_project)$ django-admin startproject auth .
(my_project)$ python manage.py runserver
```

**ATENCIÓN:**

> Es muy importante que no ejecutes los comandos "makemigrations" o "migrate" antes de completar todos los pasos descritos en este artículo.


Si todo sale como lo planeamos, lo siguiente es dar clic en la liga que aparece en la terminal o también puedes abrir un navegador e ir a [http://127.0.0.1:8000/](http://127.0.0.1:8000/) en donde debe aparecer la página de bienvenida de Django.

![Pagina de Bienvenida de Django](/images/posts/django-email-login/django-bienvenida.png)

Para detener el servidor presiona `Ctrl + c` en la terminal.

## 2. Crea La App Users

Django viene por defecto con un modelo de usuario que es el que utiliza para la autenticación.
Nosotros vamos a modificar este modelo creando uno propio que nos permita utilizar la cuenta de **Correo Electronico** en vez de un **Nombre de Usuario**.

Ahora es momento de crear una app en la cual vamos a modificar el modelo de usuario que viene por defecto con Django.

```bash
(my_project)$ python manage.py startapp users
```

Una vez creada la app, no te olvides de agregarla en INSTALLED_APPS en el archivo *settings.py*

```settings.py
INSTALLED_APPS = [
    ...
    # other apps
    'users',
]
```

## 3. Modifica El Archivo users/models.py

A continuación agrega el siguiente codigo al archivo *users/model.py*. Esto es necesario para hacer "email" el campo por defecto en vez de "username".

```users/models.py
from django.db import models
from django.contrib.auth.models import AbstractBaseUser, PermissionsMixin, BaseUserManager


class CustomUserManager(BaseUserManager):
    def _create_user(self, email, password, first_name, last_name, is_email_verified, **extra_fields):
        if not email:
            raise ValueError("Email must be provided")
        if not password:
            raise ValueError("Password must be provided")

        user = self.model(
            email=self.normalize_email(email),
            first_name=first_name,
            last_name=last_name,
            is_email_verified=is_email_verified,
            **extra_fields
        )

        user.set_password(password)
        user.save(using=self.db)
        return user

    def create_user(self, email, password, first_name, last_name, is_email_verified, **extra_fields):
        extra_fields.setdefault("is_staff", False)
        extra_fields.setdefault("is_superuser", False)
        return self._create_user(email, password, first_name, last_name, is_email_verified, **extra_fields)

    def create_superuser(self, email, password, first_name, last_name, is_email_verified, **extra_fields):
        extra_fields.setdefault("is_staff", True)
        extra_fields.setdefault("is_superuser", True)

        if extra_fields.get("is_staff") is not True:
            raise ValueError("Superuser must have is_staff=True.")
        if extra_fields.get("is_superuser") is not True:
            raise ValueError("Superuser must have is_superuser=True.")

        return self._create_user(email, password, first_name, last_name, is_email_verified, **extra_fields)


class User(AbstractBaseUser, PermissionsMixin):
    email = models.EmailField(db_index=True, unique=True, max_length=254)
    first_name = models.CharField(max_length=240)
    last_name = models.CharField(max_length=255)
    is_email_verified = models.BooleanField(default=False)

    is_staff = models.BooleanField(default=True)  # Required
    is_active = models.BooleanField(default=True)
    is_superuser = models.BooleanField(default=False)

    objects = CustomUserManager()

    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['first_name', 'last_name', 'is_email_verified']

    class Meta:
        verbose_name = 'User'
        verbose_name_plural = 'Users'

```

## 4. Modifica Settings.py

Ahora es momento de decirle a Django que utilice nuestro modelo como modelo para la autenticación de los usuarios. Agrega el siguiente codigo al archivo *settings.py*

```settings.py
AUTH_USER_MODEL = 'users.User'
```

## 5. Agrega el nuevo modelo a la consola de administración

Como modificamos el modelo del usuario que viene por defecto en Django, ahora lo que debemos hacer es agregar el nuevo modelo a la consola de administración, para eso debemos agregar lo siguiente al archivo *"users/admin.py"*

```users/admin.py
from django.contrib import admin

from .models import User


class CustomUserAdmin(admin.ModelAdmin):
    list_display = (
        'email',
        # 'first_name',
        # 'last_name',
        'is_email_verified',
        'is_staff',
        'is_active',
    )
    list_filter = ('email', 'is_staff', 'is_email_verified',)
    fieldsets = (
        (None, {'fields': (
            'first_name',
            'last_name',
            'password',
            'groups'
        )}),
        ('Permissions', {'fields': (
            'user_permissions',
            'is_email_verified',
            'is_staff',
            'is_active',
            'is_superuser'
        )}),
    )
    search_fields = ('email',)
    ordering = ('email',)


admin.site.register(User, CustomUserAdmin)
```


## 6. Crea Las Tablas

A continuación actualiza las tablas en la base de datos corriendo los comandos *makemigrations* y *migrate* de la siguiente manera:

```bash
(my_project)$ python manage.py makemigrations
(my_project)$ python manage.py migrate 
```

## 7. Crea El Superusuario

Luego vamos a crear el superusuario con el siguiente comando:

```bash
(my_project)$ python manage.py createsuperuser
Email: jorge@linuxfacil.mx
First name: Jorge
Last name: Ramirez
Is email verified: True
Password: 
Password (again): 
Superuser created successfully.
```

## 8. Prueba La Configuración

Por último vamos a correr el servidor y entrar a la consola de administración.
Ejecuta el servidor usando

```bash
(my_project)$ python manage.py runserver
```

Y en tu navegador abre la siguiente dirección: [http://127.0.0.1:8000/admin/](http://127.0.0.1:8000/admin/)

Ingresa el correo electrónico y el password del superusuario

Y listo.

