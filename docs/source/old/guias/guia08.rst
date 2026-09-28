..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

=================================================
Guía 08: Javascript - Introducción, DOM y Eventos
=================================================

.. topic:: Objetivo específico
    :class: objetivo

    Proponer código de scripting para la manipulación del DOM y manejo de eventos.

Actividades previas
=====================

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

Archivos y estructura
----------------------

1. Cree la carpeta *js* dentro del directorio de tu proyecto.
2. Cree el documento :term:`Javascript` *file01.js* dentro de la carpeta *js* de tu proyecto. Declare el documento el modo estricto.
 
   .. code-block:: javascript

       "use strict";

3. Referencie el documento *file01.js*, con el tipo \"module\", en el documento *index.html* al final de la etiqueta `<body>`. Valide su respuesta con la guía para :download:`referenciar una hoja de estilo externa <./pdfs/guia08-referenciajs.pdf>`.

HTML
----

1. Modifique el documento *index.html*, con:

   .. code-block:: html
       :caption: Notificación interactiva.
       :emphasize-lines: 4-36

       <body>
            ...
    
            <div id="toast-interactive"
                class="fixed right-5 bottom-5 flex items-center w-full max-w-xs p-4 space-x-4 text-gray-500 bg-white divide-x divide-gray-200 rounded-lg shadow-sm dark:text-gray-400 dark:divide-gray-700 dark:bg-gray-800"
                role="alert">
                <div class="flex">
                
                    <!-- Content -->
                    <div class="ms-3 text-sm font-normal">
                        <span class="mb-1 text-sm font-semibold text-gray-900 dark:text-white">Suscríbete ahora</span>
                        <div class="mb-2">Suscríbete ahora para obtener beneficios de cliente premium y pro.</div>
                        <div class="grid grid-cols-2 gap-2">
                        <div>
                            <a href="#"
                            class="inline-flex justify-center w-full px-2 py-1.5 text-xs font-medium text-white bg-blue-600 rounded-lg hover:bg-blue-700 focus:ring-4 focus:outline-none focus:ring-blue-300 dark:bg-blue-500 dark:hover:bg-blue-600 dark:focus:ring-blue-800">Sí, quiero</a>
                        </div>
                        <div>
                            <a href="#"
                            class="inline-flex justify-center w-full px-2 py-1.5 text-xs font-medium text-gray-900 bg-white border border-gray-300 rounded-lg hover:bg-gray-100 focus:ring-4 focus:outline-none focus:ring-gray-200 dark:bg-gray-600 dark:text-white dark:border-gray-600 dark:hover:bg-gray-700 dark:hover:border-gray-700 dark:focus:ring-gray-700">No ahora</a>
                        </div>
                        </div>
                    </div>

                    <!-- Close button -->
                    <button type="button"
                        class="ms-auto -mx-1.5 -my-1.5 p-1.5 inline-flex items-center justify-center w-8 h-8 text-gray-400 bg-white rounded-lg hover:text-gray-900 hover:bg-gray-100 focus:ring-2 focus:ring-gray-300 dark:text-gray-500 dark:hover:text-white dark:bg-gray-800 dark:hover:bg-gray-700"
                        data-dismiss-target="#toast-interactive" aria-label="Close">
                        <span class="sr-only">Close</span>
                        <svg class="w-3 h-3" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 14 14" aria-hidden="true">
                        <path stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2"
                            d="m1 1 6 6m0 0 6 6M7 7l6-6M7 7l-6 6" />
                        </svg>
                    </button>
                </div>
            </div>

            <script ... ></script>
            <script ... ></script>

        </body>
    
        

2. Compruebe la vista previa del resultado en el navegador.

   .. note:: El código anterior crea una notificación interactiva que permite al usuario suscribirse a un servicio o cerrar la notificación.

3. Agregue la clase `hidden` al elemento `<div id=\"toast-interactive\">` para ocultar la notificación inicialmente, con:

   .. code-block:: html
       :caption: Notificación interactiva oculta
       :emphasize-lines: 2

       <div id="toast-interactive" 
            class="hidden ... " 
            role="alert">

4. Compruebe la vista previa del resultado en el navegador.


Javascript: función de autoejecución
------------------------------------

