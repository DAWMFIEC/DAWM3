..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

====================
Guía 06: TailwindCSS
====================

.. topic:: Objetivo específico
    :class: objetivo

    Evaluar el uso de clases utilitarias de TailwindCSS en la creación de una landing page funcional y atractiva considerando su adecuación a los requerimientos específicos del diseño.

Actividades previas
=====================

Diseño
------

1. Defina el objetivo de su :term:`landing page`, p.e.: venta de productos de belleza, información de un grupo estudiantil, datos de una veterinaria, noticias de KPOP, etc.
2. Identifique las secciones de su landing page, p.e.: encabezado, presentación, servicios, testimonios, contacto, etc.

Ambiente de desarrollo
----------------------

1. Cree un repositorio en GitHub con el nombre *landing*.

   a) Agregue un archivo README.md con el título de su landing page y una breve descripción del objetivo de su proyecto.
   b) Agregue un archivo :term:`.gitignore` con la plantilla de :term:`Node`.
   
2. Acceda a su proyecto *landing* en Codespaces o en su máquina local.
3. Cree y utilice la(s) rama(s) de desarrollo.

Actividades en clases
=====================

Vite
----

.. sidebar:: 

   .. image:: https://pbs.twimg.com/media/EtZYf1FWYAMmtHj.jpg

   Más información en el sitio de `Vite <https://vite.dev/>`_.

1. Utilice la terminal para crear un proyecto :term:`Vite` mediante :term:`npm`:

   .. code-block:: bash

      npm create vite@latest .

   a) Para `Ok to proceed? (y)` ingrese `y`.
   b) Para `Current directory is not empty. Please choose how to proceed:` seleccione la opción `Ignore files and continue`.
   c) Seleccione el framework como `Vanilla`.
   d) Seleccione la variante como `JavaScript`.

2. Instale las dependencias del proyecto con el comando:

   .. code-block:: bash

      npm install

3. Inicie el servidor de desarrollo con el comando:

   .. code-block:: bash

      npm run dev

4. Abra su navegador y acceda a la dirección `http://localhost:5173/` para ver su proyecto Vite en funcionamiento.

TailwindCSS
-----------

.. sidebar::
   
   .. image:: https://upload.wikimedia.org/wikipedia/commons/thumb/9/95/Tailwind_CSS_logo.svg/2560px-Tailwind_CSS_logo.svg.png

   Documentación disponible en `TailwindCSS <https://tailwindcss.com/>`_.


Play CDN
^^^^^^^^

1. Agregue la referencia a :term:`TailwindCSS` con la etiqueta de script Play :term:`CDN` al `<head>` de su archivo HTML.

   .. code-block:: html
      :caption: Agregue la etiqueta script con la referencia al archivo js en el Play CDN
      :linenos:
      :emphasize-lines: 7

      <!doctype html>
      <html>
         <head>
            
            ...

            <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
         
         </head>
         <body> ... </body>
      </html>

2. Compruebe el resultado de la petición en el navegador. 

Sección Principal (Hero Section)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Reemplace el contenido del cuerpo del documento HTML, por:

   .. code-block:: html
      :linenos:
      :emphasize-lines: 2-11

      <body>
         <main>
            <div id="container-01">
               <h1>Tu presencia digital comienza aquí</h1>
               <p>Creamos experiencias web atractivas y rápidas con Tailwind CSS 4.1. Dale vida a tus ideas con un diseño moderno y responsivo.</p>
               <div id="container-02">
                  <button id="start">Comenzar</button>
                  <button id="demo">Ver demo</button>
               </div>
            </div>
         </main>
      </body>

2. Compruebe el resultado en el navegador. 

Clases utilitarias
^^^^^^^^^^^^^^^^^^

