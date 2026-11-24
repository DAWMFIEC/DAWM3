..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==========================================================================
Tutorial: Ionic y React - Aplicación híbrida y generación del APK Android
==========================================================================

.. topic:: Objetivo específico
   :class: objetivo

   Desarrollar una aplicación móvil híbrida con Ionic Framework y React, empaquetarla con Capacitor para la plataforma Android y generar el archivo APK para instalarla en un dispositivo móvil.

Desarrollo del tutorial
=======================

Requisitos previos
------------------

#. Verifique que tiene instalado Node.js 22 o superior, npm y Git. Capacitor 8 requiere Node.js 22 como versión mínima:

   .. code-block:: bash

      node --version
      npm --version
      git --version

#. Para generar el APK en su máquina local, instale `Android Studio <https://developer.android.com/studio>`_ en la versión *Otter 2025.2.1* o superior, que es la mínima requerida por Capacitor 8 (Capacitor, n.d.-a). Android Studio incluye el SDK de Android y el JDK 21 que usa Gradle para compilar el proyecto.

   .. note::

      Si trabaja en GitHub Codespaces, no necesita Android Studio: el APK se puede generar en la nube con GitHub Actions, como se describe en la sección *Generación del APK con GitHub Actions*.

Preparación del repositorio
---------------------------

#. Cree un repositorio en GitHub con el nombre *mobile*:

   - Agregue un archivo ``README.md`` con el título de su aplicación híbrida y una breve descripción del objetivo del proyecto.
   - Agregue el archivo ``.gitignore`` con la plantilla de *Node*.

#. Acceda al repositorio *mobile* en Codespaces o clónelo en su máquina local.
#. Cree la rama de desarrollo y cámbiese a ella:

   .. code-block:: bash

      git checkout -b desarrollo

Instalación de Ionic CLI
------------------------

#. Instale Ionic CLI de forma global:

   .. code-block:: bash

      npm install -g @ionic/cli

#. Verifique la instalación:

   .. code-block:: bash

      ionic --version

   Ionic Framework es una biblioteca de componentes de interfaz (botones, listas, pestañas, barras de navegación) con apariencia nativa para iOS y Android, que se integra con React, Angular o Vue. Ionic CLI es la herramienta de línea de comandos que crea proyectos a partir de plantillas, levanta el servidor de desarrollo y ejecuta los comandos de Capacitor (Ionic, n.d.-a).

Creación de la aplicación híbrida con Ionic y React
---------------------------------------------------

#. Cree la aplicación en una carpeta temporal a partir de la plantilla *tabs*, con React y Capacitor:

   .. code-block:: bash

      ionic start temp tabs --type=react --capacitor --no-git --no-deps

   - ``tabs`` indica la plantilla inicial, con navegación por pestañas.
   - ``--type=react`` selecciona React como framework.
   - ``--capacitor`` integra Capacitor como herramienta de empaquetado nativo.
   - ``--no-git`` evita crear un repositorio Git dentro de ``temp``, porque el proyecto ya está dentro del repositorio *mobile*.
   - ``--no-deps`` omite la instalación de dependencias, que se hará después en la raíz del proyecto.

   .. note::

      No es necesario crear una cuenta en Ionic si el asistente lo solicita.

#. Copie el contenido de ``temp`` a la raíz del repositorio, incluidos los archivos ocultos, sin sobrescribir el ``README.md`` ni el ``.gitignore`` existentes, y elimine la carpeta temporal:

   .. code-block:: bash

      cp -rn temp/. .
      rm -rf temp

#. Instale las dependencias del proyecto:

   .. code-block:: bash

      npm install

#. Abra el archivo ``capacitor.config.ts`` y reemplace el identificador y el nombre de la aplicación, que la plantilla generó a partir de la carpeta ``temp``:

   .. code-block:: typescript
      :emphasize-lines: 4-5

      import type { CapacitorConfig } from '@capacitor/cli';

      const config: CapacitorConfig = {
        appId: 'ec.edu.espol.mobile',
        appName: 'mobile',
        webDir: 'dist'
      };

      export default config;

   - ``appId`` es el identificador único del paquete Android, en notación de dominio invertido. Defínalo antes de agregar la plataforma Android, porque se usa para generar el proyecto nativo.
   - ``appName`` es el nombre visible de la aplicación en el dispositivo.
   - ``webDir`` es la carpeta donde Vite deja el sitio web compilado, que Capacitor copia al proyecto nativo.

Estructura del proyecto
-----------------------

