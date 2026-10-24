..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================
Plan de desarrollo
==================

El trabajo avanza por fases; cada una deja evidencia en el repositorio, de modo que el historial de commits cuente cómo se construyó el perfil.

.. dropdown:: Fase 1: Análisis
   :color: success

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

   .. **Evidencia:** Genera una tabla comparativa de perfiles, la lista de verificación de proyectos, el diagnóstico de tus repositorios y los requisitos definidos.

.. dropdown:: Fase 2: Planificación y Diseño
   :color: success

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

   .. **Evidencia:** Genera un esquema o boceto de la estructura del perfil y la propuesta de contenido de cada sección y proyecto destacado.

.. dropdown:: Fase 3: Implementación
   :color: warning

   En esta fase, convierte el diseño en un perfil funcional utilizando Markdown y las características disponibles en GitHub.

   **Repositorio principal**

   Crea un **repositorio especial** (Freecodecamp, 2021) con tu nombre de usuario de GitHub, por ejemplo, ``github.com/usuario``. Este repositorio contendrá únicamente el archivo ``README.md`` que se mostrará en tu perfil.

   **Marcado y formato.**

   Utiliza Markdown (Markdown Guide, n.d.) para dar formato al contenido. Emplea títulos, listas, tablas, enlaces e imágenes según lo definido en la fase de diseño. Evita el uso de HTML y CSS, ya que no son compatibles con el perfil de GitHub.

   .. code-block:: markdown

      # Título
      ## Subtítulo

      **Negrita** y *cursiva*.

      - Lista
      1. Lista numerada

      ### Subsubtítulo

      En la siguiente tabla se muestran las características destacadas:

      | Columna 1 | Columna 2 |
      |-----------|-----------|

      En este [Enlace](https://www.ejemplo.com) es posible incluir referencias externas.

   
   **Construcción incremental.**

   Implementa el perfil sección por sección. Después de completar una modificación significativa:

   * Revisa el resultado.
   * Guarda los cambios.
   * Realiza un commit descriptivo.
   * Verifica el cambio directamente en GitHub antes de continuar.

   **Historial del desarrollo.**

   Mantén commits pequeños y relacionados con cambios concretos. El historial debe permitir reconstruir cómo evolucionó el perfil desde su primera versión hasta la versión actual.

   .. **Evidencia:** Perfil implementado, con historial de commits que muestre el desarrollo incremental.

.. dropdown:: Fase 4: Publicación
   :color: warning

   En esta fase, el perfil debe estar completo y listo para ser compartido. Verifica que toda la información sea correcta, que los enlaces funcionen y que el formato sea legible en diferentes dispositivos.

   **GitHub Pages.** 
   
   Verifica que el repositorio principal esté correctamente configurado para mostrar el ``README.md`` en tu perfil. Asegúrate de que la visibilidad del repositorio sea pública.

   .. **Evidencia:** URL pública del perfil, URL de los repositorios destacados, SHA del commit correspondiente a la versión final y una breve reflexión sobre el proceso de desarrollo.



Referencias
===========

* Abhisheknaiidu. (n.d.). Abhisheknaiidu/awesome-github-profile-readme: 😎 A curated list of awesome GitHub Profile which updates in real time. GitHub. Retrieved September 28, 2026 from https://github.com/abhisheknaiidu/awesome-github-profile-readme
* Freecodecamp. (2021). How to Use the .github Repository. FreeCodeCamp.org. https://www.freecodecamp.org/news/how-to-use-the-dot-github-repository/.
* Commits Convencionales. (n.d.). Commits Convencionales. https://www.conventionalcommits.org/es/v1.0.0-beta.2/.
* Markdown Guide. (n.d.). Retrieved October 19, 2026 from https://www.markdownguide.org/