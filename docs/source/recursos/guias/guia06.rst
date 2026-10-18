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

1. Cree una carpeta `wikitracker` y, dentro de ella, un archivo `manifest.json` con el siguiente contenido:

   .. code-block:: json

      {
        "manifest_version": 3,
        "name": "WikiTracker",
        "version": "1.0",
        "description": "Registro de interacciones en Wikipedia",
        "content_scripts": [
          {
            "matches": ["https://*.wikipedia.org/wiki/*"],
            "js": ["content.js"]
          }
        ]
      }

   En la misma carpeta, cree el archivo `content.js`:

   .. code-block:: javascript

      console.log("WikiTracker está activo");
      console.log("Título:", document.title);
      console.log("Dirección:", location.href);

2. Abra ``chrome://extensions``, active **Modo desarrollador** y seleccione **Cargar descomprimida** para elegir la carpeta `wikitracker`. Abra ``https://es.wikipedia.org/wiki/JavaScript`` y examine la pestaña **Console** de las herramientas para desarrolladores. Compare lo observado con el contenido de `content.js`.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué archivo permite a Chrome identificar la carpeta como una extensión?
2. ¿Qué diferencias observa entre el contenido de `manifest.json` y el de `content.js`?
3. ¿Dónde aparecen los mensajes generados al visitar el artículo?
4. ¿Qué sucede cuando visita otro artículo de Wikipedia?
5. ¿Qué observa al abrir una página que no pertenece a Wikipedia?
6. ¿Qué preguntas le surgen sobre la relación entre la extensión y la página visitada?

¿Cómo podemos detectar clics y qué ocurre si un elemento no existe?
-------------------------------------------------------------------

#. Reemplace el contenido de `content.js` por el siguiente código. Recargue la extensión desde ``chrome://extensions`` y actualice el artículo de Wikipedia. Haga clic en distintos elementos y observe la consola.

   .. code-block:: javascript

      let clics = 0;
      const titulo = document.querySelector("h1");

      console.log("Artículo:", titulo?.textContent ?? "Sin título");

      document.addEventListener("click", function () {
        clics = clics + 1;
        console.log("Clics registrados:", clics);
      });

#. Modifique el selector para buscar un recurso que no exista en el artículo. Ejecute nuevamente el código y observe el resultado. Compare ambas ejecuciones.

   .. code-block:: javascript
      :emphasize-lines: 2,4

      let clics = 0;
      const titulo = document.querySelector("#elemento-inexistente-wikitracker");

      console.log("Artículo:", titulo?.textContent ?? "Sin título");

      document.addEventListener("click", function () {
        clics = clics + 1;
        console.log("Clics registrados:", clics);
      });

#. Complete la tabla con observaciones de las dos versiones y de las interacciones realizadas.

   .. list-table:: Registro de observaciones
      :header-rows: 1
      :widths: 35 30 35

      * - Elemento observado
        - Resultado obtenido
        - ¿Qué pregunta plantea?
      * - ``document.querySelector("h1")``
        -
        -
      * - ``document.querySelector("#elemento-inexistente-wikitracker")``
        -
        -
      * - ``titulo?.textContent``
        -
        -
      * - ``addEventListener("click", ...)``
        -
        -
      * - ``clics = clics + 1``
        -
        -

#. Observe la siguiente figura de referencia y compárela con la consola de su navegador.

   .. figure:: ../img/wikitracker_consola_eventos.png
      :alt: Consola de Chrome con mensajes de WikiTracker y registro de clics sobre Wikipedia
      :align: center

#. Complete una definición propia a partir de sus observaciones.

   .. centered::

      ___ es ___ y permite ___.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué relación encuentra entre un clic del usuario, la ejecución de una función y el cambio del contador?
#. ¿Qué modificaría para que el registro continúe funcionando cuando no se encuentre el título del artículo?

¿Cómo convertir los eventos en datos visibles de una extensión?
---------------------------------------------------------------

