..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================
Plan de desarrollo
==================

El trabajo avanza por fases; cada una deja evidencia en el repositorio, de modo que el historial de commits cuente cómo se construyó el perfil.

.. dropdown:: Fase 1: Análisis
   :color: info

   En esta fase, observa antes de construir. Estudia cómo otros desarrolladores presentan su identidad en GitHub y qué características tiene un proyecto bien versionado, para decidir con criterio qué incluir en tu propio perfil y cómo documentar los proyectos que vas a destacar.

   **Perfiles de referencia.** 

   Elige al menos tres perfiles de GitHub, por ejemplo, de la lista Abhisheknaiidu (n.d.) y analiza cada perfil con los mismos criterios. Registra en una tabla comparativa:

   * Presentación: qué se entiende del desarrollador en los primeros 10 segundos.
   * Estructura: secciones, orden y jerarquía visual.
   * Habilidades: cómo se muestran (texto, insignias, iconos) y si son verificables.
   * Proyectos: cuáles se fijan y cómo se describen.
   * Contacto: canales disponibles y su visibilidad.
   * Accesibilidad y lectura en móvil: texto alternativo, contraste, dependencia de imágenes externas.

   Elige dos o tres repositorios fijados en los perfiles analizados y verifica qué características tienen:

   * Descripción corta, temas (topics) y enlace a demo o sitio.
   * README con propósito, tecnologías, instrucciones de instalación y uso, y capturas.
   * Licencia y archivo .gitignore.
   * Historial de commits: frecuencia y calidad de los mensajes.
   * Uso de Issues, Pull Requests, ramas y Releases.
   * Estructura de carpetas ordenada.

   **Requisitos de tu perfil.** 

   Con estos resultados, redacta los requisitos funcionales (qué debe mostrar el perfil) y no funcionales (legibilidad, accesibilidad, mantenibilidad) que guiarán el diseño.

   Aplica la misma lista de verificación a tus repositorios que piensas destacar e identifica qué les falta.

   **Evidencia:** Genera una tabla comparativa de perfiles, la lista de verificación de proyectos, el diagnóstico de tus repositorios y los requisitos definidos.

.. dropdown:: Fase 2: Planificación
   :color: primary

   En esta fase, transforma los requisitos identificados en un conjunto organizado de tareas. El objetivo es definir qué vas a construir, en qué orden y cómo registrarás el avance en GitHub.

   **Organización del trabajo.**

   Divide el desarrollo del perfil en tareas pequeñas y verificables. Por ejemplo:

   * Crear el repositorio del perfil.
   * Preparar la estructura inicial del ``README.md``.
   * Redactar una presentación personal breve.
   * Incorporar formación, intereses y áreas de trabajo.
   * Seleccionar las habilidades técnicas que se mostrarán.
   * Seleccionar los proyectos que se destacarán.
   * Mejorar la documentación de los repositorios seleccionados.
   * Incorporar información de contacto.
   * Revisar accesibilidad y visualización en diferentes dispositivos.

   Organiza las tareas mediante Issues o un tablero de proyecto. Para cada tarea, define una descripción breve y un criterio que permita determinar cuándo está terminada.

   **Estrategia de commits.**

   Define cómo registrarás los cambios. Evita concentrar todo el trabajo en un único commit. Cada cambio importante debe quedar registrado mediante un mensaje descriptivo.

   Ejemplos:

   * ``docs: agrega presentación personal``
   * ``docs: incorpora sección de habilidades``
   * ``docs: agrega proyectos destacados``
   * ``docs: mejora información de contacto``
   * ``docs: corrige accesibilidad de imágenes``

   **Evidencia:** Genera la lista de tareas, su organización y el plan inicial de commits que utilizarás durante el desarrollo.


.. dropdown:: Fase 3: Diseño
   :color: warning

   En esta fase, define cómo se organizará la información antes de construir la versión final del perfil. El objetivo es tomar decisiones sobre estructura, jerarquía y contenido.

   **Estructura del perfil.**

   Diseña un esquema del ``README.md`` indicando el orden de las secciones. Por ejemplo:

   * Presentación.
   * Sobre mí.
   * Áreas de interés.
   * Habilidades y tecnologías.
   * Proyectos destacados.
   * Actividad o experiencia.
   * Contacto.

   No es obligatorio utilizar todas estas secciones. Selecciona únicamente aquellas que aporten información relevante.

   **Diseño del contenido.**

   Para cada sección, define:

   * Qué información incluirás.
   * Qué información evitarás.
   * Si utilizarás texto, tablas, listas, imágenes o insignias.
   * Qué enlaces externos serán necesarios.
   * Cómo facilitarás la lectura rápida de la información.

   Diseña además una ficha para cada proyecto que quieras destacar. Como mínimo, considera:

   * Nombre.
   * Problema o necesidad que resuelve.
   * Tecnologías utilizadas.
   * Funcionalidades principales.
   * Enlace al repositorio.
   * Enlace a demostración, cuando exista.

   **Accesibilidad.**

   Revisa desde el diseño aspectos como texto alternativo de imágenes, contraste, claridad de los enlaces y dependencia de recursos externos.

   **Evidencia:** Genera un esquema o boceto de la estructura del perfil y la propuesta de contenido de cada sección y proyecto destacado.


