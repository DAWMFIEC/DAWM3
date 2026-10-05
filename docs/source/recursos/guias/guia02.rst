..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 02: Estructura de un sitio web
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Comprender la estructura de un sitio web mediante la ejecución, observación y análisis de elementos HTML que componen un documento.

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
   - Dentro del `<head>`, agregue `<meta charset=\"UTF-8\">`, guarde el archivo y recargue la página. ¿Observa algún cambio?
   - Investigue qué significa *charset* y cómo afecta la visualización de la página web en diferentes dispositivos.
3. Utilice el DevTools y active el modo de diseño adaptable (*Responsive Design Mode*). ¿Qué observa al cambiar el tamaño de la ventana del navegador?
   - Dentro del `<head>`, agregue `<meta name="viewport" content="width=device-width, initial-scale=1.0">`. Guarde el archivo y recargue la página. ¿Observa algún cambio? 
   - Investigue qué significa *viewport* y cómo afecta la visualización de la página web en diferentes dispositivos.
4. ¿Para qué sirve la etiqueta `<meta name="description">`? ¿Por qué podría ser importante describir correctamente el contenido de una página web?
5. ¿Qué diferencia identifica entre <head> y <body>?
6. ¿Dónde aparece el texto "Ana Pérez" y dónde aparece el texto "Ana Pérez | Currículum vitae"? 
7. Si el usuario no la ve como parte de la página, ¿para quién o para qué podría resultar útil esta información? ¿Qué diferencia encuentra entre un **metadato** y el **contenido visible**?
8. Después de realizar los experimentos anteriores, marque los siguientes elementos según su función:

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
   * - `<html lang=\"es\">`
     -
     -
     -
   * - `<head>`
     -
     -
     -
   * - `<meta charset=\"UTF-8\">`
     -
     -
     -
   * - `<meta name=\"viewport\">`
     -
     -
     -
   * - `<meta name=\"description\">`
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

¿Cómo le decimos al navegador qué significa cada parte del documento HTML?
--------------------------------------------------------------------------

1. Dentro de `<body>`, reemplace el `<h1>` por:

.. code-block:: html
    :emphasize-lines: 2-69

    <body>
        <header class="cabecera">
        <div>
            <h1>Ana Pérez</h1>
            <p class="cargo">Estudiante de Computación · Desarrolladora web junior</p>
        </div>
        <nav aria-label="Secciones del CV">
            <ul>
            <li><a href="#perfil">Perfil</a></li>
            <li><a href="#experiencia">Experiencia</a></li>
            <li><a href="#educacion">Educación</a></li>
            <li><a href="#habilidades">Habilidades</a></li>
            <li><a href="#proyectos">Proyectos</a></li>
            <li><a href="#contacto">Contacto</a></li>
            </ul>
        </nav>
        </header>

        <main>
        <section id="perfil">
            <h2>Perfil</h2>
            <p>Estudiante de <strong>Ingeniería en Computación</strong> interesada en el desarrollo de
            aplicaciones web accesibles. Me gusta <em>aprender haciendo</em> y trabajar en equipo.</p>
        </section>

        <section id="experiencia">
            <h2>Experiencia</h2>
            <article class="item">
            <h3>Ayudante de laboratorio</h3>
            <p class="meta">ESPOL · <time datetime="2025-05">mayo 2025</time> – actualidad</p>
            <ul>
                <li>Apoyo en prácticas de programación para 40 estudiantes.</li>
                <li>Preparación de guías de laboratorio en HTML.</li>
            </ul>
            </article>
        </section>

        <section id="educacion">
            <h2>Educación</h2>
            <article class="item">
            <h3>Ingeniería en Computación</h3>
            <p class="meta">ESPOL · <time datetime="2023">2023</time> – en curso</p>
            </article>
        </section>

        <section id="habilidades">
            <h2>Habilidades</h2>
            <ul class="etiquetas">
            <li>HTML</li>
            <li>CSS</li>
            <li>Git y GitHub</li>
            <li>Python</li>
            <li>Trabajo en equipo</li>
            </ul>
        </section>

        <section id="proyectos">
            <h2>Proyectos</h2>
        </section>

        <section id="contacto">
            <h2>Contacto</h2>
        </section>
        </main>

        <footer>
        <p>&copy; <time datetime="2026">2026</time> Ana Pérez ·
            <a href="https://github.com/usuario">GitHub</a></p>
        </footer>
    </body>

