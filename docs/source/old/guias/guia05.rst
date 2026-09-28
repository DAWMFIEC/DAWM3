..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

========================================================
Guía 05: CSS - Diseño
========================================================

.. topic:: Objetivo específico
    :class: objetivo

    Utlizar reglas CSS en la organización visual del contenido mediante el desarrollo de un currículum vitae digital para la presentación de información profesional de manera organizada y accesible en línea.

Actividades previas
=====================

Ambiente de desarrollo
----------------------

1. Acceda a su proyecto *curriculum* en Codespaces o en su máquina local.
2. Cree y utilice la(s) rama(s) de desarrollo.
3. Haga clic en el ícono de `Live Preview` en la barra de estado de VSCode para iniciar el servidor local.

Actividades en clases
=====================

Flex
----

.. sidebar:: 

    Con :term:`flex`, o flexbox, los elementos dentro de un contenedor pueden crecer, encogerse y alinearse de manera flexible.

    .. image:: https://img.uxcel.com/practices/flexbox-1665382286109/a-1665382286109-2x.jpg


1. Utilice un cliente de IAG para generar las reglas CSS en el documento *stylesheets/style.css*:

   a) Con el selector para la etiqueta hija `<ul>` dentro de la etiqueta `<nav>`, utilice la propiedad `display` con el valor `flex` para activar el modelo de diseño flexbox.
   
   b) Para el selector anterior, distribuya el espacio equitativamente entre los elementos de la lista.
   
   c) Para el selector anterior, establece que los elementos hijos de un contenedor flexible se organicen en una fila horizontal, de izquierda a derecha, siguiendo la dirección principal del eje horizontal.

   d) Para el selector anterior, establece que los elementos hijos de un contenedor flexible se alineen verticalmente en el centro del eje transversal, ubicándolos equidistantes respecto a los bordes superior e inferior del contenedor.

   e) Para el selector anterior, establece un espacio uniforme de 1 :term:`unidad relativa al tamaño de la fuente raíz` (1rem) entre cada par de elementos hijos.

   .. dropdown:: Ver la solución
    :color: success

    .. code-block:: text
        :emphasize-lines: 3-20

        section { ... }
        
        nav > ul {

            /* Activa el modelo de diseño Flexbox*/
            display: flex; 

            /* Distribuye los elementos con espacio igual entre ellos */
            justify-content: space-between;

            /* Organiza los elementos en una fila horizontal de izquierda a derecha */ 
            flex-direction: row;

            /* Centra verticalmente los elementos dentro del contenedor */
            align-items: center; 

            /* Establece un espacio uniforme de 1rem entre cada elemento */
            gap: 1rem; 

        }

2. Compruebe la vista previa del resultado en el navegador.

   .. note::

        Revisa los conceptos básicos de flexbox en `Basic concepts of flexbox <https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox>`_ y la guía de CSS flexbox en `CSS Flexbox Layout Guide <https://css-tricks.com/snippets/css/a-guide-to-flexbox/>`_.

Grid
----

.. sidebar:: 

    Con :term:`grid` los elementos dentro de un contenedor pueden alinearse en filas y columnas, lo que permite crear diseños más complejos y estructurados.

    .. image:: https://tutorial.techaltum.com/images/css-grids.jpg

1. Compruebe en el archivo *index.html*:

   a) Todos los controles del formulario deben estar al mismo nivel, es decir, no deben estar anidados dentro de etiquetas `<div>` o `<span>`. 

   .. code-block:: html
       :caption: Controles del formulario sin etiquetas contenedoras.
       :linenos:

       <form>
            <label> ... </label>
            <input ... >
            
            <label> ... </label>
            <select ... > ... </select>
            
            <fieldset> ... <fieldset>
            
            <button> ... </button>
       </form>

2. Utilice un cliente de IAG para generar las reglas CSS en el documento *stylesheets/style.css*:

   a) Con el selector para la etiqueta hija `<form>` dentro de la etiqueta `<section>`, utilice la propiedad `display` con el valor `grid` para activar el modelo de diseño grid.

   b) Para el selector anterior, define una estructura de una sola columna en un contenedor con diseño de cuadrícula (grid), asignándole una fracción proporcional del espacio disponible, de modo que ocupe todo el ancho del área del contenedor.
   
   c) Para el selector anterior, establece un espacio uniforme de 1 unidad relativa al tamaño de la fuente raíz (1rem) entre cada par de elementos hijos por fila y por columna, por separado.

   .. dropdown:: Ver la solución
    :color: success

    .. code-block:: text
        :emphasize-lines: 3-17

        nav > ul { ... }
        
        section > form {
            
            /* Activa el modelo de diseño basado en cuadrícula */
            display: grid;

            /* Define una sola columna que ocupa todo el ancho disponible del contenedor */
            grid-template-columns: 1fr;

            /* Establece un espacio vertical uniforme de 1rem entre filas */
            row-gap: 1rem;

            /* Establece un espacio horizontal uniforme de 1rem entre columnas */
            column-gap: 1rem;

        }


