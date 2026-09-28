..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

=======================================
Guía 26: Django - Despliegue en Railway
=======================================

.. topic:: Objetivo específico
    :class: objetivo

    Realizar el despliegue de un proyecto Django en la plataforma Railway para la publicación de un servicio web accesible desde cualquier cliente y garantizar la comunicación estable y segura con los datos. 

Actividades previas
=====================

Ambiente de despliegue
----------------------

1. **Clone su proyecto en su máquina local**.
2. Cree y utilice la(s) rama(s) de despliegue: **deploy**.
3. Cree y habilite el ambiente virtual, con:

   .. code-block:: bash

       python -m venv env
       
       env\Scripts\activate # Windows
       source env/bin/activate # Linux/MacOS

4. Instale las librerías de requirements.txt, con:

   .. code-block:: bash

       pip install -r requirements.txt

Actividades en clases
=====================

Paquete: gunicorn y whitenoise
------------------------------

1. Instale `gunicorn` y `whitenoise` en su ambiente, con:

   .. code-block:: bash
    
       pip install gunicorn whitenoise

2. Utilice su cliente de IAG generativa para explicar la utilidad de los paquetes gunicorn y whitenoise.

Configuración de Django para producción
---------------------------------------

1. En el archivo `backend_analytics_server/settings.py`, configure los siguientes parámetros:

   a) **DEBUG**: Cambie a `False`.
   b) **ALLOWED_HOSTS**: Agregue el dominio de Railway.
   c) **CSRF_TRUSTED_ORIGINS**: Agregue el dominio de Railway.
   d) **MIDDLEWARE**: Agregue `WhiteNoiseMiddleware` para servir archivos estáticos.
   e) **STATIC_ROOT**: Configure la ruta para los archivos estáticos, por ejemplo:

   .. code-block:: python

       DEBUG = False
       
       ALLOWED_HOSTS = ['.up.railway.app']

       CSRF_TRUSTED_ORIGINS = ["https://*.up.railway.app"]
       
       MIDDLEWARE = [
         ...
         'whitenoise.middleware.WhiteNoiseMiddleware',
         ...
       ]

       STATIC_ROOT = "assets/"

2. Utilice su cliente de IAG generativa para explicar la utilidad de cada una de las configuraciones realizadas.


Gestión de dependencias
-----------------------

1. Genere el archivo `requirements.txt` con la lista de paquetes utilizados, con:

   .. code-block:: bash

       pip freeze > requirements.txt

2. Desactive el ambiente virtual de desarrollo, con:

   .. code-block:: bash

       deactivate

Versionamiento
--------------

1. Versione local y remotamente la rama **deploy**.


Railway
-------

1. Obtenga una cuenta gratuita en `Railway <https://railway.app/>`_ mediante su cuenta de GitHub.
2. Utilice su cliente de IAG generativa para explicar la utilidad de Railway.

Configuración en Railway
------------------------

1. En Railway, acceda a la opción **New**.
2. Seleccione **Deploy from GitHub** y conecte su cuenta de GitHub.
3. Seleccione el repositorio *django_data_monitor* y la rama *deploy*.
4. Configure el entorno de producción:

   a) En **Environment Variables**, agregue las variables (`DJANGO_SUPERUSER_EMAIL`, `DJANGO_SUPERUSER_PASSWORD` y `DJANGO_SUPERUSER_USERNAME`) para crear el superusuario.
   b) En **Build** > **Custom Build Command**, utilice:

   .. code-block:: bash

       pip install -r requirements.txt

   c) En **Deploy** > **Pre-deploy Command**, utilice:

   .. code-block:: bash

       python manage.py makemigrations && python manage.py migrate && python manage.py collectstatic && python manage.py createsuperuser --noinput
   
   d) En **Deploy** > **Custom Start Command**, utilice:

   .. code-block:: bash

       gunicorn backend_analytics_server.wsgi

5. En **Networking**, escoja la opción del dominio personalizado en el puerto 80.


Servicio: MySQL Database
------------------------

1. En Railway, acceda a la opción **New**.
2. Seleccione **Database** y luego **MySQL**.
3. Configure la base de datos con un nombre y otras opciones según sea necesario.
4. Obtenga la URL de conexión a la base de datos y guárdela para su uso en el proyecto Django.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué elementos clave del proceso de despliegue en Railway comprendiste mejor gracias a la inteligencia artificial generativa, y qué conceptos tuviste que reforzar por tu cuenta para asegurar una implementación funcional?

    * ¿Cómo verificaste el funcionamiento correcto del backend desplegado en Railway, y qué hiciste para resolver problemas como errores de conexión a la base de datos o fallas en el entorno de producción?

    * ¿Qué actitudes asumiste para garantizar que el uso de inteligencia artificial en el proceso de despliegue no reemplazara tu comprensión del entorno de producción, sino que fortaleciera tu capacidad como desarrollador responsable?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Drop the .app, it&#39;s cleaner that way <br><br>Introducing an all-new Railway (dot com)<a href="https://t.co/C5PSPyo5IO">https://t.co/C5PSPyo5IO</a></p>&mdash; Railway (@Railway) <a href="https://twitter.com/Railway/status/1857148311494623725?ref_src=twsrc%5Etfw">November 14, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>