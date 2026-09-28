..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==========================================
Guía 10: Firebase: Realtime Database y SDK
==========================================

.. topic:: Objetivo específico
    :class: objetivo

    Integrar Firebase Realtime Database utilizando su SDK para el almacenamiento y recuperación de datos en tiempo real desde una landing page desarrollada con JavaScript y TailwindCSS, permitiendo una experiencia de usuario dinámica e interactiva.

Actividades previas
=====================

Firebase: Proyecto
------------------

1. Acceda a `Firebase <https://firebase.google.com/>`_ con su cuenta personal de Google.
2. En `Firebase Console <https://console.firebase.google.com/>`_, cree el proyecto **landing**. No es necesario configurar Google Analytics para este proyecto.
3. Utilice un cliente de IAG para explicar los servicios, y sus casos prácticos de uso, que ofrece Firebase.

Ambiente de desarrollo
----------------------

1. Acceda a su proyecto *landing* en Codespaces o en su máquina local.
2. Cree y utilice la(s) rama(s) de desarrollo.
3. Instale los paquetes y levante el servidor, con:

   .. code-block:: bash

      npm install
      npm run dev

Actividades en clases
=====================

HTML
----

1. En el documento *index.html*, agregue una sección registrar el voto por un producto y mostrar el resultado de la votación.

   .. dropdown:: Ver el código 
    :color: primary
    
    .. code-block:: html
        :emphasize-lines: 3-27

        <section class="bg-slate-50 dark:bg-gray-900"> ... </section>

        <section class="flex flex-col items-center justify-center bg-white py-8">
            <div class="bg-white p-8 rounded-lg shadow-md w-full max-w-md">
               <h2 class="text-2xl font-bold mb-6 text-center text-gray-800">Vota por tu producto preferido</h2>

               <div class="mb-6">
               <form id="form_voting" class="relative flex items-center">
                  <select id="select_product"
                     class="block appearance-none w-full bg-white border border-gray-300 text-gray-700 py-3 px-4 pr-8 rounded-lg leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent">
                     <option value="" disabled selected>Seleccione un producto</option>
                     <option value="product1">Producto 1</option>
                     <option value="product2">Producto 2</option>
                     <option value="product3">Producto 3</option>
                  </select>
                  <button
                     class="ml-4 bg-blue-500 hover:bg-blue-600 text-white font-bold py-2 px-6 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-50">
                     VOTAR
                  </button>
               </form>
               </div>

               <div id="results" class="border border-gray-300 rounded-lg h-52 w-full p-2 bg-gray-50">
                  <p class="text-gray-500 text-center mt-16">Resultado de la votación</p>
               </div>
            </div>
        </section>

        <div id="toast-interactive" ... > </div>



Firebase: App - web
-------------------

1. En su proyecto de Firebase:
   
   a) Junto a **Project Overview**, despliegue el menú de configuración del proyecto y escoja **Project settings**.
   b) En la pestaña **General**, desplácese hasta la sección **Your apps**.
   c) Haga clic en el ícono de elemento HTML para crear una **web app**.

2. En la ventana modal **Add Firebase to your web app** 
    
   a) Ingrese un nombre para su aplicación web y haga clic en **Register app**.
   b) En **Add Firebase SDK**, seleccione la opción **Use a <script> tag** y copie la configuración para establecer la conexión con Firebase.

   
   .. code-block:: javascript

       const firebaseConfig = {
         apiKey: "API_KEY",
         authDomain: "PROJECT_ID.firebaseapp.com",
         projectId: "PROJECT_ID",
         storageBucket: "PROJECT_ID.firebasestorage.app",
         messagingSenderId: "SENDER_ID",
         appId: "APP_ID",
       };

3. Con un cliente de IAG, explique cómo se utiliza el objeto de configuración de Firebase en la inicialización de la aplicación web y en la conexión con los servicios con Vanilla Javascript.

Realtime Database
^^^^^^^^^^^^^^^^^

1. Dentro de su proyecto en Firebase, acceda a la categoría de productos **Build**, en la opción **Realtime Database**.
2. Cree una base de datos en tiempo real seleccionando **Create Database**.
   
   a) Seleccione la ubicación de la base de datos, preferiblemente la más cercana a su usuario final.
   b) En **Security rules**, elija **Start in Test Mode** para permitir el acceso sin restricciones durante el desarrollo inicial. 
   
   .. attention:: 

      **Nota de seguridad**: El modo de prueba permite que cualquier persona pueda leer y escribir en la base de datos sin autenticación. 
      Esto es útil para pruebas, pero asegúrese de cambiar a un modo más seguro antes de desplegar su aplicación en producción.

3. Utilice una cliente de IAG para explicar cómo se estructura la base de datos en tiempo real de Firebase.

