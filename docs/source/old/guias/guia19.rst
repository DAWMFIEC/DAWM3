..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================================================
Guía 19: Appflow - Generación del apk para Android 
==================================================

.. topic:: Objetivo específico
    :class: objetivo

    Desplegar una aplicación híbrida en un dispositivo Android utilizando Ionic Appflow.

Actividades previas
=====================

Ambiente de despliegue
----------------------

1. Obtenga una cuenta en `Ionic Appflow <https://ionic.io/appflow>`_ y verifique su cuenta a través del correo electrónico.
2. Inicie su sesión en `Ionic Appflow <https://ionic.io/appflow>`_.

Actividades en clases
=====================

Plataforma Android
------------------

1. En el proyecto, genere los archivos nativos del proyecto Android dentro de una carpeta `android`, en el directorio raíz de tu proyecto.

   .. code-block:: bash

       ionic cap add android

   **Nota:** Después de la primera adición, o cada vez que actualice el código de su aplicación, debes copiar los recursos web más recientes al proyecto nativo de Android. El comando `sync` se encarga de esto y también instala cualquier complemento nativo nuevo que hayas añadido.

2. Utilice un cliente de IAG para explicar las características principales de Capacitor como herramienta de empaquetado para aplicaciones híbridas, enfocándose en la plataforma Android.


Despliegue en Ionic Appflow
---------------------------

1. Conecte la aplicación híbrida en el repositorio *mobile* con la aplicación creada en Ionic Appflow.

   a) Siga las instrucciones en `Connect Using GitHub <https://ionic.io/docs/appflow/quickstart/github>`_ para conectar su repositorio GitHub con Ionic Appflow.
   b) Seleccione el repositorio *mobile* y la rama principal (main) para conectar con Ionic Appflow.

2. Realice un compilación nativa de la aplicación híbrida en Ionic Appflow, de acuerdo con las instrucciones en `Start a Native Build <https://ionic.io/docs/appflow/quickstart/package#start-a-native-build>`_ para realizar la compilación nativa.
3. Luego de completar exitosamente el despliegue manual, descargue o utilice el QR code proporcionado por Ionic Appflow para instalar la aplicación híbrida en un dispositivo móvil compatible.


Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Cómo te ayudó la inteligencia artificial generativa a comprender el proceso de despliegue con Appflow?

    * ¿Cómo aseguraste que el proceso de despliegue continuo fuera fluido y coherente?

    * ¿Qué actitudes consideras fundamentales para asegurar que el resultado final de la aplicación híbrida sea auténtica y producto de tu criterio profesional, incluso si partiste de una base generada por IA?


Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Introducing the Appflow CLI 🪄<br><br>This standalone CLI allows you to manage all aspect of your app&#39;s CI/CD lifecycle, whether you&#39;re using Appflow or integrating with another CI/CD platform.<br><br>Get all the details in our latest blog post 👇<a href="https://t.co/exhlIcrE2w">https://t.co/exhlIcrE2w</a></p>&mdash; Appflow (@useappflow) <a href="https://twitter.com/useappflow/status/1783581330011214221?ref_src=twsrc%5Etfw">April 25, 2024</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>