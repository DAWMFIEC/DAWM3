..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================
Guía 06: TubeTrack 
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
#. Pulse **Fork** (o **Save**) para guardar el proyecto en su cuenta y cámbiele el nombre a *tube-track*.


Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Investigue la utilidad de los archivos en la tabla:

   .. list-table:: Archivos del proyecto
      :header-rows: 1
      :widths: 25 75

      * - Archivo
        - Para qué sirve
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

¿ST?
-------------------------------------------------------------

#. Elimine `src/counter.ts` y `assets` del proyecto. 
.. #. Abra `tsconfig.json` y busque `"strict": true` dentro de `compilerOptions`. Si no aparece, agréguelo.
#. Reemplace todo el contenido de `main.ts` por:

.. code-block:: typescript

    import { Song } from "./song";
    import { Playlist } from "./playlist";

    const playlist = new Playlist();

    const song1 = new Song("Song 1", "Artist 1", 180);
    const song2 = new Song("Song 2", "Artist 2", 200);
    const song3 = new Song("Song 3", "Artist 3", 220);

    playlist.addSong(song1);
    playlist.addSong(song2);
    playlist.addSong(song3);

    console.log("Playlist:");
    playlist.getSongs().forEach((song) => {
        console.log(`${song.title} by ${song.artist} (${song.duration} seconds)`);
    });

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Cuál es la función de la propiedad `"strict": true` en el archivo `tsconfig.json`?

¿ST?
-------------------------------------------------------------

#. 

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#.

Referencias
===========

*