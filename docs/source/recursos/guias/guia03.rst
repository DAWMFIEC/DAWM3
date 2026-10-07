..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 03: Estilo de un sitio web
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Comprender la importancia del estilo en un sitio web mediante la ejecución, observación y análisis de elementos CSS que afectan la apariencia de un documento.

Actividades en clases
=====================

¿Cómo separo el diseño del contenido de mi página?
--------------------------------------------------

1. Crea la carpeta `css/` en la misma carpeta que contiene `index.html`.
2. Crear el archivo `css/estilos.css` y enlazarlo dentro de `<head>`, justo después de `<title>`:

.. code-block:: html
    :emphasize-lines: 4

    <head>
        ...
        <title>Ana Pérez | Currículum vitae</title>
        <link rel="stylesheet" href="css/estilos.css">
    </head>

3. Agrega el siguiente contenido al archivo `css/estilos.css`:

.. code-block:: css

    body {
        background-color: #f4f6f8;
        color: #222;
    }

4. Desde el directorio donde se encuentran los archivos, ejecuta el siguiente comando en la terminal para iniciar un servidor web local:

.. code-block:: bash
    
    python -m http.server 8000

5. Luego, abre tu navegador web y visita `http://localhost:8000` para ver tu sitio web en acción.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Si mañana el CV tuviera 5 páginas, ¿qué ventaja concreta te da tener los estilos en un archivo aparte?
2. ¿Qué nombre y ubicación del archivo CSS ayudarían a otra persona a encontrar los estilos sin preguntarte?
3. Utilice el **DevTools** para cambiar el *href* a `css/estilo.css` (sin la s). ¿Qué código aparece en Network y qué le pasa a la página?

¿Cómo elijo a qué elementos aplicar un estilo?
----------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

¿Qué tan preciso debo ser al señalar un elemento?
----------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Referencias
============

* CSS | MDN. (n.d.). Retrieved October 07, 2026 from https://developer.mozilla.org/es/docs/Web/CSS