.. dropdown:: Fase 4: Implementación
   :color: success

   En esta fase, convierte el diseño en un perfil funcional utilizando Markdown y las características disponibles en GitHub.

   **Construcción incremental.**

   Implementa el perfil sección por sección. Después de completar una modificación significativa:

   * Revisa el resultado.
   * Guarda los cambios.
   * Realiza un commit descriptivo.
   * Verifica el cambio directamente en GitHub antes de continuar.

   **Proyectos destacados.**

   Actualiza también los repositorios que seleccionaste durante el análisis. Cuando corresponda:

   * Mejora la descripción del repositorio.
   * Agrega topics.
   * Organiza el ``README.md``.
   * Documenta instalación y uso.
   * Agrega capturas o ejemplos.
   * Incluye ``.gitignore``.
   * Verifica la licencia.
   * Organiza los archivos y carpetas.

   No agregues tecnologías, experiencia o proyectos que no puedas justificar mediante evidencia.

   **Historial del desarrollo.**

   Mantén commits pequeños y relacionados con cambios concretos. El historial debe permitir reconstruir cómo evolucionó el perfil desde su primera versión hasta la versión actual.

   **Evidencia:** Perfil implementado, repositorios seleccionados actualizados e historial de commits que muestre el desarrollo incremental.


.. dropdown:: Fase 5: Verificación
   :color: danger

   En esta fase, comprueba que el perfil cumple los requisitos definidos durante el análisis y que puede ser comprendido por una persona que lo visita por primera vez.

   **Revisión funcional.**

   Verifica:

   * Que todas las secciones previstas estén presentes.
   * Que los enlaces funcionen.
   * Que los proyectos destacados correspondan con los repositorios enlazados.
   * Que las imágenes e insignias se carguen correctamente.
   * Que la información de contacto sea correcta.

   **Revisión de calidad.**

   Comprueba además:

   * Ortografía y redacción.
   * Consistencia entre títulos y secciones.
   * Claridad de las descripciones.
   * Legibilidad.
   * Uso adecuado del texto alternativo.
   * Visualización en escritorio y dispositivo móvil.
   * Ausencia de información personal que no quieras publicar.

   **Validación entre pares.**

   Solicita a un compañero que observe el perfil durante aproximadamente 10 segundos y responda:

   * ¿Quién es este desarrollador?
   * ¿Qué sabe hacer?
   * ¿Qué proyecto llama primero tu atención?
   * ¿Dónde buscarías evidencia de sus habilidades?
   * ¿Qué información resulta difícil de encontrar o comprender?

   Registra las observaciones recibidas y realiza las correcciones necesarias mediante nuevos commits.

   **Evidencia:** Lista de verificación completada, resultados de la revisión entre pares, problemas identificados y commits de corrección.


.. dropdown:: Fase 6: Publicación y mejora
   :color: secondary

   En esta fase, prepara una versión estable del perfil y reflexiona sobre el proceso de desarrollo.

   **Versión final.**

   Verifica nuevamente el perfil y los repositorios destacados. Asegúrate de que el estado final disponible en GitHub corresponda con la versión que deseas presentar.

   Selecciona el commit que representa la versión final del trabajo y registra su identificador SHA.

   **Evaluación del proceso.**

   Revisa el historial de commits y analiza cómo evolucionó el producto:

   * ¿Qué cambió entre la primera y la última versión?
   * ¿Qué decisiones del análisis inicial se mantuvieron?
   * ¿Qué decisiones tuvieron que modificarse?
   * ¿Qué observaciones de la verificación produjeron cambios?
   * ¿Qué mejorarías si desarrollaras nuevamente el perfil?

   **Mantenimiento.**

   El perfil de GitHub no debe considerarse un producto terminado. Define qué información necesitará actualizarse cuando desarrolles nuevos proyectos, aprendas nuevas tecnologías o cambien tus intereses profesionales.

   **Evidencia:** URL pública del perfil, URL de los repositorios destacados, SHA del commit correspondiente a la versión final y una breve reflexión sobre el proceso de desarrollo.



Referencias
===========

- Abhisheknaiidu. (n.d.). Abhisheknaiidu/awesome-github-profile-readme: 😎 A curated list of awesome GitHub Profile which updates in real time. GitHub. Retrieved September 28, 2026 from https://github.com/abhisheknaiidu/awesome-github-profile-readme