El proyecto generado por Ionic CLI contiene los siguientes elementos principales:

.. list-table:: Estructura del proyecto Ionic con React
   :header-rows: 1
   :widths: 35 65

   * - Elemento
     - Descripción
   * - ``src/main.tsx``
     - Punto de entrada: monta el componente ``App`` en el DOM.
   * - ``src/App.tsx``
     - Componente raíz. Inicializa Ionic con ``setupIonicReact``, define las rutas con ``IonReactRouter`` y la barra de pestañas con ``IonTabs``, ``IonTabBar`` e ``IonTabButton``.
   * - ``src/pages/``
     - Vistas de cada pestaña (``Tab1.tsx``, ``Tab2.tsx``, ``Tab3.tsx``), construidas con ``IonPage``, ``IonHeader``, ``IonToolbar`` e ``IonContent``.
   * - ``src/components/``
     - Componentes reutilizables, como ``ExploreContainer``.
   * - ``src/theme/variables.css``
     - Variables CSS para personalizar colores y el tema de Ionic.
   * - ``public/``
     - Recursos estáticos, como el ícono y el ``manifest.json``.
   * - ``capacitor.config.ts``
     - Configuración de Capacitor: identificador, nombre y carpeta web de la aplicación.
   * - ``ionic.config.json``
     - Configuración de Ionic CLI para el proyecto.
   * - ``vite.config.ts``
     - Configuración de Vite, el empaquetador del sitio web.

Ejecución en modo de desarrollo
-------------------------------

#. Levante el servidor de desarrollo:

   .. code-block:: bash

      ionic serve

#. Revise la aplicación en el navegador y navegue entre las tres pestañas.
#. Abra las herramientas de desarrollador y active la vista de dispositivos móviles (*Toggle device toolbar*). Seleccione distintos dispositivos (por ejemplo, *Pixel 7* o *iPhone 14*) y observe cómo los componentes de Ionic adaptan su apariencia a cada plataforma.

Plataforma Android con Capacitor
--------------------------------

Capacitor es un entorno de ejecución que empaqueta el sitio web compilado dentro de un proyecto nativo. En Android, la aplicación se ejecuta dentro de un componente *WebView* y puede acceder a funciones nativas del dispositivo (cámara, geolocalización, almacenamiento) mediante complementos (Capacitor, n.d.-b).

#. Compile el sitio web de la aplicación:

   .. code-block:: bash

      ionic build

   El resultado queda en la carpeta ``dist``, indicada en ``webDir``.

#. Agregue la plataforma Android, que genera el proyecto nativo en la carpeta ``android`` de la raíz del proyecto:

   .. code-block:: bash

      ionic cap add android

#. Sincronice el proyecto nativo:

   .. code-block:: bash

      ionic cap sync android

   El comando ``sync`` copia los recursos web más recientes de ``dist`` al proyecto Android e instala los complementos nativos que se hayan agregado. Ejecútelo cada vez que modifique el código de la aplicación, antes de generar un nuevo APK.

   La carpeta ``android`` es un proyecto Android estándar, basado en Gradle. Los recursos web se copian en ``android/app/src/main/assets/public``.

Generación del APK con Android Studio
-------------------------------------

#. Abra el proyecto Android en Android Studio:

   .. code-block:: bash

      ionic cap open android

#. Espere a que Android Studio termine la sincronización de Gradle (barra de progreso inferior).
#. En el menú **Build**, seleccione **Generate App Bundles or APKs > Generate APKs**.
#. Al finalizar, haga clic en **locate** en la notificación para abrir la carpeta del archivo generado:

   .. code-block:: text

      android/app/build/outputs/apk/debug/app-debug.apk

   Como alternativa, genere el APK desde la terminal, en la carpeta ``android``:

   .. code-block:: bash

      cd android
      ./gradlew assembleDebug

   En Windows, use ``gradlew.bat assembleDebug``.

   .. note::

      El APK de tipo *debug* está firmado con una clave de depuración y sirve para pruebas. Para publicar en Google Play se requiere un paquete *release* firmado con una clave propia.

Generación del APK con GitHub Actions
-------------------------------------

GitHub Actions permite compilar el APK en la nube cada vez que se actualiza la rama principal, sin instalar Android Studio. Los ejecutores ``ubuntu-latest`` de GitHub incluyen el SDK de Android.