.env
----

1. En la raíz de su proyecto, cree un archivo llamado **.env**.
2. En este archivo, agregue las siguiente variables de entorno y pegue los valores correspondientes de la configuración de Firebase que copió anteriormente:
    
   .. code-block:: env

       VITE_FIREBASE_API_KEY="API_KEY"
       VITE_FIREBASE_AUTH_DOMAIN="PROJECT_ID.firebaseapp.com"
       VITE_FIREBASE_PROJECT_ID="PROJECT_ID"
       VITE_FIREBASE_STORAGE_BUCKET="PROJECT_ID.firebasestorage.app"
       VITE_FIREBASE_MESSAGING_SENDER_ID="SENDER_ID"
       VITE_FIREBASE_APP_ID="APP_ID"

3. Asegúrese de que el archivo **.env** esté incluido en su archivo **.gitignore** para evitar subirlo al repositorio.

   .. code-block:: gitignore

       ...
       
       # Firebase environment variables
       .env

   .. attention::

      Al versionar, omita el archivo **.env** en el versionamiento local y remoto, para evitar exponer las credenciales de Firebase.

4. Con un cliente de IAG, explique la importancia de las variables de entorno para mantener la seguridad de las credenciales de Firebase y cómo se utilizan en el código en Vite.

JS: Conexión a Firebase
-----------------------

.. sidebar:: 

   .. image:: https://upload.wikimedia.org/wikipedia/commons/thumb/0/0b/New_Firebase_logo.svg/2560px-New_Firebase_logo.svg.png
      
   JavaScript en tu proyecto web en `Agrega Firebase al proyecto de JavaScript <https://firebase.google.com/docs/web/setup>`_.

1. Cree el documento javascript *js/firebase.js*, agregue el siguiente código: 

   a) Desde el CDN, importe la `última versión(firebase@x.y.z) <https://github.com/firebase/firebase-js-sdk/releases/latest>`_ de las funciones de Firebase para inicializar la aplicación (initializeApp), acceder a la base de datos en tiempo real (getDatabase, ref), crear datos (set, push) y obtener datos (get y child) .

      .. code-block:: javascript
         :emphasize-lines: 1-2

         import { initializeApp } from "https://www.gstatic.com/firebasejs/x.y.z/firebase-app.js";
         import { getDatabase, ref, set, push, get, child } from "https://www.gstatic.com/firebasejs/x.y.z/firebase-database.js";

   b) Utilice las variables de entorno definidas en el archivo **.env** para configurar la conexión a Firebase, considerando que utiliza Vite como herramienta de construcción.

      .. code-block:: javascript
         :emphasize-lines: 3-7

         ...

         const firebaseConfig = {
           apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
           ...,
           appId: import.meta.env.VITE_FIREBASE_APP_ID,
         };

   c) Inicialice la aplicación Firebase utilizando el objeto de configuración importado desde las variables de entorno.
   
      .. code-block:: javascript
         :emphasize-lines: 3

         const firebaseConfig = { ... };

         const app = initializeApp(firebaseConfig);

   d) Obtenga una referencia a la base de datos en tiempo real de Firebase asociada con la aplicación.

      .. code-block:: javascript
         :emphasize-lines: 3

         const app = ...;

         const database = getDatabase(app);

   e) Aún no exporte las funciones.

2. Con un cliente de IAG, explique cómo se utiliza el SDK de Firebase para enviar datos a la base de datos en tiempo real.

JS: Guardar votos en Firebase
-----------------------------

1. En el documento javascript *js/firebase.js*, implemente el siguiente código: 

   a) Define una función flecha `saveVote`, que reciba un parámetro `productID`.
   b) Dentro de la función, tome como referencia la documentación de `Operaciones básicas de escritura <https://firebase.google.com/docs/database/web/read-and-write?hl=es-419>`_:
      
      (i) Obtenga una referencia a la colección `votes` de la base de datos, con la función `ref()`.
      (ii) Crea una nueva referencia para un usuario utilizando la función `push()`.
      (iii) Guarda los datos (`productID` y la fecha actual) en la base de datos con la función `set()`.
      (iv) Maneje el resultado de la función `set()` como una promesa, devuelva un objeto con un estado y un mesaje de éxito o de error.
   
   c) Exporta la función `saveVote` para que pueda ser utilizada en otros archivos.

2. Con un cliente de IAG, explique cómo se utiliza el SDK de Firebase para realizar las operaciones CRUD (Create, Read, Update, Delete).

JS: Enviar datos desde un formulario
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

En el documento *js/file01.js*.

