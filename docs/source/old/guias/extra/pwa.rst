..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==========================================
React - Progressive Web App (PWA)
==========================================

.. topic:: Objetivo específico
    :class: objetivo

    Implementar funcionalidades propias de una Progressive Web App (PWA) con el fin de mejorar la accesibilidad, rendimiento y disponibilidad de la aplicación, incluso sin conexión a internet, fomentando así el desarrollo de interfaces modernas y resilientes. 

Actividades previas
=====================

Ambiente de desarrollo
----------------------

1. Acceda a su proyecto *dashboard* en Codespaces o en su máquina local.
2. Cree y utilice la(s) rama(s) de desarrollo.
3. Instale los paquetes y levante el servidor, con:

   .. code-block:: bash

      npm install
      npm run dev

Actividades en clases
=====================

Producción en local
-------------------

1. Asegúrese de que su proyecto esté funcionando correctamente en el entorno de desarrollo.
2. Compile para producción y levante el servidor local, con:

   .. code-block:: bash

      npm run build && npm run preview

3. Verifique que la compilación se haya realizado correctamente, revisando la carpeta ``dist`` que se genera en el directorio raíz del proyecto.
4. Abra su navegador y acceda a la URL de su aplicación

PWA Vite Plugin
---------------

1. Instale el plugin de `PWA Vite Plugin <https://vite-pwa-org.netlify.app/>`_, con:

   .. code-block:: bash

      npm install -D vite-plugin-pwa

2. Configure el plugin en el archivo ``vite.config.ts``, con:

   a) Asegúrese que la base de la URL sea **base: \"/dashboard/\"**.
   b) Importe y configure el plugin.

   .. code-block:: javascript
      :emphasize-lines: 2, 5, 8-13

      ...
      import { VitePWA } from 'vite-plugin-pwa'

      export default defineConfig({
         base: "/dashboard/",
         plugins: [
            react(),
            VitePWA({
               registerType: 'autoUpdate',
               devOptions: {
                  enabled: true
               }
            })
         ]
      }

3. Compile para producción y levante el servidor local, con:

   .. code-block:: bash

      npm run build && npm run preview

4. Verifique que la carpeta  ``dist`` contenga los archivos necesarios para la PWA, como ``manifest.webmanifest`` y ``sw.js``.
5. Abra su navegador y acceda a la URL de su aplicación para verificar que el service worker se ha registrado correctamente y que el manifest está disponible.
6. Utilice su cliente de IAG para explicar el concepto de :term:`PWA`, el uso de los archivos ``manifest.webmanifest`` y ``sw.js``; además, el beneficio en utilizar el `PWA Vite Plugin`.

Manifesto de la PWA
-------------------

1. Utilice el servicio `Favicon InBrowser.App <https://favicon.inbrowser.app/tools/favicon-generator>`_ para generar los íconos de la PWA.
2. Descargue y descomprima los archivos generados; excepto el archivo **site.webmanifest**. Guárdelos en la carpeta ``public`` de su proyecto.
3. Reemplace el contenido de la etiqueta `<head>` en el archivo ``index.html`` de su proyecto, con:

   .. code-block:: html
      :emphasize-lines: 4-12

      <!DOCTYPE html>
      <html lang="es">
      
      <head>
         <meta name="viewport" content="width=device-width,initial-scale=1.0">
         <title>Dashboard del Clima</title>
         <meta name="description" content="Dashboard del Clima con PWA">
         <link rel="icon" href="/favicon-32x32.png" type="image/png" sizes="32x32">
         <link rel="icon" href="/favicon-16x16.png" type="image/png" sizes="16x16">
         <link rel="apple-touch-icon" href="/apple-touch-icon.png">
         <meta name="theme-color" content="#D3D1D1">
      </head>
      
      <body>
         ...
      </body>
      
      </html>

4. Modifique la definición del manifest de la PWA en el archivo ``vite.config.ts``, con:

   .. code-block:: javascript
      :emphasize-lines: 6-37

      ...

      VitePWA({
         registerType: ...,
         devOptions: { ... },
         includeAssets: ['favicon.ico', 'apple-touch-icon.png'],
         manifest: {
            id: '/dashboard/',
            name: 'Dashboard del Clima - Proyecto 04',
            short_name: 'Dashboard del Clima',
            description: 'Proyecto 04 - dashboard del clima desarrollado con React y MUI',
            theme_color: '#D3D1D1',
            icons: [
               {
                  src: 'pwa-192x192.png',
                  sizes: '192x192',
                  type: 'image/png',
               },
               {
                  src: 'pwa-512x512.png',
                  sizes: '512x512',
                  type: 'image/png',
               },
               {
                  src: "pwa-maskable-192x192.png",
                  sizes: "192x192",
                  type: "image/png",
                  purpose: "maskable"
               },
               {
                  src: "pwa-maskable-512x512.png",
                  sizes: "512x512",
                  type: "image/png",
                  purpose: "maskable" 
               },
            ]
         }
      })

4. Compile para producción y levante el servidor local, con:

   .. code-block:: bash

      npm run build && npm run preview

5. Con Chrome, 
   
   a) Inspeccione el sitio web para verificar que se ha registrado el service worker y que el manifest está correctamente configurado.
   b) Instale la PWA en su dispositivo móvil o en su navegador, y verifique que se muestre el ícono de la aplicación y que funcione correctamente.

   .. note:: 

      Si no se muestra la opción de instalar la PWA, elimine el directorio ``dist`` y vuelva a compilar el proyecto.

