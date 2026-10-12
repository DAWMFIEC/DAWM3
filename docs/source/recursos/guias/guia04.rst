..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 04: Control de versiones y publicación web con Git y GitHub
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Aplicar el control de versiones para la gestión de la historia de un sitio web estático, mediante la aplicación de buenas prácticas de desarrollo.

Actividades en clases
=====================

¿Dónde vive mi código cuando no está en mi computadora?
-------------------------------------------------------

Paso 1. Crear el repositorio remoto
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Inicie sesión en `GitHub.com <https://github.com>`_.
2. Haga clic en el botón **New** (o en **+** → **New repository**).
3. Complete el formulario:
   
   * Repository name: *dawm-cv*
   * Description: *Curriculum - DAWM*
   * Visibility: **Public** (necesario para publicar gratis con `GitHub Pages <https://pages.github.com/>`_).
   * Marque **Add README**.

4. Haga clic en **Create repository**.

Paso 2. Abrir un Codespace
^^^^^^^^^^^^^^^^^^^^^^^^^^

1. En la página del repositorio, haga clic en el botón verde **<> Code**.
2. Seleccione la pestaña **Codespaces** y luego **Create codespace on main**.
3. Espere a que cargue el editor (*Visual Studio Code* en el navegador).

Paso 3. Reconocer el entorno
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Abra la terminal integrada ({kbd}Ctrl + {kbd}ñ o menú **Terminal** → **New Terminal**)
2. Ejecute los siguientes comandos para explorar el entorno del Codespace:
 
   - Con `pwd` para imprimir el directorio de trabajo actual
   - Use `ls -la` para listar los archivos (incluyendo ocultos)
   - Use `git --version` para ver la versión de Git instalada
   - Con `git status` para ver el estado del repositorio Git
   - Utilice `git remote -v` para ver la URL del repositorio remoto.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Cuál es la diferencia entre Git y GitHub? ¿Podría usar Git sin GitHub? ¿Y GitHub sin Git?
2. El comando *git remote -v* mostró una URL llamada *origin*. ¿Qué representa ese nombre y por qué existen dos líneas (*fetch* y *push*)?
3. Usted no instaló Python, Git ni un editor de código. ¿Dónde se están ejecutando realmente? Compare un Codespace con trabajar en el laboratorio o en su laptop: ventajas, desventajas y costos (investigue la cuota mensual gratuita de Codespaces).
4. La URL de previsualización del puerto 8000 es privada por defecto. ¿Por qué es una buena práctica de seguridad? ¿En qué caso la haría pública?
5. ¿Qué ocurre si cierra la pestaña del navegador sin detener el Codespace? ¿Por qué conviene detenerlo o eliminarlo cuando no se usa?

Sb1
---------------------------------------------------------------

1. .
2. .

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. .
2. .

Sb1
---------------------------------------------------------------

1. .
2. .

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. .
2. .

Sb1
---------------------------------------------------------------

1. .
2. .

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. .
2. .

Sb1
---------------------------------------------------------------

1. .
2. .

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. .
2. .

Referencias
============

* Git. (s. f.). Git reference documentation. Recuperado el 11 de octubre de 2026, de https://git-scm.com/docs
* GitHub. (s. f.-a). Acerca de GitHub Pages. GitHub Docs. Recuperado el 11 de octubre de 2026, de https://docs.github.com/es/pages/getting-started-with-github-pages/about-github-pages
* GitHub. (s. f.-c). Documentación de GitHub Codespaces. GitHub Docs. Recuperado el 11 de octubre de 2026, de https://docs.github.com/es/codespaces