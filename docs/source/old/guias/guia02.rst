..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 02: HTML - Estructura global y estructura semántica
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Justificar el uso de etiquetas HTML en la estructura general y de etiquetas semánticas mediante el desarrollo de un currículum vitae digital para la presentación de información profesional de manera organizada y accesible en línea.

Actividades previas
=====================

Diseño
------

1. Decide el contenido de su CV. Puedes considerar la recomendación de diseño para :download:`diseñar el contenido de un CV <./pdfs/guia02-disenocv.pdf>`.
2. Elije la estructura del contenido de su CV, p.e.: el orden de las secciones, título de cada sección, uso de listas numeradas o listas no numeradas de elementos, etc.

Ambiente de desarrollo
----------------------

1. Cree un repositorio en GitHub con el nombre *curriculum*.

   a) Agregue un archivo README.md con el título de su curriculum y una breve descripción del objetivo de su proyecto.
   b) Seleccione la opción *No .gitignore*.
   
2. Acceda al código de su proyecto *curriculum*:

   a) En el repositorio de GitHub: Haga clic en el botón **Code**, escoja la pestaña :term:`Codespaces` y haga clic en el botón `Create codespace on main`, o
   b) Clone su proyecto en su máquina local.

3. :material-round:`note_alt;1.5em;sd-text-success` Revise la explicación en `What is Github Codespaces <https://4geeks.com/lesson/what-is-github-codespaces>`_, o consulte con su cliente de IAG, acerca de la definición y uso de GitHub Codespaces.

Extensión: Live Preview en VSCode
---------------------------------

1. Acceda a la opción **Extensión** en la barra lateral izquierda (o presione Ctrl+Shift+X).
2. Busque e instale la extensión `Live Preview` en el Marketplace de VSCode.


Actividades en clases
=====================

HTML
----

1. Cree el documento :term:`HTML` *index.html* en el :term:`directorio raíz` de tu proyecto.
2. :material-round:`note_alt;1.5em;sd-text-success` Revise los `Conceptos básicos de HTML <https://developer.mozilla.org/es/docs/Learn_web_development/Getting_started/Your_first_website/Creating_the_content>`_ en MDN Web Docs, o consulte con su cliente de IAG, acerca de la definición de HTML. 

Servidor local y público
------------------------

1. Habilite la opción `Show Preview` 
   
   a) En `View` > `Command Palette`, busque y seleccione `Live Preview: Show Preview (Internal Browser)`, o
   b) Haga clic en el icono de `Show Preview` junto al nombre del documento.

2. Haga clic en la opción `Convertir en Público` y revise la :term:`URL` en una nueva pestaña del navegador.
3. :material-round:`note_alt;1.5em;sd-text-success` Revise el sitio `¿Qué es un servidor WEB? <https://developer.mozilla.org/es/docs/Learn_web_development/Howto/Web_mechanics/What_is_a_web_server>`_ en MDN Web Docs, o consulte con su cliente de IAG, acerca de la definición y uso de un servidor local y público.

Etiquetas HTML
--------------

.. sidebar:: Estándar HTML

    Puede consultar el :term:`estándar` de HTML en el sitio oficial `HTML Living Standard <https://html.spec.whatwg.org/multipage/>`_.

Estructura General
^^^^^^^^^^^^^^^^^^

1. Utilice la guía para :download:`crear una página HTML con la estructura general <./pdfs/guia02-estructurageneral.pdf>` para agregar la :term:`estructura general` de su página. 

   **Nota:** El sitio debe contener, como mínimo, la etiqueta `<title>` y las etiquetas `<meta>` (para keywords, description y author).

2. Revise los cambios en el navegador con la extensión `Live Preview` de VSCode.
3. :material-round:`note_alt;1.5em;sd-text-success` Revise el sitio `Estructura de un documento HTML <https://lenguajehtml.com/html/documento/estructura-documento-html/>`_ en Manz.dev, o consulte con su cliente de IAG, acerca de la definición y uso de una estructura general HTML válida.

Estructura Semántica
^^^^^^^^^^^^^^^^^^^^

.. sidebar:: Validación
     
     Puede validar el soporte de las etiquetas HTML en el navegador, con `HTML5 Test <https://html5test.co/>`_ o con `Can I Use <https://caniuse.com/>`_; además, puede validar el anidamiento de etiquetas con `Can I Include <https://caninclude.glitch.me/>`_.

1. Utilice la guía para :download:`crear una página HTML con la estructura semántica <./pdfs/guia02-estructurasemántica.pdf>` :term:`estructura semántica` a su página. 

   **Nota:** El sitio debe contener, como mínimo, 1 etiqueta `<header>`, 1 etiqueta `<main>`, 5 etiquetas `<section>`, 1 etiqueta `<nav>` y 1 etiqueta `<footer>`. El documento HTML debe contener un título (`<h1>`) con su nombre. Cada sección debe contener un subtítulo (`<h2>`).
   
2. Revise los cambios en el navegador con la extensión `Live Preview` de VSCode. 
3. :material-round:`note_alt;1.5em;sd-text-success` Revise el sitio `Etiquetas semánticas de sección <https://lenguajehtml.com/html/semantica/que-son/>`_ en Manz.dev, o consulte con su cliente de IAG, acerca de la definición y uso de una estructura semántica adecuada.

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *curriculum*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Despliegue con GitHub Pages
---------------------------

1. Siga las instrucciones de la guía :download:`publicar la rama main con GitHub Pages <./pdfs/guia02-maingithubpages.pdf>` para desplegar el sitio del repositorio *curriculum* con GitHub Pages.
2. Compruebe el resultado en el navegador.

Conclusiones
============

.. topic:: Preguntas de cierre

   * ¿Cómo te ayudó la inteligencia artificial generativa a identificar y comprender las diferencias entre una estructura general HTML válida y una estructura semántica adecuada al momento de diseñar tu currículum vitae?
  
   * ¿Cómo verificaste que el uso de etiquetas semánticas sugeridas por la IA realmente aporta claridad y organización al contenido de tu currículum, más allá de solo cumplir una estructura técnica?
  
   * ¿De qué manera el uso de IA para generar la estructura de tu currículum influye en tu responsabilidad como desarrollador en formación, especialmente en lo relacionado con la honestidad y la autoría del código?
  

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Semantic HTML elements play a crucial role in improving website SEO and its accessibility.<br><br>Replacing non-semantic elements makes code more readable and maintainable.<br><br>HTML Semantic Elements:<br>→ Carry inherent meanings;<br>→ Make web content more Structured;<br>→ More Meaningful.… <a href="https://t.co/O18NI5L8XD">pic.twitter.com/O18NI5L8XD</a></p>&mdash; Deepanshu Sharma (@deepanshusharmx) <a href="https://twitter.com/deepanshusharmx/status/1708118904391053714?ref_src=twsrc%5Etfw">September 30, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