1. Utilice la documentación `TailwindCSS - Estilo con clases de utilidad <https://tailwindcss.com/docs/styling-with-utility-classes>`_ y el inspector del navegador para cada uno de los siguientes cambios:

   a) Incorpore la clase "bg-slate-50" a la etiqueta `<main>`.
   b) Modifique la etiqueta `<div id=\"container-01\">` agregándole la clase "mx-auto px-4 py-20 text-center".
   c) Agregue las clases "text-4xl font-extrabold tracking-tight text-gray-900" a la etiqueta `<h1>`
   d) Añada a la etiqueta `<p>` las clases "mt-6 text-lg leading-relaxed text-gray-600 max-w-2xl mx-auto"
   e) Agregue las clases "inline-block px-6 py-3 text-white bg-blue-600 rounded-lg" al elemento `<button id=\"start\">`. Y, las clases "inline-block px-6 py-3 border border-gray-300 text-gray-700 rounded-lg" al elemento `<button id=\"demo\">`.

2. Compruebe el resultado en el navegador. 

Flex
^^^^

1. Utilice la documentación de `TailwindCSS - Layout <https://tailwindcss.com/docs/display>`_ y el inspector del navegador

2. Utilice un cliente de IAG para:

   a) Generar las clases para un margen superior de 2rem, convierte el contenedor en un contenedor flexbox, centra horizontalmente los elementos hijos y establece un espacio uniforme de 1rem entre ellos en la etiqueta `<div id=\"container-02\">`.

   .. admonition:: Prompt sugerido

      Para TailwindCSS versión 4.1, genera las clases para un margen superior de 2rem, convierte el contenedor en un contenedor flexbox, centra horizontalmente los elementos hijos y establece un espacio uniforme de 1rem entre ellos en la etiqueta <div id=\"container-02\">.

   .. dropdown:: Ver la solución
    :color: success

    .. code-block:: html
        
        <div id="container-02" class="mt-8 flex justify-center gap-4"> ... </div>

3. Compruebe el resultado en el navegador. 

Modo Oscuro
^^^^^^^^^^^

1. Verifique `modo oscuro de Chrome <https://support.google.com/chrome/answer/9275525>`_ de la landing page. 
2. Utilice la documentación de `TailwindCSS - Dark Mode <https://tailwindcss.com/docs/dark-mode>`_ y el inspector del navegador para cada uno de los siguientes cambios:

   a) Modifica la etiqueta `<main>` al agregar la clase "dark:bg-gray-900".
   b) Añade a la etiqueta `<h1>` la clase "dark:text-white" y a la etiqueta `<p>` la clase "dark:text-gray-300"
   c) Agregue a la etiqueta `<button id=\"demo\">` las clases "dark:border-gray-600 dark:text-white"

3. Compruebe el resultado en el navegador. 

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *landing*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Vercel
------

1. Acceda al sitio `Vercel <https://vercel.com/>`_.
2. Obtenga una cuenta en :term:`Vercel` a partir de su cuenta GitHub.
3. Autoriza a Vercel para que acceda a tus repositorios (puedes limitar a repos específicos si lo deseas).
4. Dentro de Vercel, haz clic en el botón **Import Project**.
5. Vercel mostrará una lista de tus repositorios de GitHub. Haz clic en el botón **Import** del repositorio que contiene tu proyecto.
6. Vercel detectará automáticamente el framework (si usas Vite, Next.js, etc.). En este caso mostrará **Vite**.
7. Haga clic en el botón **Deploy**.
8. Se generará un dominio automático como `[nombre_del_repositorio]-[nombre_del_contenedor].vercel.app`.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué aprendiste sobre el propósito y funcionamiento de las clases de TailwindCSS? 
    
    * ¿Qué estrategias implementaste para garantizar que el modo oscuro se adaptaran de forma funcional y estética?

    * ¿Cómo puedes equilibrar el uso de inteligencia artificial como apoyo en tu proceso creativo y técnico sin depender completamente de ella ni comprometer tu desarrollo autónomo como profesional del diseño web?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    Tailwind CSS

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">✨ Tailwind CSS v4.0 is here!<br><br>Huge performance improvements, radically simplified setup experience, CSS-first configuration, modernized P3 color palette, container queries, 3D transforms, expanded gradient APIs, @​starting-style support…<br><br>…and tons, tons more. <a href="https://t.co/zBSfm6IOf7">pic.twitter.com/zBSfm6IOf7</a></p>&mdash; Adam Wathan (@adamwathan) <a href="https://twitter.com/adamwathan/status/1882219476600635677?ref_src=twsrc%5Etfw">January 23, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>