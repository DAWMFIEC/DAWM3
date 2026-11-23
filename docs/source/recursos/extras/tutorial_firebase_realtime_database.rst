..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==========================================================================
Tutorial: Consumo de datos con Firebase Realtime Database SDK en React
==========================================================================

.. topic:: Objetivo específico
   :class: objetivo

   Integrar el SDK web de Firebase Realtime Database en una aplicación React, con TypeScript, mediante la inicialización del SDK, la definición de un modelo de datos, un servicio de acceso a datos y un hook personalizado que actualiza la interfaz según la ciudad seleccionada.

Desarrollo del tutorial
=======================

Requisitos
----------

#. Verifique que tiene instalado Node.js (versión LTS) y npm:

   .. code-block:: bash

      node --version
      npm --version

#. Cree un proyecto de React con TypeScript mediante Vite e instale las dependencias:

   .. code-block:: bash

      npm create vite@latest clima-firebase -- --template react-ts
      cd clima-firebase
      npm install

#. Fije las versiones de React y TypeScript del tutorial e instale MUI:

   .. code-block:: bash

      npm install react@19.3.1 react-dom@19.3.1
      npm install -D typescript@6.0.3
      npm install @mui/material @emotion/react @emotion/styled

#. Compruebe las versiones instaladas:

   .. code-block:: bash

      npm list react react-dom typescript

#. Cree la estructura de carpetas que se usará a lo largo del tutorial:

   .. code-block:: text

      src/
      ├── firebase/
      │   └── firebase.ts
      ├── types/
      │   └── Clima.ts
      ├── services/
      │   └── climaService.ts
      ├── hooks/
      │   └── useFetchData.ts
      └── App.tsx

   Cada archivo cumple una responsabilidad: ``firebase.ts`` inicializa el SDK, ``Clima.ts`` define la forma de los datos, ``climaService.ts`` accede a la base de datos, ``useFetchData.ts`` conecta el servicio con el estado de React y ``App.tsx`` muestra los datos.

Creación del proyecto en Firebase
---------------------------------

#. Acceda a `Firebase console <https://console.firebase.google.com/>`_ con su cuenta de Google y seleccione **Crear un proyecto de Firebase nuevo**.
#. Ingrese un nombre para el proyecto (por ejemplo, *data-repository*). Firebase genera a partir de él un **ID de proyecto** único.
#. *Asistencia de IA* y *Google Analytics* son opcionales; puede desactivarlo para este tutorial. Finalice la creación del proyecto.
#. En **Configuración > General**, cree una aplicación **Web** (ícono ``</>``).
   #. Asigne un sobrenombre a la aplicación (por ejemplo, *data-react*) y haga clic en **Registrar app**. No es necesario configurar Firebase Hosting.
   #. Copie el código de configuración de Firebase. Lo usará en la sección de inicialización (Firebase, n.d.-a), similar a:

   .. code-block:: javascript

      // Import the functions you need from the SDKs you need
      import { initializeApp } from "firebase/app";
      // TODO: Add SDKs for Firebase products that you want to use
      // https://firebase.google.com/docs/web/setup#available-libraries

      // Your web app's Firebase configuration
      const firebaseConfig = {
         apiKey: "<API_KEY>",
         authDomain: "<ID-del-proyecto>.firebaseapp.com",
         projectId: "<ID-del-proyecto>",
         storageBucket: "<ID-del-proyecto>.firebasestorage.app",
         messagingSenderId: "<SENDER_ID>",
         appId: "<APP_ID>"
      };
      
      // Initialize Firebase
      const app = initializeApp(firebaseConfig);

Creación de la base de datos en Realtime Database
-------------------------------------------------

#. En el menú lateral de Firebase console, acceda a **Bases de datos y almacenamiento > Realtime Database** y haga clic en **Crear una base de datos**.
#. Seleccione la ubicación de la base de datos (por ejemplo, *Estados Unidos (us-central1)*).
#. Seleccione **Comenzar en modo bloqueado** y finalice el asistente.
#. Copie la URL de la base de datos que aparece en la parte superior de la pestaña **Datos**. Según la ubicación, tiene uno de estos formatos (Firebase, n.d.-b):

   .. code-block:: text

      https://<ID-del-proyecto>-default-rtdb.firebaseio.com
      https://<ID-del-proyecto>-default-rtdb.<region>.firebasedatabase.app

#. Utilice el archivo en formato **JSON** con los datos de su aplicación. Por ejemplo, el archivo ``clima.json`` tiene el siguiente contenido:

   .. code-block:: json

      {
        "clima": {
          "guayaquil": {
            "ciudad": "Guayaquil",
            "temperatura": 29.4,
            "humedad": 72,
            "viento": 12.5
          },
          "quito": {
            "ciudad": "Quito",
            "temperatura": 15.8,
            "humedad": 65,
            "viento": 8.2
          }
        }
      }

