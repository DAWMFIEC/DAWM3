..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================
Guía 07: TubeTrack 
==================

.. topic:: Objetivo específico
   :class: objetivo
    
   Desarrollar una aplicación con una lista de reproducción de canciones para la comprensión de los fundamentos Typescript y su aplicación en una solución interactiva.
 
Actividades en clases
====================

¿Cómo creo y ejecuto un proyecto TypeScript en StackBlitz?
-------------------------------------------------------------

#. Ingrese a `Stackblitz <https://stackblitz.com>`_ e inicie sesión con su cuenta de `GitHub <https://github.com>`_.
#. Cree un proyecto nuevo desde `New project` → `vanilla-ts`, o también puede abrir directamente `Vanilla Ts <https://vite.new/vanilla-ts>`_.
#. Pulse **Fork** (o **Save**) para guardar el proyecto en su cuenta y cámbiele el nombre a *tubetrack*.


Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Investigue la utilidad de los archivos en la tabla:

   .. list-table:: Archivos del proyecto
      :header-rows: 1
      :widths: 25 75

      * - Archivo
        - ¿Para qué sirve?
      * - ``index.html``
        -
      * - ``src/main.ts``
        -
      * - ``src/style.css``
        -
      * - ``package.json``
        -
      * - ``tsconfig.json``
        -

¿Cómo personalizo la presentación del proyecto?
-------------------------------------------------------------

#. Abra `tsconfig.json` y busque `"strict": true` dentro de `compilerOptions`. Si no aparece, agréguelo.
#. Elimine `src/counter.ts` y `assets` del proyecto. 
#. Reeemplace todo el contenido de `src/style.css` por:

   .. dropdown:: style.css
      :color: success

      .. code-block:: css

          /* ==============================
            Estilos generales
          ============================== */

          * {
            box-sizing: border-box;
          }

          body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            color: #222;
          }

          /* ==============================
            Cabecera
          ============================== */

          header {
            background-color: #222;
            color: white;
            text-align: center;
            padding: 30px;
          }

          header h1 {
            margin-bottom: 8px;
          }

          header p {
            margin: 0;
            color: #cccccc;
          }

          /* ==============================
            Contenido principal
          ============================== */

          main {
            width: 90%;
            max-width: 800px;
            margin: 30px auto;
          }

          /* ==============================
            Buscador
          ============================== */

          .buscador {
            margin-bottom: 30px;
          }

          .buscador label {
            display: block;
            font-weight: bold;
            margin-bottom: 8px;
          }

          #buscar {
            width: 100%;
            padding: 12px;

            border: 1px solid #cccccc;
            border-radius: 8px;

            font-size: 16px;
          }

          #buscar:focus {
            outline: 2px solid #555555;
          }

          /* ==============================
            Lista de canciones
          ============================== */

          #lista-canciones {
            display: grid;
            gap: 15px;
          }

          .cancion {
            background-color: white;

            padding: 18px;

            border-radius: 10px;

            box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
          }

          .cancion h3 {
            margin-top: 0;
            margin-bottom: 8px;
          }

          .artista {
            font-weight: bold;
            margin: 5px 0;
          }

          .genero {
            color: #666666;
            margin: 5px 0;
          }

          /* ==============================
            Mensaje sin resultados
          ============================== */

          #sin-resultados {
            display: none;

            text-align: center;
            padding: 20px;

            color: #666666;
          }

#. Reemplace todo el contenido de `src/main.ts` por:

   .. dropdown:: main.ts
      :color: success 

      .. code-block:: typescript

            import './style.css';
            
            const titulo = 'TubeTrack - TypeScript';
            const descripcion = 'Explora y busca tus canciones favoritas';

            const appDiv: HTMLElement = document.getElementById('app');

            if (appDiv) {
                appDiv.innerHTML = `
                <header>
                    <h1>🎵 ${titulo}</h1>
                    <p>${descripcion}</p>
                    <h4> canciones en la lista de canciones </h4>
                </header>
                <main>
                    <section class="buscador">
                        <label for="buscar">Buscar canción</label>

                        <input
                        type="text"
                        id="buscar"
                        placeholder="Escribe una canción, artista o género..."
                        />
                    </section>

                    <section>
                        <h2>Lista de canciones</h2>

                        <div id="lista-canciones"></div>
                    </section>
                </main>`;
            }

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Cuál es la función de la propiedad *"strict": true* en el archivo `tsconfig.json`?
#. ¿Por qué aparece la advertencia *"Object is possibly 'null'"* en `const appDiv: HTMLElement = document.getElementById('app')`? 

   **Nota**: Corrija la advertencia agregando una verificación con 
   `const appDiv: HTMLElement = document.getElementById('app') as HTMLElement;`

