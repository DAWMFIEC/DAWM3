..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

======================================================
Guía 22: Django - DRF + Firebase Admin Python SDK
======================================================

.. topic:: Objetivo específico
    :class: objetivo

    Implementar Django Rest Framework (DRF) para la administración de los endpoints de una API REST que sirva de pasarela de comunicación con la base de datos de Firebase Realtime Database. 

Actividades previas
=====================

Ambiente de desarrollo
----------------------

1. Acceda a su proyecto *django_api_suite* en Codespaces o en su máquina local.
2. Cree y utilice la(s) rama(s) de desarrollo.
3. Cree y habilite el ambiente virtual de desarrollo, con:

   .. code-block:: bash

       python -m venv env
       source env/bin/activate

4. Instale las librerías de requirements.txt, con:

   .. code-block:: bash

       pip install -r requirements.txt

Actividades en clases
=====================

Paquete: Firebase Admin Python SDK
----------------------------------

1. Instale la librería :term:`Firebase Admin Python SDK` en su ambiente de desarrollo:

   .. code-block:: bash
    
       pip install firebase-admin

2. Registre el Firebase Admin Python SDK en el archivo ``backend_data_server/settings.py`` del proyecto:

   .. code-block:: python
      :emphasize-lines: 3

      INSTALLED_APPS = [
        ...
        "firebase_admin",
        "rest_framework",
        ...
      ]

3. Utilice su cliente de IAG generativa para explicar qué es `Firebase Admin Python SDK <https://firebase.google.com/docs/admin/setup>`_ y cuáles son sus principales características.

Firebase Admin Python SDK
---------------------------

Credenciales
^^^^^^^^^^^^

1. En `Firebase Console <https://console.firebase.google.com/>`_, acceda a su proyecto **landing**.
2. Acceda a `Configuración de proyecto` > `Cuentas de servicio` > `SDK de Firebase Admin` para generar la clave privada. 
3. Acceda al servicio **Realtime Database** y copie la URL de referencia a la base de datos no relacional.

   .. note:: 
      
      La URL de referencia luce como `https://<PROJECT-ID>-default-rtdb.firebaseio.com/`

Secrets
^^^^^^^

4. En la raíz del repositorio, cree la carpeta ``secrets``
5. Agregue el archivo con la clave privada a la carpeta ``secrets`` y renombre el archivo como ``landing-key.json``.
6. Añada al archivo **.gitignore** la carpeta ``secrets``.

   .. code-block:: text
      :emphasize-lines: 4

      ...
      __marimo__/

      secrets/


Configuración en el proyecto
^^^^^^^^^^^^^^^^^^^^^^^^^^

7. Edite el archivo ``backend_data_server/settings.py``, con:

   .. code-block:: python
      :emphasize-lines: 4-5, 11-12, 14-17

      ...
      import os

      import firebase_admin
      from firebase_admin import credentials

      ...

      DEFAULT_AUTO_FIELD = ... 

      # Coloque la ruta relativa al archivo con la clave privada
      FIREBASE_CREDENTIALS_PATH = credentials.Certificate("secrets/landing-key.json")
      
      # Inicialice la conexión con el Realtime Database con la clave privada y la URL de referencia
      firebase_admin.initialize_app(FIREBASE_CREDENTIALS_PATH, {
         'databaseURL': 'https://<PROJECT-ID>-default-rtdb.firebaseio.com/'
      })

8. Utilice su cliente de IAG generativa para explicar cómo se configura el Firebase Admin SDK en un proyecto Django y cuáles son los pasos necesarios para establecer una conexión con Firebase Realtime Database.

Aplicación: Landing API
-----------------------

1. Cree una la aplicación **landing_api** en su proyecto.
2. Registre la aplicación en el archivo de configuración ``backend_data_server/settings.py`` del proyecto:
3. Registre la ruta \"landing/api/\" con las subrutas de la aplicación **landing_api**
4. Modifique el archivo ``landing_api/views.py`` con su cliente de IAG generativa, de acuerdo con:

   a) Importe los módulos **APIView**, **Response** y **status** de DRF, el módulo **db** de Firebase Admin SDK y el módulo **datetime** de Python,
   b) Cree la clase **LandingAPI** vista basada en clases,
   c) Dentro la clase, agregue el atributo **name** con el valor \"Landing API\" y el atributo **collection_name** con el nombre de la colección en Firebase Realtime Database que se utilizará para las operaciones CRUD,   