#. En la pestaña **Datos**, abra el menú de opciones (``⋮``), seleccione **Importar JSON** y cargue el archivo ``clima.json``.

   Realtime Database almacena la información como un único árbol JSON. Cada nodo se identifica por su ruta; por ejemplo, ``clima/guayaquil`` contiene los datos de Guayaquil.

#. En la pestaña **Reglas**, permita solo la lectura pública del nodo ``clima`` y **publique** los cambios:

   .. code-block:: json

      {
        "rules": {
          "clima": {
            ".read": true,
            ".write": false
          }
        }
      }

   .. note::

      Las rutas que no aparecen en las reglas quedan denegadas por defecto. Con esta configuración, cualquier cliente puede leer ``clima``, pero ninguno puede modificarlo desde la aplicación.

Integración de Firebase con React
-------------------------------

Instalación del SDK de Firebase
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Instale el paquete ``firebase`` desde npm:

   .. code-block:: bash

      npm install firebase

#. Compruebe la versión instalada:

   .. code-block:: bash

      npm list firebase

   El SDK web usa una API modular: cada servicio se importa desde su propio subpaquete con la forma ``firebase/<servicio>``. Vite agrupa solo las funciones importadas, lo que reduce el tamaño del sitio final (Firebase, n.d.-a).

Inicialización de Firebase
^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Cree el archivo ``src/firebase/firebase.ts``:

   - Importe ``initializeApp`` desde ``firebase/app`` y ``getDatabase`` desde ``firebase/database``.
   - Pegue el objeto ``firebaseConfig`` y agregue la propiedad ``databaseURL`` con la URL de su base de datos.
   - Inicialice la aplicación con ``initializeApp`` y exporte la instancia de la base de datos obtenida con ``getDatabase``.

   .. dropdown:: firebase.ts
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 7

         import { initializeApp } from "firebase/app";

         import { getDatabase } from "firebase/database";

         const firebaseConfig = {
           apiKey: "<API_KEY>",
           databaseURL: "https://<ID-del-proyecto>-default-rtdb.firebaseio.com",
           authDomain: "<ID-del-proyecto>.firebaseapp.com",
           projectId: "<ID-del-proyecto>",
           storageBucket: "<ID-del-proyecto>.firebasestorage.app",
           messagingSenderId: "<SENDER_ID>",
           appId: "<APP_ID>"
         };

         const app = initializeApp(firebaseConfig);

         export const database = getDatabase(app);

   El objeto ``app`` es el contenedor que guarda la configuración común del proyecto y la comparte entre los servicios de Firebase. El objeto ``database`` es la instancia de Realtime Database asociada a esa aplicación.

   .. note::

      Si omite ``databaseURL``, el SDK intenta usar la URL por defecto ``https://<projectId>-default-rtdb.firebaseio.com``. Esa URL no funciona para bases de datos creadas fuera de Estados Unidos, por lo que conviene declararla siempre.

      La ``apiKey`` de una aplicación web identifica el proyecto ante Firebase y no es una credencial secreta. El acceso a los datos lo controlan las reglas de seguridad configuradas en la sección anterior.

Definición del modelo de datos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Utilice las interfaces de su aplicación para definir el modelo de datos. Por ejemplo, el archivo ``src/types/Clima.ts`` contiene la interfaz ``Clima``, que describe la forma de cada nodo hijo de ``clima``:

   .. dropdown:: Clima.ts
      :color: success

      .. code-block:: typescript

         export interface Clima {
           ciudad: string;
           temperatura: number;
           humedad: number;
           viento: number;
         }

   Las propiedades de la interfaz coinciden con las claves del JSON importado. Si agrega una nueva clave en la base de datos (por ejemplo, ``presion``), agregue también la propiedad correspondiente en la interfaz para que TypeScript la reconozca.

Servicio de acceso a datos
^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Cree un archivo para el requerimiento de acceso a datos, por ejemplo, el archivo ``src/services/climaService.ts`` con la función asincrónica ``obtenerClima``:

   - Reciba como parámetro la ruta del nodo que se desea leer.
   - Cree una referencia al nodo con ``ref(database, ruta)``.
   - Lea los datos una sola vez con ``get(referencia)``, que retorna una promesa con una instantánea (*snapshot*).
   - Si la instantánea contiene datos (``snapshot.exists()``), retorne su valor (``snapshot.val()``) con el tipo ``Clima``; en caso contrario, retorne ``null``.

   .. dropdown:: climaService.ts
      :color: success

      .. code-block:: typescript

         import { get, ref } from "firebase/database";
         import { database } from "../firebase/firebase";
         import type { Clima } from "../types/Clima";

         export async function obtenerClima( collection: string ): Promise<Clima | null> {

           const referencia = ref(database, `${collection}`);

           const snapshot = await get(referencia);

           if (snapshot.exists()) {
             return snapshot.val() as Clima;
           }

           return null;
         }

   Las funciones ``ref`` y ``get`` pertenecen al subpaquete ``firebase/database``. Una instantánea representa los datos de la ruta en el momento de la lectura: ``exists()`` retorna ``false`` y ``val()`` retorna ``null`` cuando la ruta no contiene datos (Firebase, n.d.-c).

   Separar el acceso a datos en un servicio permite que los componentes de React no dependan directamente del SDK de Firebase.

