..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 02: Estructura y estilo de páginas web 
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Comprender el modelo cliente-servidor en la Web, identificando el rol del navegador y del servidor durante el proceso de solicitud y respuesta de recursos, mediante la ejecución, observación y análisis de aplicaciones web sencillas.

Actividades en clases
=====================

¿Qué necesito saber el navegador para mostrar una página web? 
-------------------------------------------------------------

1. Copia el siguiente código en tu editor de texto y guárdalo como `index.html`:

.. code-block:: html
    
    <!DOCTYPE html>
    <html lang="es">
        <head>
            <meta name="description" content="Currículum vitae de Ana Pérez, estudiante de Computación en ESPOL">
            <title>Ana Pérez | Currículum vitae</title>
        </head>
        <body>
            <h1>Ana Pérez</h1>
        </body>
    </html>

2. Desde el directorio donde se encuentra `index.html`, ejecuta el siguiente comando en la terminal para iniciar un servidor web local:

.. code-block:: bash
    
    python -m http.server 8000

3. Luego, abre tu navegador web y visita `http://localhost:8000` para ver tu sitio web en acción.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Abra las Herramientas para desarrolladores (**DevTools**) del navegador (`F12` o clic derecho → *Inspeccionar*) y seleccione la pestaña *Elements/Elementos*. ¿La estructura que muestra DevTools es similar al código fuente que escribió? ¿Qué observa?
2. ¿Se visualizan correctamente estos caracteres? 
3. Dentro del `<head>`, agregue `<meta charset=\"UTF-8\">`, guarde el archivo y recargue la página. ¿Observa algún cambio?
4. Investigue qué significa *charset* y cómo afecta la visualización de la página web en diferentes dispositivos.
5. Utilice el DevTools y active el modo de diseño adaptable (*Responsive Design Mode*). ¿Qué observa al cambiar el tamaño de la ventana del navegador?
6. Dentro del `<head>`, agregue `<meta name="viewport" content="width=device-width, initial-scale=1.0">`. Guarde el archivo y recargue la página. ¿Observa algún cambio? 
7. Investigue qué significa *viewport* y cómo afecta la visualización de la página web en diferentes dispositivos.
8. ¿Qué diferencia identifica entre <head> y <body>?
9. ¿Dónde aparece el texto "Ana Pérez" y dónde aparece el texto "Ana Pérez | Currículum vitae"? 
10. Si el usuario no la ve como parte de la página, ¿para quién o para qué podría resultar útil esta información? ¿Qué diferencia encuentra entre un **metadato** y el **contenido visible**?
11. Después de realizar los experimentos anteriores, clasifique los siguientes elementos según su función:

.. list-table::
    :header-rows: 1
    :widths: 30 15 30 20

    * - Elemento
    - Estructura
    - Metadato/configuración
    - Contenido visible
    * - `<!DOCTYPE html>`
    -
    -
    -
    * - `<html lang="es">`
    -
    -
    -
    * - `<meta charset="UTF-8">`
    -
    -
    -
    * - `<meta name="viewport">`
    -
    -
    -
    * - `<meta name="description">`
    -
    -
    -
    * - `<title>`
    -
    -
    -
    * - `<body>`
    -
    -
    -
    * - `<h1>`
    -
    -
    -

Subtítulo 2
-----------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Subtítulo 2
-----------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Subtítulo 2
-----------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Referencias
============

* HTML: lenguaje de marcado de hipertexto | MDN. (2026, 30 septiembre). https://developer.mozilla.org/es/docs/Web/HTML
* Especificación HTML | WHATWG. (2026, 5 octubre). https://html.spec.whatwg.org/