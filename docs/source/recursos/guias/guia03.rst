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

¿Cómo logra una página cambiar de apariencia sin tocar su HTML?
---------------------------------------------------------------

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

    body { background-color: #f4f6f8; }

4. Desde el directorio donde se encuentran los archivos, ejecuta el siguiente comando en la terminal para iniciar un servidor web local:

.. code-block:: bash
    
    python -m http.server 8000

5. Luego, abre tu navegador web y visita `http://localhost:8000` para ver tu sitio web en acción.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Si mañana el CV tuviera 5 páginas, ¿qué ventaja concreta te da tener los estilos en un archivo aparte?
2. ¿Qué nombre y ubicación del archivo CSS ayudarían a otra persona a encontrar los estilos sin preguntarte?
3. Utilice el **DevTools** para cambiar el *href* a `css/estilo.css` (sin la s). ¿Qué código de estado HTTP mostró **Network** cuando la ruta estaba mal? ¿Por qué la página se sigue viendo, aunque sin estilos?
4. ¿Qué es CSS?

¿Por qué mi caja mide más de lo que le dije?
-------------------------------------------------

1. Agrega la siguiente regla temporal:

.. code-block:: css

    main { width: 600px; padding: 32px; border: 2px solid red; }

2. En DevTools, inspecciona `<main>` y revisa el diagrama del modelo de caja (pestaña Computed). Anota el ancho total que ocupa en pantalla. 

3. Agrega al inicio de `estilos.css` el selector universal:

.. code-block:: css

    * { box-sizing: border-box; }

4. Vuelve a medir `<main>`. Anota el nuevo ancho total y luego borra la regla temporal del **paso 1**.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué es el modelo de caja en CSS?
2. ¿Por qué el ancho total de la caja es mayor que el ancho que le asigné?
3. ¿Qué propiedades del modelo de caja están afectando el tamaño final de la caja?
4. ¿Cómo puedo hacer que el ancho total de la caja sea igual al ancho que le asigné?

¿Cómo doy una identidad visual a todo el documento con pocas reglas?
---------------------------------------------------------------------

1. Agrega la siguiente regla al inicio de `estilos.css`:

.. code-block:: css

    body {
        margin: 0;
        background-color: #f4f6f8;
        color: #263238;
        font-family: Arial, Helvetica, sans-serif;
        line-height: 1.6;
    }

    header { background-color: #1f3a5f; color: white; padding: 32px; }
    nav    { margin-top: 16px; }
    main   { max-width: 1000px; margin: auto; padding: 32px; }

    section {
        background-color: #ffffff;
        margin-bottom: 32px;
        padding: 32px;
        border-radius: 10px;
        box-shadow: 0 4px 14px rgba(0, 0, 0, 0.08);
    }

    footer {
        padding: 32px;
        background-color: #1f3a5f;
        color: white;
        text-align: center;
    }

    h1 { margin: 0; font-size: 2.4rem; }
    h2 { margin-top: 0; color: #1f3a5f; border-bottom: 2px solid #d9e0e6; padding-bottom: 8px; }
    h3 { margin-bottom: 8px; color: #4f6d8a; }
    p  { margin-top: 8px; }
    ul { padding-left: 24px; }
    li { margin-bottom: 6px; }
    a  { color: #2f80a3; text-decoration: none; }

2. Guarda los cambios y recarga la página en el navegador. Observa cómo cambia la apariencia del sitio web.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Por qué el texto de `<h1>` se ve blanco si nunca le diste color? Usa *DevTools → Computed → Show all* para rastrear de dónde lo hereda.
2. Imagina que el HTML usara solo `<div>` en lugar de `<header>`, `<main>`, `<section>` y `<footer>`. ¿Qué pasaría con estas reglas? ¿Qué relación hay entre HTML semántico y CSS mantenible?
3. Cambia `line-height: 1.6` por `line-height: 1.6em` y observa el `<h1>`. ¿Por qué se recomienda el valor sin unidad?

¿Por qué una imagen puede romper el diseño en un celular?
---------------------------------------------------------

1. En **DevTools** activa la barra de dispositivos (Ctrl+Shift+M) y elige un ancho de *375px*. Ve a la sección Proyectos: la imagen y el video de *640 px* se salen de la tarjeta.

2. Agrega la siguiente regla al final de `estilos.css`:

.. code-block:: css

    figure { margin: 32px 0; }

    img {
        display: block;
        max-width: 100%;
        height: auto;
        border-radius: 10px;
    }

    video {
        display: block;
        max-width: 100%;
        border-radius: 10px;
    }

    figcaption {
        margin-top: 8px;
        color: #66727c;
        font-size: 0.9rem;
    }

3. Guarda los cambios y recarga la página en el navegador. Observa cómo cambia la apariencia del sitio web.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Por qué una imagen puede romper el diseño en un celular?
2. ¿Qué efecto tiene la propiedad `max-width: 100%` en las imágenes y videos?
3. ¿Por qué es importante usar `display: block` para las imágenes y videos?
4. ¿Qué efecto tiene la propiedad `height: auto` en las imágenes y videos?
5. Quita `height: auto` y prueba a `375 px`. ¿Qué le pasa a la proporción de la imagen? ¿Por qué ocurre si el HTML trae `height="360"`?

sb1
-------------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

sb1
-------------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

sb1
-------------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

sb1
-------------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

sb1
-------------------------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^


Referencias
============

* CSS | MDN. (n.d.). Retrieved October 07, 2026 from https://developer.mozilla.org/es/docs/Web/CSS