Hook personalizado para obtener los datos
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Cree el archivo ``src/hooks/useFetchData.ts`` con el hook ``useFetchData``:

   - Reciba como parámetro el identificador de la ciudad (``ciudad``), de tipo ``string``.
   - Declare la variable de estado ``clima`` (de tipo ``Clima`` o ``null``, con valor inicial ``null``).
   - Dentro de ``useEffect``, defina la función asincrónica ``cargarDatos``, que llama a ``obtenerClima`` con la ruta ``clima/<ciudad>`` y guarda el resultado en el estado.
   - Evite la petición mientras no exista una ciudad seleccionada.
   - Agregue ``ciudad`` al arreglo de dependencias para que el efecto se ejecute cada vez que cambie la ciudad.
   - Retorne un objeto con la propiedad ``clima``.

   .. dropdown:: useFetchData.ts
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 5,11-13

         import { useEffect, useState } from "react";
         import { obtenerClima } from "../services/climaService";
         import type { Clima } from "../types/Clima";

         export default function useFetchData( ciudad: string ) {

           const [clima, setClima] = useState<Clima | null>(null);

           useEffect(() => {

             if (!ciudad) {
               return;
             }

             async function cargarDatos() {

               const datos = await obtenerClima(`clima/${ciudad}`);

               setClima(datos);
             }

             cargarDatos();

           }, [ciudad]);

           return { clima }
         }

   .. note::

      El parámetro se declara con el tipo primitivo ``string`` y no con el tipo envoltorio ``String``. La guarda ``if (!ciudad)`` evita que, con el valor inicial ``''``, se lea la ruta ``clima/``, que retornaría el nodo completo con todas las ciudades en lugar de un objeto ``Clima``.

Integración en el componente App
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Modifique el archivo ``src/App.tsx``:

   - Declare la variable de estado ``cityInput`` (valor inicial ``''``) para la ciudad seleccionada.
   - Llame al hook ``useFetchData`` con ``cityInput`` y desestructure la propiedad ``clima``.
   - En ``handleChange``, actualice ``cityInput`` con el valor seleccionado en el ``Select``.
   - Use como valores de las opciones del ``Select`` las mismas claves de la base de datos (``guayaquil`` y ``quito``).
   - Muestre la temperatura y la humedad solo si ``clima`` contiene datos.

   .. dropdown:: App.tsx
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 9,14-15,34-35,40-48

         import { useState } from 'react';

         import { Grid, Typography } from '@mui/material';
         import FormControl from '@mui/material/FormControl';
         import InputLabel from '@mui/material/InputLabel';
         import Select, { type SelectChangeEvent } from '@mui/material/Select';
         import MenuItem from '@mui/material/MenuItem';

         import useFetchData from './hooks/useFetchData';
         import './App.css'

         function App() {

           const [cityInput, setCityInput] = useState('');
           const { clima } = useFetchData(cityInput);

           const handleChange = (event: SelectChangeEvent<string>) => {
             setCityInput(event.target.value);
           };

           return (
             <Grid container spacing={5} sx={{ justifyContent: 'center', alignItems: 'center' }}>
               {/* Selector */}
               <Grid size={{ xs: 12, md: 3 }}>
                 <FormControl fullWidth>
                   <InputLabel id="city-select-label">Ciudad</InputLabel>
                   <Select
                     labelId="city-select-label"
                     id="city-simple-select"
                     label="Ciudad"
                     value={cityInput}
                     onChange={handleChange}>
                     <MenuItem disabled><em>Seleccione una ciudad</em></MenuItem>
                     <MenuItem value={'guayaquil'}>Guayaquil</MenuItem>
                     <MenuItem value={'quito'}>Quito</MenuItem>
                   </Select>
                 </FormControl>
               </Grid>
               <Grid size={{ xs: 12, md: 12 }}>
                 {clima &&
                   <>
                     <Typography variant="h5" component="div">
                       Temperatura: {clima.temperatura}
                     </Typography>
                     <Typography variant="h5" component="div">
                       Humedad: {clima.humedad}
                     </Typography>
                   </>}
               </Grid>
             </Grid>
           )
         }

         export default App

   El recorrido de los datos es el siguiente:

   - El usuario selecciona una ciudad y ``handleChange`` actualiza el estado ``cityInput``.
   - React vuelve a renderizar ``App`` y el hook ``useFetchData`` recibe la nueva ciudad.
   - Como ``ciudad`` cambió, se ejecuta el efecto y se llama a ``obtenerClima`` con la ruta ``clima/<ciudad>``.
   - El servicio lee el nodo con ``get`` y retorna un objeto ``Clima``.
   - El hook guarda el objeto en el estado ``clima`` y ``App`` muestra los nuevos valores.

