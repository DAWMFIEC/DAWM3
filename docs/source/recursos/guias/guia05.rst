..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

=============================================================================================
Guía 05: Flexbox versus Grid
=============================================================================================

.. topic:: Objetivo específico
    :class: objetivo

    Construir un mismo diseño utilizando tanto Flexbox como Grid, comparando sus ventajas y desventajas en diferentes contextos.

Actividades en clases
=====================

Sitio original y diseño objetivo
--------------------------------

**EcoRuta** es un sitio *ficticio* de turismo sostenible que solo tiene colores, tipografía y espaciado. **No tiene ninguna regla de distribución de contenidos**, como se muestra en la siguiente imagen. 


.. dropdown:: Página original de EcoRuta sin reglas de distribución
   :color: primary
   
   .. figure:: ../img/ecoruta_base.png
      :alt: Página original de EcoRuta sin reglas de distribución
      :align: center

A partir de esa página, deberás construir **dos versiones** sin modificar el HTML ni la hoja de estilos `base.css`. La meta es que **ambas versiones se vean exactamente iguales** al diseño objetivo. 

.. dropdown:: Diseño objetivo en escritorio, ambas versiones deben producir exactamente la misma imagen.
   :color: primary
   
   .. figure:: ../img/ecoruta_objetivo_escritorio.png
      :alt: Diseño objetivo de EcoRuta en escritorio
      :align: center

¿Qué hace el navegador cuando no le damos instrucciones de distribución?
------------------------------------------------------------------------

1. Haz un fork del `repositorio EcoRuta <https://github.com/aavendan/ecoruta>`_ en tu cuenta de GitHub.
2. Abre un Codespace en tu fork y explora el entorno de trabajo.
3. Crea una rama llamada `flexbox-grid` para trabajar.
4. Levanta el servidor de desarrollo con `python -m http.server 8000`.
5. Abra las páginas `flexbox.html` y `grid.html` en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Con DevTools abierto (`F12` → *Elements*), seleccione el elemento `<article>`. ¿Por qué los elementos `<article>` aparecen uno debajo de otro?
2. La hoja `base.css` no contiene ninguna regla de ubicación. ¿Qué ventaja tiene separar los estilos **visuales** de los estilos de **distribución** en archivos distintos?

.. admonition:: 

   Durante toda la clase:

    - Abra `flexbox.html` y `grid.html` en **dos ventanas lado a lado** en el navegador. 
    - Edite las hojas `css/flexbox.css` y `css/grid.css` en el editor de código.

¿Cómo alineamos y centramos elementos en uno o en dos ejes?
-----------------------------------------------------------

Cabecera, menú y *hero*
^^^^^^^^^^^^^^^^^^^^^^^

1. Agregue las siguientes reglas a las hojas de estilo correspondientes

.. tab-set::

    .. tab-item:: Hoja de estilo `css/flexbox.css`

        .. code-block:: css

            /* Actividad 1: cabecera y hero */

            .cabecera {
                display: flex;                   
                justify-content: space-between;  
                align-items: center;             
            }
            .menu ul { display: flex; gap: 1.5rem; }

            .hero {
                display: flex;
                justify-content: center;   
                align-items: center;       
            }
    
    .. tab-item:: Hoja de estilo `css/grid.css`

        .. code-block:: css

            /* Actividad 1: cabecera y hero */

            .cabecera {
                display: grid;
                grid-template-columns: auto auto;  
                justify-content: space-between;
                align-items: center;
            }
            .menu ul { display: grid; grid-auto-flow: column; gap: 1.5rem; }

            .hero {
                display: grid;
                place-items: center;
            }

2. Compare ambas ventanas: la cabecera debe verse igual.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. En `flexbox.html`, ¿Qué propiedad centra verticalmente `justify-content` o `align-items`?
2. Para la cabecera, Flexbox necesitó solo `display: flex`, mientras que Grid necesitó **declarar las columnas** (`grid-template-columns: auto auto`). ¿Qué dice esto sobre quién decide el tamaño: el **contenido** (Flexbox) o el **contenedor** (Grid)?

¿Cómo logramos tarjetas de igual ancho e igual altura?
------------------------------------------------------

1. Agregue las siguientes reglas a las hojas de estilo correspondientes

.. tab-set::

    .. tab-item:: Hoja de estilo `css/flexbox.css`

        .. code-block:: css

           /* Actividad 2: tarjetas */
           
           .tarjetas { display: flex; gap: 1.5rem; }
           .tarjeta { flex: 1; }   /* las tres tarjetas reparten el espacio por igual */

           .tarjeta {
                flex: 1;
                display: flex;
                flex-direction: column;   /* h3, p y enlace se apilan en vertical */
           }
           .enlace { margin-top: auto; }   /* el margen automático absorbe el espacio libre */
    
    .. tab-item:: Hoja de estilo `css/grid.css`

        .. code-block:: css

           /* Actividad 2: tarjetas */
           .tarjetas {
                display: grid;
                grid-template-columns: repeat(3, 1fr);   /* tres columnas de igual fracción */
                gap: 1.5rem;
           }

           .tarjeta {
                display: grid;
                grid-template-rows: auto 1fr auto;   /* título | párrafo flexible | enlace */
           }

¿Cómo describimos la estructura general de la página?
-----------------------------------------------------

¿Qué ocurre cuando los elementos deben ocupar varias filas?
-----------------------------------------------------------

¿Cómo se adapta cada versión a una pantalla pequeña?
----------------------------------------------------

¿Cuándo Flexbox, cuándo Grid?
-----------------------------

Referencias
============

* Andrew, R. (2021). *Learn CSS: Flexbox*. web.dev. https://web.dev/learn/css/flexbox
* Andrew, R. (2021). *Learn CSS: Grid*. web.dev. https://web.dev/learn/css/grid
* Coyier, C. (2013). *A complete guide to Flexbox*. CSS-Tricks. https://css-tricks.com/snippets/css/a-guide-to-flexbox/
* House, C. (2016). *A complete guide to CSS Grid*. CSS-Tricks. https://css-tricks.com/snippets/css/complete-guide-grid/
* MDN Web Docs. (s. f.-a). *Basic concepts of flexbox*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox
* MDN Web Docs. (s. f.-b). *Basic concepts of grid layout*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout
* Park, T. (s. f.). *Flexbox Froggy* y *Grid Garden* [Juegos educativos]. Codepip. https://codepip.com/