1. Utilice un cliente de IAG en el documento *js/file01.js*, para:

   a) Crea una :term:`función de autoejecución` que muestre un mensaje de bienvenida con una alerta y por consola.
    
   .. dropdown:: Ver la solución
    :color: success

    .. code-block:: javascript
        :emphasize-lines: 3-6

        "use strict";

        (() => {
            alert("¡Bienvenido a la página!");
            console.log("Mensaje de bienvenida mostrado.");
        })();

2. Compruebe la vista previa del resultado y la :term:`consola del navegador` para verificar la ejecución del código.


Javascript: DOM
---------------

1. Utilice un cliente de IAG en el documento *js/file01.js*, para:

   a) Crear la :term:`función flecha` **showToast** que obtiene la referencia al elemento con el ID `toast-interactive`. En caso de existir la referencia muestre la notificación por pantalla, agregando la clase `md:block`.

   b) Dentro de la función de autoejecución, elimine el codigo anterior y  llame a la función showToast.

   .. dropdown:: Ver la solución
    :color: success

    .. code-block:: javascript
        :emphasize-lines: 3-8, 11

        "use strict";

        const showToast = () => {
            const toast = document.getElementById("toast-interactive");
            if (toast) {
                toast.classList.add("md:block");
            }
        };

        (() => {
            showToast();
        })();

2. Compruebe la vista previa del resultado y la consola del navegador para verificar la ejecución del código.

Javascript: Eventos
-------------------

1. Utilice un cliente de IAG en el documento *js/file01.js*, para:

   a) Crear la función flecha **showVideo** que obtiene la referencia al elemento con el ID `demo`. En caso de existir la referencia, agregue un :term:`evento` `click` que abra un enlace a un video de YouTube en una nueva pestaña del navegador.
   b) Dentro de la función de autoejecución, agregue la llamada a la función showVideo.

   .. dropdown:: Ver la solución
    :color: success

    .. code-block:: javascript
        :emphasize-lines: 5-12, 16

        "use strict";

        const showToast = () => { ... }

        const showVideo = () => {
            const demo = document.getElementById("demo");
            if (demo) {
                demo.addEventListener("click", () => {
                    window.open("https://www.youtube.com/watch?v=dQw4w9WgXcQ", "_blank");
                });
            }
        };

        (() => {
            showToast();
            showVideo();
        })();

2. Compruebe la vista previa del resultado y la consola del navegador para verificar la ejecución del código.

JSDoc
-----

1. Utilice un cliente de IAG en el documento *js/file01.js*, para:

   a) Agregar comentarios JSDoc a las funciones **showToast** y **showVideo**.
   b) Asegúrese de que los comentarios JSDoc incluyan descripciones, parámetros y tipos de retorno.

2. Valide su respuesta con `JSDoc: La Guía Definitiva para Documentar tu Código JavaScript <https://dev.to/goaqidev/jsdoc-la-guia-definitiva-para-documentar-tu-codigo-javascript-ik5>`_.

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *landing*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Vercel
------

1. Verifique el despliegue continuo (CD) del proyecto en Vercel.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué limitaciones encontraste en las explicaciones o ejemplos generados por la IA que te motivaron a investigar más sobre cómo funcionan getElementById y addEventListener en el flujo de ejecución de JavaScript?
    
    * ¿Cómo resolviste errores o comportamientos inesperados al combinar estructuras condicionales con eventos del DOM, y qué papel tuvo tu razonamiento frente a lo sugerido por la IA?

    * ¿Cómo aseguras que el uso de la inteligencia artificial no sustituya tu proceso de aprendizaje, sino que complemente tu desarrollo autónomo al escribir e interpretar código JavaScript?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    DOM con JavaScript  

    <blockquote class="twitter-tweet"><p lang="es" dir="ltr">💛¡Las bases del DOM en un hilo!💛<br><br>📜Se conoce como DOM a la forma de referirnos a la estructura del documento HTML.<br><br>Mediante el DOM podemos cambiar el HTML/CSS dinámicamente desde Javascript y hacer cosas mágicas🧙‍♂️<br><br>¡Te explico en un par de tweets como dominarlo!<br><br>🧵⬇ <a href="https://t.co/zgjZsiDRVK">pic.twitter.com/zgjZsiDRVK</a></p>&mdash; Manz 🇮🇨⚡👾 (@Manz) <a href="https://twitter.com/Manz/status/1521890518976073729?ref_src=twsrc%5Etfw">May 4, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>