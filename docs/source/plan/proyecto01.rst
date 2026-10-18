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

.. dropdown:: Fase 2: Planificación y Diseño
   :color: info

   En esta fase, define cómo se organizará la información antes de construir la versión final del perfil. El objetivo es tomar decisiones sobre estructura, jerarquía y contenido.

   **Estructura del perfil.**

   Diseña un esquema del ``README.md`` indicando el orden de las secciones, por ejemplo:

   * Presentación.
   * Sobre mí.
   * Áreas de interés.
   * Habilidades y tecnologías.
   * Proyectos destacados.
   * Actividad o experiencia.
   * Contacto.

   No es obligatorio utilizar todas estas secciones. Selecciona únicamente aquellas que aporten información relevante.

   **Estrategia de commits.**

   Define cómo registrarás los cambios. Evita concentrar todo el trabajo en un único commit. Cada cambio importante debe quedar registrado mediante un mensaje descriptivo ("Commits Convencionales", n.d.), por ejemplo:

   * ``docs: agrega presentación personal e incorpora sección de habilidades``
   * ``fix: corrige errores de ortografía en README.md``
   * ``feat: agrega proyecto destacado con enlace a demo``

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

   **Evidencia:** Genera un esquema o boceto de la estructura del perfil y la propuesta de contenido de cada sección y proyecto destacado.

.. dropdown:: Fase 3: Implementación
   :color: warning

   En esta fase, convierte el diseño en un perfil funcional utilizando Markdown y las características disponibles en GitHub.

   **Repositorio principal**

   Crea un **repositorio especial** (Freecodecamp, 2021) con tu nombre de usuario de GitHub, por ejemplo, ``github.com/usuario``. Este repositorio contendrá únicamente el archivo ``README.md`` que se mostrará en tu perfil.

   **Construcción incremental.**

   Implementa el perfil sección por sección. Después de completar una modificación significativa:

   * Revisa el resultado.
   * Guarda los cambios.
   * Realiza un commit descriptivo.
   * Verifica el cambio directamente en GitHub antes de continuar.

   **Historial del desarrollo.**

   Mantén commits pequeños y relacionados con cambios concretos. El historial debe permitir reconstruir cómo evolucionó el perfil desde su primera versión hasta la versión actual.

   **Evidencia:** Perfil implementado, con historial de commits que muestre el desarrollo incremental.


.. dropdown:: Fase 5: Verificación
   :disabled: True
   :color: info

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
   :disabled: True
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

* Abhisheknaiidu. (n.d.). Abhisheknaiidu/awesome-github-profile-readme: 😎 A curated list of awesome GitHub Profile which updates in real time. GitHub. Retrieved September 28, 2026 from https://github.com/abhisheknaiidu/awesome-github-profile-readme
* Freecodecamp. (2021). How to Use the .github Repository. FreeCodeCamp.org. https://www.freecodecamp.org/news/how-to-use-the-dot-github-repository/.
* Commits Convencionales. (n.d.). Commits Convencionales. https://www.conventionalcommits.org/es/v1.0.0-beta.2/.