5. Cree el archivo ``landing_api/urls.py`` con la ruta \"index/\" a la vista **LandingAPI**.
6. Levante el servidor de desarrollo de Django.
7. Revise los cambios en el navegador con la URL raíz, seguida por la ruta `/landing/api/index/`

GET
---

.. note:: 
   
   Considere la documentación `Agrega el SDK de Firebase Admin a tu servidor <https://firebase.google.com/docs/admin/setup?hl=es-419>`_.

1. Edite el archivo ``landing_api/views.py``
2. Con su cliente de IAG generativa crear el código necesario para el método **get** que:

   a) Obtenga una referencia a la colección en Firebase Realtime Database,
   b) Utilice el método **get** de la referencia para obtener todos los elementos de la colección,
   c) Devuelva un arreglo JSON con los datos obtenidos y el código de estado HTTP 200 OK, para que el cliente pueda consumir la información de la colección.

   .. dropdown:: Ver el código 
      :color: primary  

      .. code-block:: python
         :emphasize-lines: 7-16

         ...
         
         class LandingAPI(APIView):
            
            ...

            def get(self, request):

               # Referencia a la colección
               ref = db.reference(f'{self.collection_name}')
               
               # get: Obtiene todos los elementos de la col ección
               data = ref.get()

               # Devuelve un arreglo JSON
               return Response(data, status=status.HTTP_200_OK)

3. Levante el servidor de desarrollo de Django.
4. Revise los cambios en el navegador en la URL en la ruta `/landing/api/index/`

POST
----

1. Edite el archivo ``landing_api/views.py``
2. Con su cliente de IAG generativa crear el código necesario para el método **post** que:

   a) Obtenga los datos del cuerpo de la solicitud,
   b) Obtenga una referencia a la colección en Firebase Realtime Database,
   c) Obtener la fecha y hora actual en el servidor y formatearla con el siguiente formato personalizado: "dd/mm/yyyy, hh:mm:ss a. m./p. m." en minúsculas y con la notación española (a. m. y p. m.).
   d) Añadir esa fecha formateada al objeto recibido en la solicitud bajo el campo "timestamp"
   e) Utilice el método **push** de la referencia para guardar el objeto en la colección,
   f) Devuelva el ID del objeto guardado y el código de estado HTTP 201 Created, para que el cliente pueda confirmar que el objeto fue creado exitosamente.

   .. dropdown:: Ver el código 
      :color: primary  

      .. code-block:: python
         :emphasize-lines: 7-22

         ...

         class LandingAPI(APIView):
            
            ...

            def post(self, request):

               data = request.data
	        
               # Referencia a la colección
               ref = db.reference(f'{self.collection_name}')

               current_time  = datetime.now()
               custom_format = current_time.strftime("%d/%m/%Y, %I:%M:%S %p").lower().replace('am', 'a. m.').replace('pm', 'p. m.')
               data.update({"timestamp": custom_format })
               
               # push: Guarda el objeto en la colección
               new_resource = ref.push(data)
               
               # Devuelve el id del objeto guardado
               return Response({"id": new_resource.key}, status=status.HTTP_201_CREATED)

3. Levante el servidor de desarrollo de Django.
4. Revise los cambios en el navegador en la URL en la ruta `/landing/api/index/`

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

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *django_api_suite*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Cómo te ayudó la inteligencia artificial generativa a comprender la función del Firebase Admin SDK en la administración de servicios como la autenticación, la base de datos y el almacenamiento desde el backend en Python?

    * ¿Cómo garantizaste que las operaciones realizadas desde Firebase Admin SDK, como la lectura o escritura de datos, se ejecutaran de forma segura y eficiente en tu proyecto backend?

    * ¿Cómo equilibraste el uso del código generado por la IA con tu responsabilidad como desarrollador para asegurar que comprendes y controlas el flujo de datos y la configuración del backend?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">When you&#39;re building an app, you&#39;ll often need a way to send and get back data between your app &amp; a server.<br><br>And using a REST API is a great way to do this.<br><br>In this tutorial, <a href="https://twitter.com/_udemezue?ref_src=twsrc%5Etfw">@_udemezue</a> teaches you how to build a REST API in Django using the Django Rest Framework.… <a href="https://t.co/aPeAxcpTAM">pic.twitter.com/aPeAxcpTAM</a></p>&mdash; freeCodeCamp.org (@freeCodeCamp) <a href="https://twitter.com/freeCodeCamp/status/1945454273652785446?ref_src=twsrc%5Etfw">July 16, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>