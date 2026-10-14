..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

=============================================================================================
Guía 05: Un diseño, dos caminos de implementación para la misma página con Flexbox y con Grid
=============================================================================================

.. topic:: Objetivo específico
    :class: objetivo

    Construir un mismo diseño utilizando tanto Flexbox como Grid, comparando sus ventajas y desventajas en diferentes contextos.

Actividades en clases
=====================

Sitio original y diseño objetivo
--------------------------------

**EcoRuta** es un sitio ficticio de turismo sostenible que solo tiene colores, tipografía y espaciado. **No tiene ninguna regla de distribución de contenidos**, así que el navegador apila todos los elementos uno debajo de otro, en el flujo normal, como se muestra a continuación:


.. collapsible:: Página original de EcoRuta sin reglas de distribución; todos los elementos aparecen apilados
   :class: dropdown
   
    .. figure:: ../img/ecoruta_inicial.png
        :alt: Página original de EcoRuta sin reglas de distribución; todos los elementos aparecen apilados
        :align: center


A partir de esa página, deberás construir **dos versiones** sin modificar el HTML ni la hoja `base.css`:

- `flexbox.html` + `css/flexbox.css`, que solo usa **Flexbox**.
- `grid.html` + `css/grid.css`, que solo usa **Grid**.

La meta es que **ambas versiones se vean exactamente iguales** al diseño objetivo. En cada actividad se resuelve la misma zona de la página primero con Flexbox y luego con Grid, y se comparan los resultados.


.. collapsible:: Diseño objetivo en escritorio (1280 px). Ambas versiones, Flexbox y Grid, deben producir exactamente esta imagen.
   :class: dropdown
   
    .. figure:: ../img/ecoruta_objetivo.png
        :alt: Diseño objetivo de EcoRuta en escritorio, con cabecera, hero centrado, tres tarjetas, barra lateral, galería de 3 columnas y pie de 3 columnas
        :align: center


Referencias
============

* Andrew, R. (2017). *The new CSS layout*. A Book Apart.
* Andrew, R. (2021). *Learn CSS: Flexbox*. web.dev. https://web.dev/learn/css/flexbox
* Andrew, R. (2021). *Learn CSS: Grid*. web.dev. https://web.dev/learn/css/grid
* Coyier, C. (2013). *A complete guide to Flexbox*. CSS-Tricks. https://css-tricks.com/snippets/css/a-guide-to-flexbox/
* House, C. (2016). *A complete guide to CSS Grid*. CSS-Tricks. https://css-tricks.com/snippets/css/complete-guide-grid/
* MDN Web Docs. (s. f.-a). *Basic concepts of flexbox*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox
* MDN Web Docs. (s. f.-b). *Basic concepts of grid layout*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout
* Park, T. (s. f.). *Flexbox Froggy* y *Grid Garden* [Juegos educativos]. Codepip. https://codepip.com/