#. Observe los resultados de la actividad anterior y compárelos con la siguiente representación del registro de eventos. Identifique qué datos están disponibles y cuáles faltan.

   .. figure:: ../img/wikitracker_resultado_eventos.png
      :alt: Resultado del registro de interacciones con título del artículo y contador de clics
      :align: center

#. Investigue los componentes que intervendrían en un registro persistente y complete la tabla. Escriba primero su hipótesis y después contraste con la documentación oficial.

   .. list-table:: Hipótesis e investigación
      :header-rows: 1
      :widths: 28 35 37

      * - Componente
        - ¿Qué cree que significa?
        - Definición después de investigar
      * - ``manifest.json``
        -
        -
      * - ``content.js``
        -
        -
      * - ``popup.html``
        -
        -
      * - ``popup.js``
        -
        -
      * - ``chrome.storage.local``
        -
        -

#. Cree `popup.html` y `popup.js` para observar cómo una interfaz recupera un valor de prueba guardado localmente.

   .. code-block:: html

      <!DOCTYPE html>
      <html lang="es">
      <head>
        <meta charset="UTF-8">
        <title>WikiTracker</title>
      </head>
      <body>
        <h1>WikiTracker</h1>
        <p>Clics registrados: <strong id="clics">0</strong></p>
        <script src="popup.js"></script>
      </body>
      </html>

   .. code-block:: javascript

      chrome.storage.local.set({ clics: 3 }, function () {
        chrome.storage.local.get(["clics"], function (datos) {
          document.querySelector("#clics").textContent = datos.clics ?? 0;
        });
      });

   Reemplace `manifest.json` por la siguiente configuración y recargue la extensión. Abra el popup desde su icono.

   .. code-block:: json

      {
        "manifest_version": 3,
        "name": "WikiTracker",
        "version": "1.0",
        "description": "Registro de interacciones en Wikipedia",
        "permissions": ["storage"],
        "content_scripts": [
          {
            "matches": ["https://*.wikipedia.org/wiki/*"],
            "js": ["content.js"]
          }
        ],
        "action": {
          "default_popup": "popup.html"
        }
      }

#. Examine el siguiente esquema y complete sus componentes y relaciones a partir de la ejecución realizada.

   .. figure:: ../img/wikitracker_esquema_completar.png
      :alt: Esquema incompleto de la comunicación entre Wikipedia, content script, almacenamiento y popup
      :align: center

   * ¿Qué componente detecta los clics dentro del artículo?
   * ¿Qué archivo configura dónde se ejecuta la extensión?
   * ¿Qué componente debería guardar los valores registrados?
   * ¿Qué archivo obtiene los valores que aparecen en el popup?
   * ¿Qué recorrido deberían seguir los datos desde Wikipedia hasta la interfaz?

#. Investigue y clasifique los siguientes casos. Complete las dos últimas columnas a partir de pruebas y documentación.

   .. list-table:: Clasificación de casos
      :header-rows: 1
      :widths: 40 25 35

      * - Caso o código
        - Tipo o categoría
        - Uso o comportamiento observado
      * - ``click``
        -
        -
      * - ``scroll``
        -
        -
      * - ``document.querySelector("h1")``
        -
        -
      * - ``null``
        -
        -
      * - ``chrome.storage.local.set()``
        -
        -
      * - ``chrome.storage.local.get()``
        -
        -
      * - ``content_scripts.matches``
        -
        -
      * - ``action.default_popup``
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué diferencias identifica entre los datos generados en la página y los que se muestran en el popup?
#. ¿Qué cambios investigaría para reemplazar el valor de prueba por el número real de clics?
#. ¿Cómo organizaría una prueba para verificar que los datos permanecen después de cerrar el popup?

Referencias
===========

* Chrome extensions documentation. (n.d.). Chrome for Developers. Retrieved October 18, 2026 from https://developer.chrome.com/docs/extensions/
* Document: querySelector() method. (n.d.). MDN Web Docs. Retrieved October 18, 2026 from https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector
