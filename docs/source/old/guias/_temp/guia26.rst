..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

===================================================
Guía 26: React y Ionic - Introducción y Componentes
===================================================

.. topic:: Objetivo específico
    :class: objetivo

    Introducir el entorno de desarrollo de aplicaciones híbridas con Ionic y React mediante la creación de interfaces responsivas y reutilizables utilizando Ionic Components, con el fin de comprender la estructura del framework y aplicar buenas prácticas en el diseño visual y funcional de la aplicación. 

Actividades previas
=====================

Ambiente de desarrollo
----------------------

1. Cree un repositorio en GitHub con el nombre *hibrida*.

   a) Agregue un archivo README.md con el título de su aplicación híbrida y una breve descripción del objetivo de su proyecto.
   b) Agregue un archivo *.gitignore* con la plantilla de *Node*.

2. Acceda a su proyecto *hibrida* en Codespaces o en su máquina local.
3. Cree y utilice la(s) rama(s) de desarrollo.

Actividades en clases
=====================

Ionic: Inicialización del proyecto
----------------------------------

1. Explore la documentación de `Ionic <https://ionicframework.com/docs/>`_ para comprender los conceptos básicos de esta biblioteca.
2. Instale Ionic y sus dependencias con el siguiente comando:

    .. code-block:: bash
    
        npm install -g @ionic/cli

3. Crea una aplicación Ionic, de acuerdo con:

   a) Comience la configuración, utilizando el comando:

   .. code-block:: bash

      ionic start .

   b) **No** utilice el asistente de creación de proyectos. 

   .. code-block:: bash

       ? Use the app creation wizard? No

   c) Seleccione *React* como framework y elija un nombre para su proyecto, por ejemplo, *hibrida*.

   .. code-block:: bash

       ? Framework to use: React
       ? Project name: hibrida

   d) Seleccione la plantilla **tabs**.

    .. code-block:: 

        ? Starter template: 
        ...
        ❯ tabs         | A starting project with a simple tabbed interface 

3. 
   .. code-block:: bash

      npm install
      npm run dev

Componentes de Ionic
---------------------

1. Explore los componentes de Ionic disponibles en la documentación oficial y familiarícese con su uso.
2. Implemente al menos tres componentes de Ionic en su aplicación, asegurándose de que sean funcionales y estén bien integrados en la interfaz de usuario.
3. Aplique estilos personalizados a los componentes utilizando las herramientas de Ionic para mejorar la apariencia visual de la aplicación.
4. Compruebe la vista previa del resultado en el navegador.


Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *hibrida*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué?

    * ¿Cómo?

    * ¿Qué?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">This is why I love <a href="https://twitter.com/Ionicframework?ref_src=twsrc%5Etfw">@Ionicframework</a>. We can build essentially any UI, sometimes even like this iOS Twitter settings screen with UI Components out of the box. 👨🏼‍🔧<br><br>Everything you see here is from Ionic, <a href="https://twitter.com/ionicons?ref_src=twsrc%5Etfw">@ionicons</a> and styled using Ionic&#39;s theme application colors. <a href="https://t.co/ZocsDvBShH">pic.twitter.com/ZocsDvBShH</a></p>&mdash; Alan Montgomery (@93alan) <a href="https://twitter.com/93alan/status/1512587338962116611?ref_src=twsrc%5Etfw">April 9, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>