2. Guarde el archivo y recargue la página en el navegador. 

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué tipo de página web parece representar el contenido? ¿Qué información permite identificar rápidamente a Ana Pérez?
2. Si tuviera que dividir la página en **inicio**, **contenido principal** y **cierre**, ¿qué información colocaría en cada parte?
3. ¿Qué grandes bloques de información puede reconocer visualmente?
4. ¿Qué contenido se encuentra dentro de `<header>`, `<main>` y `<footer>`?
5. Reemplace la etiqueta `<header> por `<div>` y recargue la página. ¿Qué diferencia observa en la visualización de la página? ¿Qué diferencia encuentra entre `<header>` y `<div>`?
6. Si visualmente el resultado pudiera ser similar, ¿por qué cree que existen estas etiquetas?
7. ¿Qué ventaja podría tener esta estructura para una persona que posteriormente necesite modificar la página?
8. ¿Qué característica tienen en común los contenidos agrupados dentro de cada `<section>`? ¿Qué elemento se utiliza como título de cada sección?
9. ¿Para qué cree que sirven valores como `id="perfil"` o `id="experiencia"`?
10. En DevTools, modifique temporalmente `<h2>Perfil</h2>` por `<h1>Sobre mi</h1>`
    
    ¿El cambio realizado desde DevTools modifica permanentemente el archivo HTML? ¿Qué sucede al recargar la página?

11. Haga clic en el enlace **Experiencia**. ¿Qué sucede? ¿Qué relación encuentra entre `href=\"#experiencia\"` e `id=\"experiencia\"`?
12. ¿Qué tipo de contenido se encuentra dentro de `<nav>`?
13. Si eliminamos `aria-label=\"Secciones del CV\"`, ¿observamos algún cambio visual inmediato? 
    
    - Si no produce un cambio visual, ¿significa que el atributo aria-label no tiene utilidad?

14. ¿Por qué Ayudante de laboratorio utiliza `<h3>` y no `<h2>`? ¿Qué relación jerárquica existe entre *h1*, *h2* y *h3*?
15. Si Ana tuviera tres experiencias laborales, ¿qué elemento repetiría: `<section>` o `<article>`?
16. ¿Qué representa el `<section id="experiencia">` completo y qué representa el `<article class=\"item\">` dentro de esa sección?
17. ¿Qué efecto visual observa al utilizar `<strong>` y `<em>`? ¿Qué información ve el usuario en el elemento `<time>`?
18. Compare estos los enlaces `<a href=\"#contacto\">Contacto</a>` y `<a href=\"https://github.com/usuario\">GitHub</a>`. ¿Cuál permite navegar dentro del mismo documento y cuál dirige hacia un recurso externo?

¿Qué ocurre cuando agrego imágenes y videos a la página web?
-------------------------------------------------------------

1. Modifique la sección de **Proyectos** con el siguiente código:

.. code-block:: html
    :emphasize-lines: 2-13

    <section id="proyectos">
        <h2>Proyectos</h2>
        <figure>
            <img src="https://placehold.co/640x360" alt="Captura de la aplicación de tareas" width="640" height="360">
            <figcaption>Aplicación de tareas con HTML, CSS y una API REST.</figcaption>
        </figure>
        <figure>
            <video controls width="640" poster="https://placehold.co/640x360">
            <source src="https://placeholdervideo.dev/640x360" type="video/mp4">
            Tu navegador no puede reproducir video. <a href="media/presentacion.mp4">Descárgalo aquí</a>.
            </video>
            <figcaption>Video de presentación (1 minuto).</figcaption>
        </figure>
    </section>

2. Guarde el archivo y recargue la página en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué elementos están contenidos dentro de `<figure>` y qué relación existe entre la imagen y `<figcaption>`?
2. En la etiqueta `<img>`, ¿qué cree que significa el atributo **src** y qué función parece cumplir **alt**?
3. Utilice DevTools → Network, recargue la página y localice la solicitud correspondiente a la imagen.

   - ¿Qué información le proporciona DevTools sobre la solicitud de la imagen?
   - ¿Qué diferencia encuentra entre la solicitud de la imagen y la solicitud del video?

4. En la etiqueta `<video>`, ¿qué cree que significa el atributo **controls** y qué función parece cumplir **poster**?

Referencias
============

* HTML: lenguaje de marcado de hipertexto | MDN. (2026, 30 septiembre). https://developer.mozilla.org/es/docs/Web/HTML
* Especificación HTML | WHATWG. (2026, 5 octubre). https://html.spec.whatwg.org/
* Placehold | A simple, fast and free image placeholder service. (2024). Placehold.Co. https://placehold.co/
* Gianito. (2026, May 23). Placeholder Video Generator. Placeholder Video Generator. https://placeholdervideo.dev