3. Compruebe la vista previa del resultado en el navegador.

   .. note::

        Revisa los conceptos básicos de grid en `Basic concepts of grid layout <https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout>`_ y la guía de CSS grid en `CSS Grid Layout Guide <https://css-tricks.com/snippets/css/complete-guide-grid/>`_.

Flex vs Grid
-------------------------------------

1. Compruebe en el archivo *index.html*:

   a) En la sección de referencias, organice los elementos de la siguiente manera:

      (i) El subtítulo es una etiqueta `<h2>`.
      (ii) Las tres artículos son etiquetas `<article>`, y se encuentran al mismo nivel que el subtítulo.

   .. code-block:: html
       :caption: Estructura de la sección referencias.
       :linenos:

       <section>
           <h2>Referencias</h2>
           <article> ... </article>
           <article> ... </article>
           <article> ... </article>
       </section>
        
2. Utilice un cliente de IAG para generar las reglas CSS en el documento *stylesheets/style.css*:

   a) Para organizar la sección referencias para Grid y Flex, independientemente. El subtitulo ocupa todo el ancho y los tres artículos ocupen el espacio equitativamente. El espacio de separación entre filas y columnas es 1rem. No agregue otras etiquetas, ni atributos. 


   .. dropdown:: Ver la solución
    :color: success

    .. tab-set::

        .. tab-item:: Flex

           .. code-block:: text
               :emphasize-lines: 3-14,16-21,23-28

               section > form { ... }

               #referencias {

                    /* Activa el modelo Flexbox */
                    display: flex;

                    /* Permite que los hijos pasen a la siguiente línea si no caben */
                    flex-wrap: wrap;

                    /* Espaciado uniforme entre filas y columnas */
                    gap: 1rem;
            
               }

               #referencias > h2 {
            
                    /* Hace que el subtítulo ocupe el 100% del ancho del contenedor */
                    flex: 0 0 100%;
                
               }

               #referencias > article {
                
                    /* Distribuye equitativamente los artículos en tres columnas */
                    flex: 1 1 calc(33.333% - 1rem);
                
               }            

        .. tab-item:: Grid
        
           .. code-block:: text
               :emphasize-lines: 3-14,16-21

               section > form { ... }

               #referencias {

                    /* Activa el modelo Grid */
                    display: grid;

                    /* Define tres columnas iguales */
                    grid-template-columns: repeat(3, 1fr);

                    /* Espacio entre filas y columnas */
                    row-gap: 1rem;
                    column-gap: 1rem;

               }
                    
               #referencias > h2 {

                    /* El subtítulo ocupa las tres columnas */
                    grid-column: 1 / -1;

               }
        


3. Compruebe la vista previa del resultado en el navegador.

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *curriculum*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.
3. Compruebe el resultado en el navegador.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué diferencias conceptuales encontraste entre el uso de Flexbox y Grid para organizar visualmente el contenido del currículum vitae?

    * ¿Qué modificaciones realizaste sobre el código generado por la IA al momento de implementar el diseño con Flexbox o Grid?

    * ¿Cómo puedes garantizar que el uso de inteligencia artificial en la implementación de Flexbox o Grid no reemplace tu capacidad de toma de decisiones como futuro desarrollador web?


Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    Flexbox vs Grid mediante juegos interactivos.

    <blockquote class="twitter-tweet"><p lang="es" dir="ltr">Descubre <a href="https://twitter.com/hashtag/CSSGrid?src=hash&amp;ref_src=twsrc%5Etfw">#CSSGrid</a> y <a href="https://twitter.com/hashtag/Flexbox?src=hash&amp;ref_src=twsrc%5Etfw">#Flexbox</a> de manera divertida con Grid Garden y Flexbox Froggy. 🎮🌐 Aprende jugando . 💻🚀 <br>Jardín Grid: <a href="https://t.co/SLubvps9gb">https://t.co/SLubvps9gb</a><br>Flexbox Froggy: <a href="https://t.co/e17lQydbXT">https://t.co/e17lQydbXT</a><br>¡CSS nunca fue tan divertido! 🌈✨<a href="https://twitter.com/hashtag/WebDev?src=hash&amp;ref_src=twsrc%5Etfw">#WebDev</a> <a href="https://twitter.com/hashtag/CodingFun?src=hash&amp;ref_src=twsrc%5Etfw">#CodingFun</a> <a href="https://t.co/OPd5eAouGd">pic.twitter.com/OPd5eAouGd</a></p>&mdash; Cristian Omar Guzman (@cristiank170319) <a href="https://twitter.com/cristiank170319/status/1710508125567000742?ref_src=twsrc%5Etfw">October 7, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>