.. note::

   Las versiones anteriores de este contenido usaban Ionic Appflow para la compilación en la nube. Ionic dejó de vender Appflow a nuevos clientes en febrero de 2025 y el servicio finaliza el 31 de diciembre de 2027 (Ionic, 2025), por lo que este tutorial usa GitHub Actions.

#. Cree el archivo ``.github/workflows/android.yml`` en la raíz del proyecto:

   .. dropdown:: android.yml
      :color: success

      .. code-block:: yaml

         name: Android APK

         on:
           push:
             branches: [main]
           workflow_dispatch:

         jobs:
           build:
             runs-on: ubuntu-latest

             steps:
               - name: Obtener el código
                 uses: actions/checkout@v5

               - name: Configurar Node.js
                 uses: actions/setup-node@v5
                 with:
                   node-version: 22
                   cache: npm

               - name: Configurar Java
                 uses: actions/setup-java@v5
                 with:
                   distribution: temurin
                   java-version: 21

               - name: Instalar dependencias
                 run: npm ci

               - name: Compilar el sitio web
                 run: npm run build

               - name: Sincronizar Capacitor
                 run: npx cap sync android

               - name: Compilar el APK
                 working-directory: android
                 run: |
                   chmod +x gradlew
                   ./gradlew assembleDebug

               - name: Publicar el APK
                 uses: actions/upload-artifact@v5
                 with:
                   name: app-debug
                   path: android/app/build/outputs/apk/debug/app-debug.apk

   - ``on`` indica los eventos que ejecutan el flujo: cada ``push`` a ``main`` y la ejecución manual (``workflow_dispatch``).
   - ``actions/setup-node`` y ``actions/setup-java`` instalan Node.js 22 y el JDK 21 requeridos por Capacitor 8.
   - Los pasos siguientes repiten los comandos de las secciones anteriores: instalar dependencias, compilar el sitio web, sincronizar Capacitor y compilar con Gradle.
   - ``actions/upload-artifact`` publica el APK como un artefacto descargable de la ejecución.

#. Verifique que la carpeta ``android`` no esté excluida en ``.gitignore``, porque el flujo necesita el proyecto nativo versionado en el repositorio.

Versionamiento y publicación
----------------------------

#. Versione local y remotamente la rama de desarrollo en el repositorio *mobile*:

   .. code-block:: bash

      git add .
      git commit -m "Aplicación híbrida con Ionic, React y Capacitor para Android"
      git push origin desarrollo

#. Genere la solicitud de cambios (*pull request*) hacia la rama principal (*main*) y apruebe los cambios.
#. Acceda a la pestaña **Actions** del repositorio y abra la ejecución del flujo **Android APK**. Espere a que todos los pasos terminen correctamente.
#. En la sección **Artifacts** de la ejecución, descargue el archivo ``app-debug.zip`` y extraiga ``app-debug.apk``.

Instalación en un dispositivo Android
-------------------------------------

#. Transfiera el archivo ``app-debug.apk`` al dispositivo Android (por cable USB, correo electrónico o un servicio de almacenamiento en la nube).
#. Abra el archivo desde el administrador de archivos del dispositivo. Si el sistema lo solicita, permita la instalación de aplicaciones de origen desconocido para esa aplicación.
#. Confirme la instalación y abra la aplicación desde el menú del dispositivo.

   Como alternativa, con el dispositivo conectado por USB y la *depuración USB* activada en las opciones de desarrollador, instale el APK desde la terminal:

   .. code-block:: bash

      adb install android/app/build/outputs/apk/debug/app-debug.apk

   O ejecute la aplicación directamente en el dispositivo o en un emulador:

   .. code-block:: bash

      ionic cap run android

Referencias
===========

* Capacitor. (n.d.-a). Environment Setup. Retrieved November 24, 2026 from https://capacitorjs.com/docs/getting-started/environment-setup
* Capacitor. (n.d.-b). Android Documentation. Retrieved November 24, 2026 from https://capacitorjs.com/docs/android
* GitHub. (n.d.). Store and share data with workflow artifacts. Retrieved November 24, 2026 from https://docs.github.com/en/actions/tutorials/store-and-share-data
* Ionic. (n.d.-a). Ionic React Overview. Retrieved November 24, 2026 from https://ionicframework.com/docs/react
* Ionic. (n.d.-b). Ionic CLI: ionic start. Retrieved November 24, 2026 from https://ionicframework.com/docs/cli/commands/start
* Ionic. (2025, February 11). Announcement on Ionic commercial products. Retrieved November 24, 2026 from https://ionic.io/blog/?p=6282