1. Importe la función `saveVote` desde *js/firebase.js*.
2. Implemente la siguiente funcionalidad: 

   a) Defina la función flecha `enableForm` (antes de la función de autoejecución).
   b) Dentro de la función flecha, obtenga una referencia al formulario HTML con el identificador \'form_voting\'.
   c) Con la referencia al formulario, utilice `addEventListener <https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener>`_ para el evento \'submit\' y un *listener* que:
      
      (i) Use el evento del *callback* para prevenir el comportamiento por defecto del formulario. Use el método `preventDefault <https://developer.mozilla.org/en-US/docs/Web/API/Event/preventDefault>`_.
      (ii) Obtenga la referencia al elemento con identificador \'select_product\' y extraiga el valor mediante el atributo `value <https://developer.mozilla.org/en-US/docs/Web/API/HTMLSelectElement/value>`_.
      (iii) Llame a la función `saveVote` con el valor obtenido del campo de texto. Maneje la promesa y muestre el resultado con un mensaje de alerta.

   d) Invoque la función `enableForm` en la función de autoejecución.

3. En el navegador, verifique que al enviar el formulario se guarden los votos en Firebase y que se muestre un mensaje de éxito o error.
4. Con un cliente de IAG, explique cómo se maneja la interacción entre el JavaScript y la interfaz de usuario, y cómo se envían los datos a Firebase.

JS: Obtener votos en Firebase
-----------------------------

1. Modifique el código del archivo *js/firebase.js*, de acuerdo con las siguientes especificaciones: 

   a) Defina `getVotes` y asigne una función flecha asíncrona.
   b) Dentro de la función, tome como referencia la documentación de `Lee los datos una sola vez <https://firebase.google.com/docs/database/web/read-and-write?hl=es-419#read_data_once>`_:
      
      (i) Obtenga una referencia a la colección `votes` de la base de datos, con la función `ref()`.
      (ii) Utilice la función `get` para esperar por los datos de la colección.
      (iii) Si existen datos, retorne un objeto con el estado y los datos obtenidos, con la función `val()`. De lo contrario, retorne un objeto con un estado y un mensaje indicando que no hay datos.
   
   c) Exporte la función `getVotes` para que pueda ser utilizada en otros archivos.

3. Con un cliente de IAG, explique cómo se utiliza el SDK de Firebase para obtener datos de la base de datos.

JS: Mostrar datos en una tabla
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

En el documento *js/file01.js*.

1. Importe la función `getVotes` desde *js/firebase.js*.
2. Implemente la siguiente funcionalidad: 

   a) Defina `displayVotes` y asigne una función flecha asíncrona.
   b) Dentro de la función, use la función `getVotes` y espere por los votos.
   c) Itere sobre los votos obtenidos y cree una tabla donde cada fila represente un voto. En cada fila, muestre el producto votado y el total de votos.
   d) Inserte la tabla en el elemento HTML con el identificador `results`.
   e) Invoque la función `displayVotes` en la función de autoejecución.

3. En el navegador, verifique que al cargar la página se muestren los votos almacenados en Firebase en una tabla.

Versionamiento
--------------

1. Versione local y remotamente la(s) rama(s) de desarrollo en el repositorio *landing*.
2. Genere la(s) solicitud(es) de cambios (pull request) para la rama principal y apruebe los cambios.

Vercel
------

.. attention::
   
   Agregue las variables de entorno con el nombre y valor que aparecen en el archivo **.env**.

1. Verifique el despliegue continuo (CD) del proyecto en Vercel.

Conclusiones
============

.. topic:: Preguntas de cierre

    * ¿Qué desafíos conceptuales encontraste al interpretar el código generado por IA para integrar Firebase en tu landing page?

    * ¿Qué modificaciones realizaste al código sugerido por la IA para adaptarlo a los requerimientos específicos de tu landing page?

    * ¿Cómo aseguras que el uso de IA en la implementación de Firebase no sustituya tu comprensión del flujo de datos ni tu responsabilidad en el manejo seguro de la información del usuario?

Actividades autónomas
=====================

Recursos extras
------------------------------

En redes:

.. raw:: html

    <blockquote class="twitter-tweet"><p lang="es" dir="ltr">🔥 <a href="https://twitter.com/hashtag/Firebase?src=hash&amp;ref_src=twsrc%5Etfw">#Firebase</a> está preparando un nuevo SDK para JavaScript que hará la librería más ligera y traerá cambios importantes que nos harán refactorizar nuestras apps si queremos aprovechas sus ventajas.<br><br>🧵 Te las cuento en el hilo 👇 <a href="https://t.co/oJHLopDw1J">pic.twitter.com/oJHLopDw1J</a></p>&mdash; Carlos Azaustre 💻 (@carlosazaustre) <a href="https://twitter.com/carlosazaustre/status/1421036271242252288?ref_src=twsrc%5Etfw">July 30, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>