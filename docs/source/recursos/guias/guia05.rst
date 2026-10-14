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
5. Abra la página `index.html` en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Con DevTools abierto (`F12` → *Elements*), seleccione el elemento `<article>`. ¿Por qué los elementos `<article>` aparecen uno debajo de otro?
2. La hoja `base.css` no contiene ninguna regla de ubicación. ¿Qué ventaja tiene separar los estilos **visuales** de los estilos de **distribución** en archivos distintos?

.. attention:: Durante toda la clase
    
    - Abra `flexbox.html` y `grid.html` en **dos ventanas lado a lado** en el navegador. 
    - Modifique **ambas hojas de estilo** `css/flexbox.css` y `css/grid.css` en el editor de código.

¿Cómo alineamos y centramos elementos en uno o en dos ejes?
-----------------------------------------------------------

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

1.

¿Cómo logramos tarjetas de igual ancho e igual altura?
------------------------------------------------------

1. Agregue las siguientes reglas a las hojas de estilo correspondientes

.. tab-set::

    .. tab-item:: Hoja de estilo `css/flexbox.css`

        .. code-block:: css

           /* Actividad 2: tarjetas */
           
           .tarjetas { display: flex; gap: 1.5rem; }
           
           .tarjeta {
                display: flex;
                flex: 1;
                flex-direction: column;   
           }
           .enlace { margin-top: auto; }
    
    .. tab-item:: Hoja de estilo `css/grid.css`

        .. code-block:: css

           /* Actividad 2: tarjetas */

           .tarjetas {
                display: grid;
                grid-template-columns: repeat(3, 1fr);  
                gap: 1.5rem;
           }

           .tarjeta {
                display: grid;
                grid-template-rows: auto 1fr auto;
           }

2. Compare ambas ventanas: las tarjetas deben verse igual.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. 

¿Cómo describimos la estructura general de la página?
-----------------------------------------------------

1. Agregue las siguientes reglas a las hojas de estilo correspondientes

.. tab-set::

    .. tab-item:: Hoja de estilo `css/flexbox.css`

        .. code-block:: css

            /* Actividad 3: estructura y pie */

            .contenedor {
                display: flex;
                gap: 2rem;
                align-items: flex-start;   
            }
            .principal { flex: 1; min-width: 0; }   
            .lateral { flex: 0 0 260px; }           

            .pie-columnas { display: flex; gap: 2rem; }
            .columna { flex: 1; }

    .. tab-item:: Hoja de estilo `css/grid.css`

        .. code-block:: css

            /* Actividad 3: estructura y pie */

            .contenedor {
                display: grid;
                grid-template-columns: 1fr 260px;
                grid-template-areas: "principal lateral";   
                gap: 2rem;
                align-items: start;
            }
            .principal { grid-area: principal; min-width: 0; }
            .lateral { grid-area: lateral; }

            .pie-columnas {
                display: grid;
                grid-template-columns: repeat(3, 1fr);
                gap: 2rem;
            }

2. Compare ambas ventanas: el esqueleto de dos columnas deben verse igual.

¿Qué ocurre cuando los elementos deben ocupar varias filas?
-----------------------------------------------------------

1. Agregue las siguientes reglas a las hojas de estilo correspondientes

.. tab-set::

    .. tab-item:: Hoja de estilo `css/flexbox.css`

        .. code-block:: css

            /* Actividad 4: galería */

            .galeria { display: flex; flex-wrap: wrap; gap: 1rem; }
            .galeria img {
                flex: 0 0 calc((100% - 2 * 1rem) / 3);   /* (ancho − 2 huecos) ÷ 3 columnas */
                min-width: 0;   /* evita que la imagen imponga su ancho natural (400 px) */
            }

    .. tab-item:: Hoja de estilo `css/grid.css`

        .. code-block:: css

            /* Actividad 4: galería */

            .galeria {
                display: grid;
                grid-template-columns: repeat(3, 1fr);
                gap: 1rem;
            }

2. Compare ambas ventanas: las galerías deben verse igual.

¿Cómo se adapta cada versión a una pantalla pequeña?
----------------------------------------------------

1. Agregue las siguientes reglas a las hojas de estilo correspondientes

.. tab-set::

    .. tab-item:: Hoja de estilo `css/flexbox.css`

        .. code-block:: css

            /* Actividad 5: adaptación a pantallas pequeñas */

            @media (max-width: 768px) {
                .cabecera { flex-direction: column; gap: .5rem; padding: .5rem 1rem; }
                .contenedor { flex-direction: column; align-items: stretch; }
                .lateral { flex: none; }   /* sin esto, 260px se aplicaría a la ALTURA */
                .tarjetas, .pie-columnas { flex-direction: column; gap: 1rem; }
                .galeria img { flex-basis: calc((100% - 1rem) / 2); }
            }

    .. tab-item:: Hoja de estilo `css/grid.css`

        .. code-block:: css

            /* Actividad 5: adaptación a pantallas pequeñas */

            @media (max-width: 768px) {
                .cabecera {
                    grid-template-columns: auto;
                    justify-content: center;
                    justify-items: center;
                    gap: .5rem;
                    padding: .5rem 1rem;
                }
                .contenedor {
                    grid-template-columns: 1fr;
                    grid-template-areas: "principal" "lateral";
                }
                .tarjetas, .pie-columnas { grid-template-columns: 1fr; gap: 1rem; }
                .galeria { grid-template-columns: repeat(2, 1fr); }
            }

            

2. Compare ambas ventanas: las galerías deben verse igual.

¿Cuándo Flexbox, cuándo Grid?
-----------------------------

.. admonition:: Reflexión final

    **Flexbox** trabaja de **adentro hacia afuera**: el contenido de los elementos determina cómo se reparte el espacio en **un eje**. 
    
    **Grid** trabaja de **afuera hacia adentro**: el contenedor define una estructura de **filas y columnas**, y los elementos se ubican en ella. No compiten entre sí, sino que se complementan.

Referencias
============

* Andrew, R. (2021). *Learn CSS: Flexbox*. web.dev. https://web.dev/learn/css/flexbox
* Andrew, R. (2021). *Learn CSS: Grid*. web.dev. https://web.dev/learn/css/grid
* Coyier, C. (2013). *A complete guide to Flexbox*. CSS-Tricks. https://css-tricks.com/snippets/css/a-guide-to-flexbox/
* House, C. (2016). *A complete guide to CSS Grid*. CSS-Tricks. https://css-tricks.com/snippets/css/complete-guide-grid/
* MDN Web Docs. (s. f.-a). *Basic concepts of flexbox*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox
* MDN Web Docs. (s. f.-b). *Basic concepts of grid layout*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout
* Park, T. (s. f.). *Flexbox Froggy* y *Grid Garden* [Juegos educativos]. Codepip. https://codepip.com/
