..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

====================================
Guía 07: Responsividad y Componentes 
====================================

.. topic:: Objetivo específico
    :class: objetivo

    Aplicar principios de diseño responsivo y reutilización de componentes utilizando TailwindCSS para estructurar una landing page de una sola página (SPA) que se adapte correctamente a diferentes dispositivos y mantenga coherencia visual y funcional en toda la interfaz.
    

Actividades previas
=====================

Diseño
------

1. Organice el contenido de su landing page en secciones, p.e.: encabezado, presentación, servicios, testimonios, contacto, etc.
2. Diseñe cada una de las secciones de la landing page. Primero, considere la versión para la resolución de un dispositivo móvil (640px). Luego para la resolución de una computadora (1280px), p.e.: 

Ambiente de desarrollo
----------------------

1. Acceda a su proyecto *landing* en Codespaces o en su máquina local.
2. Cree y utilice la(s) rama(s) de desarrollo.
3. Instale los paquetes y levante el servidor, con:

   .. code-block:: bash

      npm install
      npm run dev

Actividades en clases
=====================

HTML
----

Viewport
^^^^^^^^

1. Identifique si su archivo *index.html* contiene la etiqueta `<meta>`:
   
   .. code-block:: html
      :linenos:
      :emphasize-lines: 3

      <head>
        ...
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        ...
      </head>

2. Utilice un cliente de IAG para justificar la importancia de la etiqueta `<meta>`.

Secciones
^^^^^^^^^

1. Agregue a su archivo *index.html*:

   .. code-block:: html
       :caption: Sección con imágenes.
       :emphasize-lines: 1-10
       :linenos:
        
        <section class="bg-white dark:bg-gray-900">
            <div id="container-03" class="px-4 py-8 mx-auto">
                <div id="container-04">
                    <img src="https://placehold.co/50" alt="Imagen 1" class="w-full h-auto rounded">
                    <img src="https://placehold.co/50" alt="Imagen 2" class="w-full h-auto rounded">
                    <img src="https://placehold.co/50" alt="Imagen 3" class="w-full h-auto rounded">
                    <img src="https://placehold.co/50" alt="Imagen 4" class="w-full h-auto rounded">
                </div>
            </div>
        </section>

2. Compruebe el resultado en el navegador. 


TailwindCSS
-----------

Diseño Responsivo
^^^^^^^^^^^^^^^^^

.. sidebar:: 

   .. image:: https://blog.yunusemre.dev/_astro/mobile-first.bafe34e3_22KpYS.webp

   Punto de quiebre o breakpoints para diferentes tamaños de dispositivos que considera TailwindCSS.

1. Identifica los :term:`breakpoints` con el `simulador del modo de dispositivo <https://developer.chrome.com/docs/devtools/device-mode?hl=es-419>`_ en el inspector del navegador.

2. Utilice la documentación de `TailwindCSS - Responsive Design <https://tailwindcss.com/docs/responsive-design>`_ y `TailwindCSS - max-width <https://tailwindcss.com/docs/max-width>`_.

3. Modifique el archivo *index.html*, verifique el efecto en los breakpoints recomendados con el inspector y analice el uso de las clases con su cliente AIG, de:

   a) Modifique la etiqueta `<div id=\"container-03\">` agregándole la clase "max-w-md". Revise la diferencia de los efectos para los breakpoints `sm`, `md`, `lg`, `xl` y `2xl`.
   b) Agregue la clase "md:max-w-xl" a la etiqueta `<div id=\"container-03\">` . Revise las diferencias para cada breakpoint.
   c) Modifique la etiqueta `<div id=\"container-04\">` agregándole las clases "grid grid-cols-2 gap-4". Revise la diferencia de los efectos para los breakpoints `sm`, `md`, `lg`, `xl` y `2xl`.
   d) Agregue la clase "md:grid-cols-4" a la etiqueta `<div id=\"container-04\">` . Revise las diferencias para cada breakpoint.

4. Compruebe el resultado en el navegador. 

Flowbite
--------

Componentes
^^^^^^^^^^^

1. Utilice la documentación de `Flowbite <https://flowbite.com/>`_

2. `Incluye Flowbite con el CDN <https://flowbite.com/docs/getting-started/quickstart/#include-using-cdn>`_ en el archivo *index.html*.

3. Modifique el archivo *index.html*, verifique el efecto en los breakpoints recomendados con el inspector y analice el uso de las clases con su cliente AIG

   a) Agregue el :term:`componente` `Default Navbar <https://flowbite.com/docs/components/navbar/#default-navbar>`_.
   b) Modifique el componente y compruebe el comportamiento para cada tamaño de dispositivo.

4. Compruebe el resultado en el navegador. 

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *landing*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Vercel
------

1. Verifique el :term:`despliegue continuo (CD)` del proyecto en Vercel.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué diferencias identificas entre aplicar clases responsivas manualmente y utilizar sugerencias generadas por IA?

    * ¿Cómo validaste la funcionalidad y coherencia visual de las clases responsivas aplicadas en tu proyecto?

    * ¿Cómo influye el uso de inteligencia artificial en la forma en que asumes tu responsabilidad como diseñador web?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    Diseño responsivo

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Happy 11th Birthday Responsive Web Design! On May 25, 2010, web designer Ethan Marcotte published an article entitled &quot;Responsive Web Design&quot; in the online magazine A List Apart. <a href="https://t.co/vjK4affT5b">https://t.co/vjK4affT5b</a><a href="https://twitter.com/hashtag/WebDesignHistory?src=hash&amp;ref_src=twsrc%5Etfw">#WebDesignHistory</a> <a href="https://t.co/2Crd5GZ4qC">pic.twitter.com/2Crd5GZ4qC</a></p>&mdash; Web Design Museum (@WebDesignMuseum) <a href="https://twitter.com/WebDesignMuseum/status/1397228466693681163?ref_src=twsrc%5Etfw">May 25, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>