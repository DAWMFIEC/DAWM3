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

1. Abra la terminal integrada (`Ctrl + ñ` o menú **Terminal** → **New Terminal**)
2. Ejecute los siguientes comandos para explorar el entorno del Codespace:

.. code-block:: bash

    # Imprimir el directorio de trabajo actual
    pwd 
    
    # Listar los archivos y directorios en el directorio actual
    ls -la 
    
    # Verificar la versión de Git instalada
    git --version 

    # Estado del repositorio Git
    git status
    
    # Verificar la URL del repositorio remoto
    git remote -v

Paso 4. Crear la primera página del portafolio
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Guarde los *index.html* y *css/estilos.css* en la raíz del repositorio.
2. Levante un servidor web local para previsualizar el sitio:

.. code-block:: bash

    # Levantar un servidor web local en el puerto 8000
    python3 -m http.server 8000

**Nota:** Codespaces mostrará una notificación con el botón *Open in Browser* (o revise la pestaña **PORTS**). Abra el enlace y verifique que su página se muestra. Detenga el servidor con `Ctrl + C`.
  
Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Cuál es la diferencia entre Git y GitHub? ¿Podría usar Git sin GitHub? ¿Y GitHub sin Git?
2. El comando *git remote -v* mostró una URL llamada *origin*. ¿Qué representa ese nombre y por qué existen dos líneas (*fetch* y *push*)?
3. Usted no instaló Python, Git ni un editor de código. ¿Dónde se están ejecutando realmente? Compare un Codespace con trabajar en el laboratorio o en su laptop: ventajas, desventajas y costos (investigue la cuota mensual gratuita de Codespaces).
4. La URL de previsualización del puerto 8000 es privada por defecto. ¿Por qué es una buena práctica de seguridad? ¿En qué caso la haría pública?
5. ¿Qué ocurre si cierra la pestaña del navegador sin detener el Codespace? ¿Por qué conviene detenerlo o eliminarlo cuando no se usa?

¿Por qué guardar con `Ctrl + S` no es lo mismo que hacer un commit?
-------------------------------------------------------------------

1. Observe el estado del repositorio, con:

.. code-block:: bash

    git status

2. Prepare y confirme el primer cambio, con:

.. code-block:: bash

    # Agregar los archivos al área de preparación (staging area)
    git add index.html css/estilos.css

    # Vuelva a revisar el estado: ¿qué cambió?
    git status

    # Confirmar los cambios con un mensaje descriptivo
    git commit -m "feat: agrega página principal y hoja de estilos del currículum"

    # Vuelva a revisar el estado: ¿qué cambió?
    git status

    # Enviar los cambios al repositorio remoto
    git push origin main

    # Vuelva a revisar el estado: ¿qué cambió?
    git status

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Cuando presiona `Ctrl + S`, ¿Git registra algo? ¿Qué diferencia hay entre guardar un archivo y confirmar un cambio?
2. ¿Por qué Git utiliza un área de preparación intermedia en lugar de confirmar directamente todos los archivos modificados? Proponga un escenario donde esto sea útil.
3. ¿Por qué se recomienda hacer commits pequeños y atómicos (un cambio lógico por commit) en vez de un único commit al final del día?
4. Antes del `git push`, ¿dónde existían sus commits? ¿Qué habría pasado con ellos si el Codespace se eliminaba en ese momento?
5. ¿Qué convenciones de nomenclatura existen para los mensajes de commit? ¿Qué significa el mensaje de commit que escribió? 

¿Cómo comparto mi sitio web con el mundo sin pagar un servidor?
---------------------------------------------------------------

1. En el repositorio, vaya a *Settings → Pages*.
2. En **Build and deployment**, seleccione:
    
   * **Source:** Deploy from a branch
   * **Branch:** main
   * **Folder:** / (root)
   * Haga clic en **Save**.

3. Abra la pestaña **Actions** del repositorio y observe el flujo *pages build and deployment*.
4. Cuando termine (ícono verde), regrese a *Settings → Pages* y abra la URL publicada `https://<usuario>.github.io/dawm-cv/`

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué tipo de sitios puede alojar GitHub Pages? ¿Podría publicar allí una aplicación con base de datos o código de servidor (por ejemplo, Node.js o PHP)? Justifique su respuesta.
2. Describa el camino completo que recorrió la publicación de la página: desde que editó el archivo hasta que apareció en Internet. ¿Qué papel cumplió la pestaña Actions? Relacione este proceso con el concepto de despliegue continuo (CD).
3. ¿Qué información nunca debería subir a un repositorio que se publica con GitHub Pages? ¿Qué implica que el repositorio sea público?
4. Investigue cómo usar la carpeta `/docs` o un dominio personalizado con GitHub Pages. ¿En qué situaciones convendría cada opción?

Referencias
============

* Git. (s. f.). Git reference documentation. Recuperado el 11 de octubre de 2026, de https://git-scm.com/docs
* GitHub. (s. f.-a). Acerca de GitHub Pages. GitHub Docs. Recuperado el 11 de octubre de 2026, de https://docs.github.com/es/pages/getting-started-with-github-pages/about-github-pages
* GitHub. (s. f.-c). Documentación de GitHub Codespaces. GitHub Docs. Recuperado el 11 de octubre de 2026, de https://docs.github.com/es/codespaces
* Commits Convencionales. (n.d.). Commits Convencionales. https://www.conventionalcommits.org/es/v1.0.0-beta.2/.