#. Levante el servidor de desarrollo:

   .. code-block:: bash

      npm run dev

Verificación del funcionamiento
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Abra la aplicación en el navegador y seleccione cada ciudad. Compruebe que los valores coinciden con los de la pestaña **Datos** de Firebase console.
#. Abra las herramientas de desarrollador, acceda a la pestaña **Network** y filtre por **WS**. El SDK de Realtime Database se comunica con el servidor mediante una conexión WebSocket, no con peticiones HTTP individuales; los mensajes de cada lectura aparecen dentro de esa conexión.
#. Modifique un valor en Firebase console (por ejemplo, la temperatura de Quito), vuelva a seleccionar la ciudad en la aplicación y compruebe que se muestra el nuevo valor.
#. Si la consola del navegador muestra el mensaje ``permission_denied``, revise las reglas de seguridad del nodo ``clima``. Si los datos no llegan y no aparece ningún error, revise el valor de ``databaseURL`` en ``firebase.ts``.

Opcional: Suscripción a cambios en tiempo real
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

La función ``get`` lee los datos una sola vez. Para que la interfaz se actualice automáticamente cuando cambian los datos en la base, el SDK ofrece la función ``onValue``, que registra un observador sobre la ruta y lo ejecuta con cada cambio (Firebase, n.d.-c).

#. Opcionalmente, reemplace el contenido de ``src/hooks/useFetchData.ts`` por una versión que use ``onValue``:

   - Cree la referencia ``clima/<ciudad>`` directamente en el hook.
   - Registre el observador con ``onValue``, que retorna una función para cancelar la suscripción.
   - Retorne esa función desde el efecto para cancelar la suscripción anterior al cambiar de ciudad o al desmontar el componente.

   .. dropdown:: useFetchData.ts (tiempo real)
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 2,16-22

         import { useEffect, useState } from "react";
         import { onValue, ref } from "firebase/database";
         import { database } from "../firebase/firebase";
         import type { Clima } from "../types/Clima";

         export default function useFetchData( ciudad: string ) {

           const [clima, setClima] = useState<Clima | null>(null);

           useEffect(() => {

             if (!ciudad) {
               return;
             }

             const referencia = ref(database, `clima/${ciudad}`);

             const cancelarSuscripcion = onValue(referencia, (snapshot) => {
               setClima(snapshot.exists() ? snapshot.val() as Clima : null);
             });

             return cancelarSuscripcion;

           }, [ciudad]);

           return { clima }
         }

#. Con la aplicación abierta, modifique un valor en Firebase console y compruebe que la interfaz se actualiza sin volver a seleccionar la ciudad.

Versionamiento y publicación
----------------------------

#. Versione local y remotamente los cambios en la rama de desarrollo:

   .. code-block:: bash

      git add .
      git commit -m "Consumo de datos con Firebase Realtime Database SDK"
      git push origin desarrollo

#. Genere la solicitud de cambios (*pull request*) hacia la rama principal y apruebe los cambios.
#. Ejecute la transpilación y el despliegue del sitio web:

   .. code-block:: bash

      npm run deploy

   - De ser necesario, elimine, corrija o comente las secciones de código identificadas por el transpilador.
   - Vuelva a ejecutar el comando de despliegue.

#. Compruebe el resultado en el navegador, con la URL: `https://<username>.github.io/<repositorio>`

Referencias
===========

* Firebase. (n.d.-a). Agrega Firebase a tu proyecto de JavaScript. Retrieved November 23, 2026 from https://firebase.google.com/docs/web/setup?hl=es-419
* Firebase. (n.d.-b). Comienza a usar Firebase Realtime Database. Retrieved November 23, 2026 from https://firebase.google.com/docs/database/web/start?hl=es
* Firebase. (n.d.-c). Lee y escribe datos en la Web. Retrieved November 23, 2026 from https://firebase.google.com/docs/database/web/read-and-write?hl=es-419
* MUI. (n.d.). Select - Material UI. Retrieved November 23, 2026 from https://mui.com/material-ui/react-select/
* Vite. (n.d.). Getting Started. Retrieved November 23, 2026 from https://vite.dev/guide/