#. ¿Qué relación hay entre TypeScript y JavaScript? (TypeScript: JavaScript With Syntax For Types, n.d.)
#. ¿Qué significa que TypeScript sea un *superconjunto* de JavaScript? (Chacón, 2021)

¿Cómo describo una canción para que TypeScript la entienda?
-------------------------------------------------------------

#. Antes de escribir código, qué datos tiene una canción y de qué tipo es cada uno:

   .. list-table:: Propiedades de una canción
      :header-rows: 1
      :widths: 15 40 15 30

      * - Propiedad
        - Tipo
        - ¿Obligatoria? (Sí/No)
        - Ejemplo
      * - ``id``
        - 
        - 
        - 
      * - ``titulo``
        - 
        - 
        - 
      * - ``artista``
        - 
        - 
        - 
      * - ``anio``
        - 
        - 
        - 
      * - ``genero``
        - 
        - 
        - `
      * - ``duracion``
        - 
        - 
        - 
      * - ``album``
        - 
        - 
        - 

    .. dropdown:: Tipos de datos en TypeScript
       :color: success

       .. list-table:: Propiedades de una canción
          :header-rows: 1
          :widths: 15 40 15 30

          * - Propiedad
            - Tipo
            - ¿Obligatoria? (Sí/No)
            - Ejemplo
          * - ``id``
            - ``number`` (no se puede cambiar)
            - Sí
            - ``1``
          * - ``titulo``
            - ``string``
            - Sí
            - ``'Bohemian Rhapsody'``
          * - ``artista``
            - ``string``
            - Sí
            - ``'Queen'``
          * - ``anio``
            - ``number``
            - Sí
            - ``1975``
          * - ``genero``
            - Uno de: ``rock``, ``pop``, ``salsa``, ``reggaeton``, ``bolero``
            - Sí
            - ``'rock'``
          * - ``duracion``
            - ``number`` (en segundos)
            - Sí
            - ``355``
          * - ``album``
            - ``string``
            - **No**
            - ``'A Night at the Opera'``

#. Cree el archivo `src/tipos.ts`, con el código:

   - Tipo literal de cadena para género (Geeksforgeeks, 2022), con las opciones: ``rock``, ``pop``, ``salsa``, ``reggaeton``, ``bolero``.
   - Interfaz para describir una canción, con las propiedades: ``id``, ``titulo``, ``artista``, ``anio``, ``genero``, ``duracion`` y ``album`` (opcional).

   .. code-block:: typescript

      export type Genero = ... ;

      export interface Cancion { ... }
    
   .. dropdown:: Solución
       :color: success

       .. code-block:: typescript

          export type Genero = 'rock' | 'pop' | 'salsa' | 'reggaeton' | 'bolero';

          export interface Cancion {
              id: number;
              titulo: string;
              artista: string;
              anio: number;
              genero: Genero;
              duracion: number; // en segundos
              album?: string; // propiedad opcional
          }
     
Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué tipo de datos se espera para la propiedad `genero` en la interfaz `Cancion`?

Referencias
===========

* Chacón, J. L. (2021). TypeScript: Qué es, diferencias con JavaScript y por qué aprenderlo. Profile Software Services. Retrieved October 21, 2026 from https://profile.es/blog/que-es-typescript-vs-javascript/
* TypeScript: JavaScript With Syntax for Types. (n.d.). Retrieved October 21, 2026 from https://www.typescriptlang.org/
* Geeksforgeeks. (2022). What are string literal types in TypeScript ? - GeeksforGeeks. GeeksforGeeks. https://www.geeksforgeeks.org/typescript/what-are-string-literal-types-in-tpescript/.