6. Con su cliente de IAG, explique el propósito del :term:`manifest` de la PWA, la importancia de los íconos y cómo estos contribuyen a la experiencia del usuario al instalar la aplicación.

Service workers y Almacenamiento en caché
-----------------------------------------

1. Con Chrome, inspeccione el sitio web en la opción "Red" (Network) para deshabilitar la conexión a internet ``Sin conexión``.
2. Modifique la definición del manifest de la PWA en el archivo ``vite.config.ts``, con:

   .. code-block:: javascript
      :emphasize-lines: 8-26

      ...

      VitePWA({
         registerType: ... ,
         devOptions: { ... },
         includeAssets: [ ... ],
         manifest: { ... }, 
         workbox: {
            runtimeCaching: [
               {
                  // Intercepta todas las peticiones a esta API (ajusta según necesidad)
                  urlPattern: /^https:\/\/api\.open-meteo\.com\/.*$/,
                  handler: 'NetworkFirst',
                  options: {
                     cacheName: 'open-meteo-cache',
                     expiration: {
                        maxEntries: 10,
                        maxAgeSeconds: 60 * 60 * 24, // 1 día
                     },
                     cacheableResponse: {
                        statuses: [0, 200],
                     }
                  }
               }
            ]
         }
      })

3. Compile para producción y levante el servidor local, con:

   .. code-block:: bash

      npm run build && npm run preview

4. Con Chrome:
   
   a) Habilite la conexión a internet y recargue la página.
   b) Realice las peticiones para todas las ciudades que desee almacenar en caché.
   c) En la pestaña **Almacenamiento**, en las opciones **IndexedDB** y **Almacenamiento en caché**, verifique que las peticiones a la API de Open Meteo se almacenan en caché.
   d) Deshabilite la conexión a internet y recargue la página.
   e) Verifique que la aplicación funciona correctamente y que los datos se cargan desde el caché manejado por el ServiceWorker.

5. Con su cliente de IAG, explique el concepto de :term:`service worker`, el uso del `Workbox` para manejar el almacenamiento en caché y cómo esto mejora la experiencia del usuario al permitir que la aplicación funcione sin conexión a internet.

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *dashboard*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Despliegue
----------

1. Desde la línea de comandos, ejecute el comando de transpilación y despliegue del sitio web, con:

   .. code-block:: bash

      npm run deploy

   a) De ser necesario, elimine, corrija o comente las secciones de código identificadas por el transpilador.
   b) Vuelva a ejecutar el comando de transpilación y despliegue del sitio web.

2. Compruebe el resultado en el navegador, con la URL: `https://<username>.github.io/dashboard`
3. Verifique que la aplicación sea instalable como una PWA y que funcione correctamente sin conexión a internet.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué conceptos clave sobre PWA lograste comprender mejor gracias a la inteligencia artificial generativa, y cómo se relacionan con la capacidad del dashboard para funcionar sin conexión y ser instalable?

    * ¿Qué ajustes realizaste a la configuración sugerida por la IA para garantizar que tu dashboard cumpla con los criterios de una PWA (instalabilidad, funcionamiento offline, respuesta rápida)?

    * ¿Cómo demuestras que el uso de la inteligencia artificial no reemplazó tu responsabilidad en la creación de una aplicación sólida, sino que fue una herramienta para desarrollar tus competencias como desarrollador de PWAs?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="en" dir="ltr">Many people don’t even know that a lot of the popular everyday use apps are actually Progressive Web Apps.<a href="https://twitter.com/hashtag/ProgressiveWebApp?src=hash&amp;ref_src=twsrc%5Etfw">#ProgressiveWebApp</a> <a href="https://twitter.com/hashtag/PWA?src=hash&amp;ref_src=twsrc%5Etfw">#PWA</a> <a href="https://twitter.com/hashtag/AppDevelopment?src=hash&amp;ref_src=twsrc%5Etfw">#AppDevelopment</a> <a href="https://twitter.com/hashtag/WebDevelopment?src=hash&amp;ref_src=twsrc%5Etfw">#WebDevelopment</a> <a href="https://twitter.com/hashtag/WebAppDevelopment?src=hash&amp;ref_src=twsrc%5Etfw">#WebAppDevelopment</a> <a href="https://twitter.com/hashtag/FutureAhead?src=hash&amp;ref_src=twsrc%5Etfw">#FutureAhead</a> <a href="https://t.co/xvFUv59EqW">https://t.co/xvFUv59EqW</a></p>&mdash; Valentin Podkamennyi (@vpodk) <a href="https://twitter.com/vpodk/status/1941847225291915594?ref_src=twsrc%5Etfw">July 6, 2025</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>