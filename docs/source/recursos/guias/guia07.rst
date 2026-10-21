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
            let cantidad = 0;

            const appDiv: HTMLElement = document.getElementById('app');

            if (appDiv) {
                appDiv.innerHTML = `
                <header>
                    <h1>🎵 ${titulo}</h1>
                    <p>${descripcion}</p>
                    <h4>${cantidad} canciones en la lista de canciones </h4>
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
   - Interfaz (Llamas, 2024) para describir una canción, con las propiedades: ``id``, ``titulo``, ``artista``, ``anio``, ``genero``, ``duracion`` y ``album`` (opcional).

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

#. Cree el archivo `src/canciones.ts`, con el código:

   - Importe (Viktorsson, 2025) la interfaz `Cancion` y el tipo literal `Genero` desde `src/tipos.ts`.
   - Una lista de canciones (Lists | TypeScript Guide by Convex, n.d.), con al menos 10 canciones, usando la interfaz `Cancion` y el tipo literal `Genero`. Recuerde que algunas canciones pueden no tener álbum.
   
   .. code-block:: typescript

      import type { Cancion } from './tipos';

      export const canciones: Cancion[] = [ ... ]
    
   .. dropdown:: Solución
       :color: success

       .. code-block:: typescript

          import type { Cancion } from './tipos';

          export const canciones: Cancion[] = [
              { id: 0,  titulo: 'Adiós',                   artista: 'Gustavo Cerati',         anio: 2006, genero: 'rock',   duracion: 233, album: 'Ahí Vamos' },
              { id: 1,  titulo: 'Bohemian Rhapsody',       artista: 'Queen',                  anio: 1975, genero: 'rock',               duracion: 355, album: 'A Night at the Opera' },
              { id: 2,  titulo: 'Billie Jean',             artista: 'Michael Jackson',        anio: 1982, genero: 'pop',                duracion: 294, album: 'Thriller' },
              { id: 3,  titulo: 'Despacito',               artista: 'Luis Fonsi',             anio: 2017, genero: 'reggaeton',          duracion: 229 },
              { id: 4,  titulo: 'Pedro Navaja',            artista: 'Rubén Blades',           anio: 1978, genero: 'salsa',              duracion: 441, album: 'Siembra' },
              { id: 5,  titulo: 'Nuestro juramento',       artista: 'Julio Jaramillo',        anio: 1957, genero: 'bolero',             duracion: 174 },
              { id: 6,  titulo: 'Hotel California',        artista: 'Eagles',                 anio: 1976, genero: 'rock',               duracion: 391, album: 'Hotel California' },
              { id: 7,  titulo: 'Vivir mi vida',           artista: 'Marc Anthony',           anio: 2013, genero: 'salsa',              duracion: 252, album: '3.0' },
              { id: 8,  titulo: "Hips Don't Lie",          artista: 'Shakira',                anio: 2006, genero: 'pop',                duracion: 218 },
              { id: 9,  titulo: 'Gasolina',                artista: 'Daddy Yankee',           anio: 2004, genero: 'reggaeton',          duracion: 192, album: 'Barrio Fino' },
              { id: 10, titulo: 'Bésame mucho',            artista: 'Consuelo Velázquez',     anio: 1940, genero: 'bolero',             duracion: 200 },
              { id: 11, titulo: 'Smells Like Teen Spirit', artista: 'Nirvana',                anio: 1991, genero: 'rock',               duracion: 301, album: 'Nevermind' },
              { id: 12, titulo: 'La bicicleta',            artista: 'Carlos Vives y Shakira', anio: 2016, genero: 'pop',                duracion: 230 },
          ]; 
  
     
Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué tipo de datos se espera para la propiedad `genero` en la interfaz `Cancion`?
#. ¿Qué significa que la propiedad `album?` en la interfaz `Cancion`?
#. ¿Qué ventajas ofrece TypeScript al definir tipos y estructuras de datos en comparación con JavaScript?
#. ¿Cómo ayuda la definición de tipos de datos en el desarrollo de software?
#. ¿Cuál es la utilidad de la palabra clave `export` en TypeScript y cómo se relaciona con la modularidad del código?

¿Cómo muestro la lista de canciones en la página?
-------------------------------------------------

#. Modifique el archivo `src/main.ts` para mostrar la lista de canciones en la página web:
   
   - Importe la interfaz `Cancion` desde `src/tipos.ts` y la lista de canciones desde `src/canciones.ts`
   - Asigne el valor de la longitud del arreglo de canciones a la variable `cantidad`.
   - Implementa una función llamada `mostrarCanciones` que reciba como parámetro un arreglo de objetos de tipo Cancion. Mapee los atributos de cada canción a un bloque de HTML y los agregue al contenedor `#lista-canciones`.
   - Llame a la función `mostrarCanciones` pasando la lista de canciones como argumento.

   .. code-block:: typescript
      :emphasize-lines: 1,2,4,10-19, 21
      
      import { ... } from './canciones';
      import type { ... } from './tipos';

      let cantidad = ...;

      if (appDiv) {
        
        appDiv.innerHTML = ... ; 

        let mostrarCanciones = (canciones: Cancion[]): void => {
          const listaDiv = document.getElementById( ... );
          if (listaDiv) {
            listaDiv.innerHTML = canciones
              .map(
                (cancion) => `...`
              )
              .join('');
          }
        };

        mostrarCanciones(canciones);
      }

   .. dropdown:: Solución
       :color: success

       .. code-block:: typescript
          :emphasize-lines: 1,2,5,11-27, 29

          import { canciones } from './canciones';
          import type { Cancion } from './tipos';

          ...
          let cantidad = canciones.length;

          if (appDiv) {
            
            appDiv.innerHTML = ` ... `; 

            let mostrarCanciones = (canciones: Cancion[]): void => {
              const listaDiv = document.getElementById('lista-canciones');
              if (listaDiv) {
                listaDiv.innerHTML = canciones
                  .map(
                    (cancion) => `
                          <article class="cancion">
                              <h3>${cancion.titulo}</h3>
                              <p class="artista">Artista: ${cancion.artista}</p>
                              <p class="anio">Año: ${cancion.anio}</p>
                              <p class="genero">Género: ${cancion.genero}</p>
                              <p class="duracion">Duración: ${cancion.duracion} segundos</p>
                          </article>`
                  )
                  .join('');
              }
            };

            mostrarCanciones(canciones);
          }

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué sucede si la lista de canciones está vacía? ¿Cómo podrías manejar ese caso en la función `mostrarCanciones`?
#. ¿Cómo podrías mejorar la presentación de la lista de canciones, por ejemplo, agregando estilos CSS o mostrando un mensaje cuando no se encuentren resultados?
#. 

¿Cómo encuentro una canción escribiendo parte de su nombre?
-------------------------------------------------------------

#.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. 

¿Cómo describo una canción para que TypeScript la entienda?
-------------------------------------------------------------

#.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. 

Referencias
===========

* Chacón, J. L. (2021). TypeScript: Qué es, diferencias con JavaScript y por qué aprenderlo. Profile Software Services. Retrieved October 21, 2026 from https://profile.es/blog/que-es-typescript-vs-javascript/
* TypeScript: JavaScript With Syntax for Types. (n.d.). Retrieved October 21, 2026 from https://www.typescriptlang.org/
* Geeksforgeeks. (2022). What are string literal types in TypeScript ? - GeeksforGeeks. GeeksforGeeks. https://www.geeksforgeeks.org/typescript/what-are-string-literal-types-in-tpescript/.
* Llamas, L. (2024). What are Interfaces and how to use them in TypeScript. Luis Llamas. https://www.luisllamas.es/en/typescript-interfaces/.
* Viktorsson, R. (2025, January 10). TypeScript and ES Modules: Best Practices for Imports and Exports | by Robin Viktorsson | Medium. Retrieved October 21, 2026 from https://medium.com/@robinviktorsson/typescript-and-es-modules-best-practices-for-imports-and-exports-9ce200e75a88
* Lists | TypeScript Guide by Convex. (n.d.). Retrieved October 21, 2026 from https://www.convex.dev/typescript/core-concepts/arrays-collections/typescript-list