..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================
Plan de desarrollo
==================

En grupos de dos integrantes trabajará de manera colaborativa durante todas las fases del proyecto, desde el análisis, planificación y diseño hasta la implementación y presentación del dashboard.

.. dropdown:: Fase 1: Análisis
   :color: info

   En esta fase, se ha identificado la necesidad de desarrollar las habilidades de programación con JavaScript y TypeScript, así como la comprensión de conceptos clave relacionados con el desarrollo de dashboards interactivos. Se ha investigado sobre las tecnologías web modernas y se ha analizado la importancia de presentar información de manera clara y concisa.

   **JavaScript**

   Esta actividad requiere del uso de JavaScript en el desarrollo de soluciones web interactivas, especialmente en la manipulación del DOM, la gestión de eventos y la comunicación asincrónica, lo que es esencial para construir un dashboard interactivo.

   **TypeScript**

   Con esta actividad, se busca que los integrantes del grupo comprendan la importancia de utilizar TypeScript en el desarrollo de aplicaciones web, ya que proporciona tipado estático y características avanzadas que mejoran la calidad del código y facilitan el mantenimiento del proyecto.

   **Fuente de datos**

   Se ha identificado la necesidad de utilizar una fuente de datos confiable y actualizada para alimentar el dashboard. Esto implica investigar y seleccionar conjunto de datos que proporcione información relevante y precisa para el monitoreo en tiempo real, considerando que contenga variables categóricas y numéricas que permitan realizar comparaciones, agrupaciones y visualizaciones estadísticas significativas.

.. dropdown:: Fase 2: Diseño
   :color: info

   Elabore un boceto de baja fidelidad y luego un prototipo (Figma, Excalidraw o papel digitalizado) de la vista en escritorio y en móvil. Ubique los indicadores, gráficos, tabla y filtros según su importancia para el usuario.

.. dropdown:: Fase 3: Planificación
   :color: info

   Al finalizar esta fase, cada equipo contará con una arquitectura de componentes React documentada, directamente relacionada con sus prototipos, que servirá como referencia para la siguiente fase de desarrollo del dashboard.

.. dropdown:: Fase 4: Implementación
   :color: warning

   En esta fase, cada equipo implementará su dashboard utilizando React y TypeScript, siguiendo la arquitectura de componentes previamente definida. Se espera que los integrantes del grupo apliquen buenas prácticas de desarrollo, como la modularidad, la reutilización de componentes y la gestión eficiente del estado de la aplicación.

   **Sprint 1 – Transformación de un conjunto de datos CSV en JSON**

   En este sprint, cada equipo se encargará de transformar su conjunto de datos en formato CSV a JSON. Esta tarea es fundamental para preparar los datos que se utilizarán en el dashboard, asegurando que estén estructurados de manera adecuada para su posterior visualización y análisis.

   **Sprint 2 – Implementación de la interfaz mediante la creación de componentes React**

   Transformarán los elementos visuales del prototipo en componentes React independientes y reutilizables. Implementarán la estructura general del dashboard, los contenedores para gráficos, los indicadores y los controles de filtrado, utilizando inicialmente datos de ejemplo.

   Se introducirá useState mediante una interacción sencilla que permita observar cómo un cambio de estado actualiza la interfaz.

   **Sprint 3 – Conexión de React con Firebase para recuperar información**

   Configurarán Firebase Realtime Database, almacenarán los registros transformados y establecerán la conexión con su aplicación React.

   Utilizarán `useEffect` para ejecutar la consulta inicial y `useState` para conservar los registros recuperados y controlar los estados de carga y error. Finalmente, reemplazarán los datos de ejemplo de algunos componentes por información obtenida desde Firebase.

   **Sprint 4 – Registros en visualizaciones estadísticas**

   Procesarán los registros obtenidos de Firebase para calcular indicadores y construir visualizaciones relacionadas con las necesidades de información identificadas en la fase de análisis.

   Implementarán al menos tres KPI, tres gráficos y una tabla de datos, respetando las decisiones establecidas en los prototipos.

   **Sprint 5 – Filtros y actualización de visualizaciones**

   Integrarán las funcionalidades desarrolladas durante los sprints anteriores para construir un dashboard interactivo.

   Implementarán un estado compartido en el componente principal y utilizarán props para enviar información a los componentes hijos. Los filtros comunicarán los cambios mediante funciones *callback*, permitiendo que los indicadores, gráficos y tabla se actualicen de manera coordinada sin recargar la página.

Referencias
===========

* JavaScript | MDN. (n.d.). Retrieved October 19, 2026 from https://developer.mozilla.org/es/docs/Web/JavaScript
* React. (n.d.). Retrieved October 19, 2026 from https://es.react.dev/
* TypeScript: JavaScript With Syntax for Types. (n.d.). Retrieved October 19, 2026 from https://www.typescriptlang.org/