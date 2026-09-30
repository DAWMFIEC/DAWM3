..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 01: Cliente y servidor en la web 
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Comprender el modelo cliente-servidor en la Web, identificando el rol del navegador y del servidor durante el proceso de solicitud y respuesta de recursos, mediante la ejecución, observación y análisis de aplicaciones web sencillas.

Actividades en clases
=====================

¿Quién es el cliente y quién el servidor cuando usas WhatsApp Web, Netflix y una impresora de red?
--------------------------------------------------------------------------------------------------

Copia el siguiente código en tu editor de texto y guárdalo como `index.html`:

.. code-block:: html
    
    <!DOCTYPE html>
    <html lang="es">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Mi primer sitio web</title>
    </head>
    <body>
        <h1>¡Hola, mundo!</h1>
        <p>Este es mi primer sitio web.</p>
    </body>
    </html>

Desde el directorio donde se encuentra `index.html`, ejecuta el siguiente comando en la terminal para iniciar un servidor web local:

.. code-block:: bash
    
    python -m http.server 8000

Luego, abre tu navegador web y visita `http://localhost:8000` para ver tu sitio web en acción.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Quién cumple el rol de cliente y quién el de servidor?
2. En la actividad realizada, ¿qué programa está actuando como cliente?
3. ¿Qué elemento está actuando como servidor web?
4. ¿Qué representa localhost en la dirección http://localhost:8000?
5. ¿Qué representa el número 8000?
6. ¿Qué ocurre desde que escribe http://localhost:8000 en el navegador hasta que aparece “¡Hola, mundo!”?
7. ¿Cuál es la diferencia entre abrir directamente index.html y acceder a él mediante http://localhost:8000?
8. En esta actividad, ¿el cliente y el servidor están en la misma computadora? ¿Podrían encontrarse en computadoras diferentes?

¿Qué ocurre cuando escribo una URL?
-----------------------------------

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué representa ::1 al inicio de cada línea? ¿Qué relación tiene con localhost?
2. ¿Qué indica el método GET en "GET / HTTP/1.1"?
3. ¿Qué recurso está solicitando el navegador cuando aparece GET /?
4. ¿Qué significa HTTP/1.1 en el registro de la solicitud?
5. ¿Qué significa el código de estado 200? ¿Qué ocurrió con la solicitud?
6. Compare estas dos respuestas:
    - "GET / HTTP/1.1" 200
    - "GET / HTTP/1.1" 304
    ¿Qué diferencia existe entre los códigos 200 y 304?

¿Qué es HTTP?
-----------------------------------

Referencias
============

* 3.14.7 Documentation. (n.d.). Python Documentation. Retrieved September 30, 2026 from https://docs.python.org/es/3/
* HTTP | MDN. (n.d.). Retrieved September 30, 2026 from https://developer.mozilla.org/es/docs/Web/HTTP