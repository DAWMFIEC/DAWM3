..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

===========================================
Guía 01: WikiTracker
===========================================

.. topic:: Objetivo específico
   :class: objetivo

   Explorar el registro de interacciones con artículos de Wikipedia mediante la creación, ejecución, observación y análisis de una extensión de Chrome con JavaScript y Manifest V3.

Actividades en clases
====================

¿Cómo puede Chrome reconocer que estamos visitando Wikipedia?
-------------------------------------------------------------

#. Cree una carpeta `wikitracker` y, dentro de ella, un archivo `manifest.json` con el siguiente contenido:

   .. code-block:: json

      {
        "manifest_version": 3,
        "name": "WikiTracker",
        "version": "1.0",
        "description": "Registro de interacciones en Wikipedia",
        "content_scripts": [
          {
            "matches": ["https://*.wikipedia.org/wiki/*"],
            "js": ["content.js"],
            "run_at": "document_idle"
          }
        ]
      }

   En la misma carpeta, cree el archivo `content.js`:

   .. code-block:: javascript
    
      (() => {

        console.log("WikiTracker está activo");
        console.log("Título:", document.title);
        console.log("Dirección:", location.href);
      
      })();

#. Abra ``chrome://extensions``, active **Modo desarrollador** y seleccione **Cargar descomprimida** para elegir la carpeta `wikitracker`. 
#. Abra `Javascript - Wikipedia <https://es.wikipedia.org/wiki/JavaScript>`_ y examine la pestaña **Console** de las herramientas para desarrolladores. Compare lo observado con el contenido de `content.js`.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué archivo permite a Chrome identificar la carpeta como una extensión?
#. ¿Qué diferencias observa entre el contenido de `manifest.json` y el de `content.js`?
#. ¿Dónde aparecen los mensajes generados al visitar el artículo?
#. ¿Qué sucede cuando visita otro artículo de Wikipedia?
#. ¿Qué observa al abrir una página que no pertenece a Wikipedia?
#. ¿Qué conclusiones puede sacar sobre el uso de **JavaScript**?
#. ¿Cuál es la utilidad de los objetos `console`, `document` y `location`?
#. ¿Qué asegura la función anónima autoejecutable `(()=> {})()` en `content.js`?

¿Cómo puede Chrome reconocer el título y detectar los clics en un artículo de Wikipedia?
----------------------------------------------------------------------------------------

#. Actualice el contenido de `content.js` por el siguiente código. 

   .. code-block:: javascript
      :emphasize-lines: 7-8, 10-14

      (() => {

        console.log("WikiTracker está activo");
        console.log("Título:", document.title);
        console.log("Dirección:", location.href);

        const titulo = document.querySelector("h1");
        console.log("Artículo:", titulo?.textContent ?? "Sin título");

        let clics = 0;
        document.addEventListener("click", function () {
            clics = clics + 1;
            console.log("Clics registrados:", clics);
        });

      })();

#. Recargue la extensión desde ``chrome://extensions``, actualice el artículo de Wikipedia y observe la consola.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Complete la tabla con las observaciones realizadas.

   .. list-table:: Registro de observaciones
      :header-rows: 1
      :widths: 35 30 35

      * - Elemento observado
        - Resultado obtenido
        - ¿Cuál es el propósito?
      * - ``document.querySelector("h1")``
        -
        -
      * - ``titulo?.textContent``
        -
        -
      * - ``document.addEventListener("click", function () { ... })``
        -
        -

#. ¿Qué relación encuentra entre un clic del usuario, la ejecución de una función y el cambio del contador?
#. Además de los clics, ¿qué otros eventos podría registrar la extensión en Chrome? ¿Cómo podría hacerlo?

¿Cómo acceder a las secciones de un artículo de Wikipedia?
----------------------------------------------------------

#. Actualice el contenido de `content.js` por el siguiente código. 

   .. code-block:: javascript
      :emphasize-lines: 16-19

      (() => {

        console.log("WikiTracker está activo");
        console.log("Título:", document.title);
        console.log("Dirección:", location.href);

        const titulo = document.querySelector("h1");
        console.log("Artículo:", titulo?.textContent ?? "Sin título");

        let clics = 0;
        document.addEventListener("click", function () {
            clics = clics + 1;
            console.log("Clics registrados:", clics);
        });

        const secciones = document.querySelectorAll("h2");
        secciones.forEach((seccion, index) => {
                console.log(`Sección ${index + 1}:`, seccion.textContent);
        });

      })();

#. Recargue la extensión desde ``chrome://extensions``, actualice el artículo de Wikipedia y observe la consola.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué diferencia existe entre `document.querySelector` y `document.querySelectorAll`?
#. Investigue qué es un `NodeList` y cómo se diferencia de un `Array` (Ramez, 2023). ¿Qué métodos de iteración puede utilizar con cada uno?

¿Qué está visible en un artículo de Wikipedia?
----------------------------------------------

#. Actualice el contenido de `content.js` por el siguiente código. 

   .. code-block:: javascript
      :emphasize-lines: 3-11, 28-30

      (() => {

        // Función para determinar si un elemento está visible
        function estaVisible(elemento) {
            const rect = elemento.getBoundingClientRect();

            return (
                rect.top >= 0 &&
                rect.bottom <= window.innerHeight
            );
        }

        console.log("WikiTracker está activo");
        console.log("Título:", document.title);
        console.log("Dirección:", location.href);

        const titulo = document.querySelector("h1");
        console.log("Artículo:", titulo?.textContent ?? "Sin título");

        let clics = 0;
        document.addEventListener("click", function () {
            clics = clics + 1;
            console.log("Clics registrados:", clics);
        });

        const secciones = document.querySelectorAll("h2");
        secciones.forEach((seccion, index) => {
            if (estaVisible(seccion)) {
                console.log(`Sección ${index + 1}:`, seccion.textContent);
            }
        });

      })();
    
Referencias
===========

* Chrome extensions documentation. (n.d.). Chrome for Developers. Retrieved October 18, 2026 from https://developer.chrome.com/docs/extensions/
* Document: querySelector() method. (n.d.). MDN Web Docs. Retrieved October 18, 2026 from https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector
* Ramez, C. (2023). NodeList vs Array - DEV Community. DEV Community. https://dev.to/cesar_ramez/nodelist-vs-array-87.