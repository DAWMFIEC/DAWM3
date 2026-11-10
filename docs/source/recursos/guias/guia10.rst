..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==================================
Guía 10: React - Hooks (useEffect)
==================================

.. topic:: Objetivo específico
   :class: objetivo

   Implementar la gestión de efectos secundarios en el dashboard del clima mediante un hook personalizado con useEffect, que realiza una petición asincrónica a la API de Open-Meteo y actualiza dinámicamente los indicadores tipados con interfaces de TypeScript.

Actividades en clases
=====================

¿Qué datos del clima actual entrega la API de Open-Meteo?
---------------------------------------------------------

#. Acceda a su proyecto *dashboard* en Codespaces o en su máquina local, cámbiese a la rama de desarrollo y levante el servidor:

   .. code-block:: bash

      git checkout desarrollo
      npm install
      npm run dev

#. Acceda al sitio `Open-Meteo API <https://open-meteo.com/en/docs>`_ (Open-Meteo, n.d.) y configure la consulta:

   - En la sección **Location and Time**, ingrese las coordenadas de Guayaquil (latitud ``-2.1962``, longitud ``-79.8862``) y seleccione la zona horaria *America/Guayaquil*.
   - En la sección **Current Weather**, marque los indicadores: `Temperature (2 m)`, `Relative Humidity (2 m)`, `Apparent Temperature` y `Wind Speed (10 m)`.
   - En **Settings**, escoja las unidades de medida: temperatura en `Celsius °C`, velocidad del viento en `km/h` y precipitación en `Millimeter`.

#. Copie la URL generada en la sección **API Response** y ábrala en una pestaña de su navegador. La URL debe ser similar a:

   .. code-block:: text

      https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&timezone=America%2FGuayaquil

#. Complete la tabla con las claves del JSON de respuesta:

   .. list-table:: Estructura de la respuesta de Open-Meteo
      :header-rows: 1
      :widths: 30 45 25

      * - Clave
        - ¿Qué contiene?
        - Tipo de dato
      * - ``latitude`` / ``longitude``
        -
        -
      * - ``timezone``
        -
        -
      * - ``current_units``
        -
        -
      * - ``current``
        -
        -
      * - ``current.time``
        -
        -
      * - ``current.temperature_2m``
        -
        -
      * - ``current_units.temperature_2m``
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué parámetros de la URL corresponden a cada opción que configuró en el sitio de Open-Meteo?
#. ¿Por qué las unidades (``current_units``) se envían en un objeto separado de los valores (``current``)?
#. ¿Por qué la URL no incluye parámetros para las unidades de medida que eligió en **Settings**?
#. ¿Qué cambia en la respuesta si retira el parámetro ``timezone``? Fíjese en el valor de ``current.time``.
#. ¿Qué representa la clave ``interval`` dentro de ``current``?

¿Cómo describo la respuesta de la API con interfaces de TypeScript?
-------------------------------------------------------------------

#. Acceda al sitio `Transform Tools / JSON to TypeScript <https://transform.tools/json-to-typescript>`_ (Transform Tools, n.d.).
#. Pegue el JSON de respuesta de la API de Open-Meteo para generar las interfaces de TypeScript.
#. Dentro de su proyecto *dashboard*:

   - Cree el archivo `src/types/DashboardTypes.ts`.
   - Coloque el código de las interfaces generadas y cambie el nombre de la interfaz `Root` por `OpenMeteoResponse`.

   .. code-block:: typescript

      export interface ... {
          latitude: ...
          longitude: ...
          ...
          current_units: ...
          current: ...
      }

      export interface CurrentUnits { ... }

      export interface Current { ... }

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript

         export interface OpenMeteoResponse {
             latitude: number
             longitude: number
             generationtime_ms: number
             utc_offset_seconds: number
             timezone: string
             timezone_abbreviation: string
             elevation: number
             current_units: CurrentUnits
             current: Current
         }

         export interface CurrentUnits {
             time: string
             interval: string
             temperature_2m: string
             relative_humidity_2m: string
             apparent_temperature: string
             wind_speed_10m: string
         }

         export interface Current {
             time: string
             interval: number
             temperature_2m: number
             relative_humidity_2m: number
             apparent_temperature: number
             wind_speed_10m: number
         }

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Por qué la herramienta genera tres interfaces y no una sola? ¿Qué relación existe entre `OpenMeteoResponse`, `CurrentUnits` y `Current`?
#. ¿Por qué `temperature_2m` es de tipo `number` en `Current` y de tipo `string` en `CurrentUnits`?
#. ¿Por qué el archivo tiene la extensión `.ts` y no `.tsx`?
#. ¿Qué sucedería con las interfaces si mañana agrega el indicador `precipitation` a la consulta? ¿Qué tendría que actualizar?
#. Las interfaces se generaron a partir de **una** respuesta de ejemplo. ¿Qué riesgos tiene confiar en ellas sin revisar la documentación de la API?

¿Cómo muestro un indicador del clima con una tarjeta?
-----------------------------------------------------

#. Revise la documentación del componente `Card <https://mui.com/material-ui/react-card/>`_ (MUI, n.d.).
#. Cree el componente funcional `IndicatorUI` en el archivo `src/components/IndicatorUI.tsx`:

   .. dropdown:: IndicatorUI.tsx
      :color: success

      .. code-block:: tsx

         import Card from '@mui/material/Card';
         import CardContent from '@mui/material/CardContent';
         import Typography from '@mui/material/Typography';

         interface IndicatorUIProps {
             title?: string;
             description?: string;
         }

         export default function IndicatorUI(props: IndicatorUIProps) {
             return (
                 <Card>
                     <CardContent sx={{ height: '100%' }}>
                         <Typography variant="h5" component="div">
                             {props.description}
                         </Typography>
                         <Typography variant="body2" component="p" color="text.secondary">
                             {props.title}
                         </Typography>
                     </CardContent>
                 </Card>
             );
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe el componente `IndicatorUI` desde `./components/IndicatorUI`.
   - En la sección **Indicadores**, convierta el `Grid` en un contenedor.
   - Agregue cuatro componentes `IndicatorUI` con los indicadores *Temperatura (2m)*, *Temperatura aparente*, *Velocidad del viento* y *Humedad relativa*, con valores de ejemplo y sus unidades.

   .. code-block:: tsx

      ...
      import IndicatorUI from './components/IndicatorUI';

      function App() {
          return (
              <Grid container ... >

                  ...

                  {/* Indicadores */}
                  <Grid container size={{ xs: 12, md: 9 }}>

                      <Grid size={{ xs: 12, md: 3 }}>
                          <IndicatorUI title='Temperatura (2m)' description='XX °C' />
                      </Grid>

                      <Grid size={{ xs: 12, md: 3 }}>
                          {/* IndicatorUI con la Temperatura aparente en °C */}
                      </Grid>

                      <Grid size={{ xs: 12, md: 3 }}>
                          {/* IndicatorUI con la Velocidad del viento en km/h */}
                      </Grid>

                      <Grid size={{ xs: 12, md: 3 }}>
                          {/* IndicatorUI con la Humedad relativa en % */}
                      </Grid>

                  </Grid>

                  ...
              </Grid>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2,11-29

         ...
         import IndicatorUI from './components/IndicatorUI';

         function App() {
             return (
                 <Grid container ... >

                     ...

                     {/* Indicadores */}
                     <Grid container size={{ xs: 12, md: 9 }}>

                         <Grid size={{ xs: 12, md: 3 }}>
                             <IndicatorUI title='Temperatura (2m)' description='XX °C' />
                         </Grid>

                         <Grid size={{ xs: 12, md: 3 }}>
                             <IndicatorUI title='Temperatura aparente' description='XX °C' />
                         </Grid>

                         <Grid size={{ xs: 12, md: 3 }}>
                             <IndicatorUI title='Velocidad del viento' description='XX km/h' />
                         </Grid>

                         <Grid size={{ xs: 12, md: 3 }}>
                             <IndicatorUI title='Humedad relativa' description='XX %' />
                         </Grid>

                     </Grid>

                     ...
                 </Grid>
             );
         }

#. Compruebe la vista previa del resultado en el navegador, en pantallas pequeñas y medianas.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué función cumplen los componentes `Card` y `CardContent`?
#. ¿Por qué las propiedades `title` y `description` de `IndicatorUIProps` son opcionales? ¿Qué se muestra si omite una de ellas?
#. ¿Por qué el valor del indicador (`description`) se muestra con un estilo tipográfico más grande que su nombre (`title`)?
#. ¿Qué ocurre con la distribución de los cuatro indicadores al convertir el `Grid` de la sección **Indicadores** en contenedor? ¿Cuántas columnas ocupa cada indicador dentro de él?
#. ¿Qué ventaja tiene escribir un solo componente `IndicatorUI` y usarlo cuatro veces con props distintas?

¿Cómo obtengo los datos de la API al cargar el dashboard?
---------------------------------------------------------

#. Revise la documentación del hook `useEffect <https://es.react.dev/reference/react/useEffect>`_ (React, n.d.-a) y de los `hooks personalizados <https://es.react.dev/learn/reusing-logic-with-custom-hooks>`_ (React, n.d.-b).
#. Cree el archivo `src/hooks/useFetchData.ts` con el hook personalizado `useFetchData`:

   - Importe los hooks `useState` y `useEffect` de React.
   - Importe como tipo (`type`) la interfaz `OpenMeteoResponse` desde `../types/DashboardTypes`.
   - Declare que `useFetchData` retorna un objeto del tipo `OpenMeteoResponse` o `null`.

   .. code-block:: typescript

      import { ... , ... } from 'react';
      import { type ... } from '../types/DashboardTypes';

      export default function useFetchData(): ... | ... { }

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript

         import { useEffect, useState } from 'react';
         import { type OpenMeteoResponse } from '../types/DashboardTypes';

         export default function useFetchData(): OpenMeteoResponse | null { }

#. Complete la estructura del hook:

   - Fuera de la función, defina la constante `API_URL` con el :term:`endpoint` de Open-Meteo que obtuvo en la primera actividad.
   - Dentro de la función, declare la variable de estado `data` y su función de actualización `setData`, del tipo `OpenMeteoResponse` o `null`, con el valor inicial `null`.
   - Agregue el hook `useEffect` para que se ejecute **únicamente** después del primer renderizado.
   - Retorne `data` al final del hook.

   .. code-block:: typescript

      ...

      const API_URL = ... ;

      export default function useFetchData(): OpenMeteoResponse | null {

          const [ ... , ... ] = useState< ... >( ... );

          useEffect(() => {
              // Petición asincrónica
          }, ... );

          return ... ;
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 4,8,10-12,14

         import { useEffect, useState } from 'react';
         import { type OpenMeteoResponse } from '../types/DashboardTypes';

         const API_URL = 'https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&timezone=America%2FGuayaquil';

         export default function useFetchData(): OpenMeteoResponse | null {

             const [data, setData] = useState<OpenMeteoResponse | null>(null);

             useEffect(() => {
                 // Petición asincrónica
             }, []); // El arreglo vacío ejecuta el efecto solo después del primer renderizado

             return data;
         }

#. Dentro de la función flecha del `useEffect`, declare y llame a la función asincrónica `obtenerDatos`, que realice la petición a `API_URL` con `fetch` (MDN, n.d.), convierta la respuesta a JSON y actualice el estado `data`:

   .. code-block:: typescript

      useEffect(() => {
          const obtenerDatos = async () => {
              const respuesta = await ... (API_URL);
              const json: OpenMeteoResponse = await respuesta. ... ();
              ... (json);
          };

          obtenerDatos();
      }, []);

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 11-17

         import { useEffect, useState } from 'react';
         import { type OpenMeteoResponse } from '../types/DashboardTypes';

         const API_URL = 'https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&timezone=America%2FGuayaquil';

         export default function useFetchData(): OpenMeteoResponse | null {

             const [data, setData] = useState<OpenMeteoResponse | null>(null);

             useEffect(() => {
                 const obtenerDatos = async () => {
                     const respuesta = await fetch(API_URL);
                     const json: OpenMeteoResponse = await respuesta.json();
                     setData(json);
                 };

                 obtenerDatos();
             }, []); // El arreglo vacío ejecuta el efecto solo después del primer renderizado

             return data;
         }

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué es un *efecto secundario* en React? ¿Por qué una petición a una API se considera un efecto y no se escribe directamente en el cuerpo del componente?
#. ¿Qué diferencia existe entre un componente y un hook personalizado? ¿Por qué el nombre `useFetchData` empieza con `use`?
#. ¿Por qué `data` tiene el tipo `OpenMeteoResponse | null` y comienza con el valor `null`?
#. ¿Por qué se declara la función `obtenerDatos` dentro del efecto, en lugar de escribir `useEffect(async () => { ... })`?
#. Complete la tabla con el comportamiento del efecto según su arreglo de dependencias:

   .. list-table:: Arreglo de dependencias de useEffect
      :header-rows: 1
      :widths: 30 70

      * - Arreglo de dependencias
        - ¿Cuándo se ejecuta el efecto?
      * - ``useEffect(() => { ... })``
        -
      * - ``useEffect(() => { ... }, [])``
        -
      * - ``useEffect(() => { ... }, [ciudad])``
        -

#. ¿Por qué la constante `API_URL` se define fuera de la función del hook? ¿Por qué no se llama simplemente `URL`?

¿Cómo muestro los datos obtenidos en los indicadores?
-----------------------------------------------------

#. En el archivo `src/App.tsx`, importe el hook `useFetchData` y almacene su salida en la constante `dataFetcherOutput`:

   .. code-block:: tsx
      :emphasize-lines: 2,6

      ...
      import useFetchData from './hooks/useFetchData';

      function App() {

          const dataFetcherOutput = useFetchData();

          return ( ... );
      }

#. En el componente `IndicatorUI` del indicador *Temperatura (2m)*, utilice `dataFetcherOutput` para renderizar el indicador **solo si** existen datos, con el valor de `temperature_2m` y su unidad. Revise la estructura del JSON.

   .. code-block:: tsx

      <Grid size={{ xs: 12, md: 3 }}>
          { ... &&
              (<IndicatorUI
                  title='Temperatura (2m)'
                  description={`${ ... } ${ ... }`} />)
          }
      </Grid>

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2-6

         <Grid size={{ xs: 12, md: 3 }}>
             {dataFetcherOutput &&
                 (<IndicatorUI
                     title='Temperatura (2m)'
                     description={`${dataFetcherOutput.current.temperature_2m} ${dataFetcherOutput.current_units.temperature_2m}`} />)
             }
         </Grid>

#. Renderice los demás indicadores con los datos correspondientes de `dataFetcherOutput`.

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx

         {/* Indicadores */}
         <Grid container size={{ xs: 12, md: 9 }}>

             <Grid size={{ xs: 12, md: 3 }}>
                 {dataFetcherOutput &&
                     (<IndicatorUI
                         title='Temperatura (2m)'
                         description={`${dataFetcherOutput.current.temperature_2m} ${dataFetcherOutput.current_units.temperature_2m}`} />)
                 }
             </Grid>

             <Grid size={{ xs: 12, md: 3 }}>
                 {dataFetcherOutput &&
                     (<IndicatorUI
                         title='Temperatura aparente'
                         description={`${dataFetcherOutput.current.apparent_temperature} ${dataFetcherOutput.current_units.apparent_temperature}`} />)
                 }
             </Grid>

             <Grid size={{ xs: 12, md: 3 }}>
                 {dataFetcherOutput &&
                     (<IndicatorUI
                         title='Velocidad del viento'
                         description={`${dataFetcherOutput.current.wind_speed_10m} ${dataFetcherOutput.current_units.wind_speed_10m}`} />)
                 }
             </Grid>

             <Grid size={{ xs: 12, md: 3 }}>
                 {dataFetcherOutput &&
                     (<IndicatorUI
                         title='Humedad relativa'
                         description={`${dataFetcherOutput.current.relative_humidity_2m} ${dataFetcherOutput.current_units.relative_humidity_2m}`} />)
                 }
             </Grid>

         </Grid>

#. Compruebe el resultado en el navegador y revise la petición en la pestaña **Network** de las herramientas de desarrollador.
#. En la pestaña **Network**, active la limitación de red *Slow 4G* (o *3G*), recargue la página y observe qué muestra la sección **Indicadores** mientras llega la respuesta. Luego, desactive la limitación.
#. Elimine temporalmente el arreglo de dependencias `[]` del `useEffect` en `useFetchData`, observe la pestaña **Network** durante unos segundos y vuelva a colocarlo.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Cuántas peticiones a Open-Meteo observó al cargar la página en modo de desarrollo? Investigue qué relación tiene el componente `StrictMode` de `src/main.tsx` con ese comportamiento.
#. ¿Qué mostraba la sección **Indicadores** con la red lenta? ¿Por qué es necesaria la condición `dataFetcherOutput && (...)`?
#. ¿Qué mensaje muestra TypeScript si escribe `dataFetcherOutput.current.temperature_2m` sin la condición? ¿Qué significa?
#. ¿Qué ocurrió en la pestaña **Network** al eliminar el arreglo de dependencias? Explique la secuencia renderizado → efecto → `setData` → renderizado.
#. El hook no informa si la petición falló. ¿Qué estados adicionales agregaría (por ejemplo, de carga y de error) y cómo los mostraría en el dashboard?
#. En la Guía 09, el componente `SelectorUI` guarda la ciudad seleccionada. ¿Qué cambios necesitarían `useFetchData` y su arreglo de dependencias para consultar el clima de la ciudad elegida?
#. Si usó una IA para escribir el hook, ¿cómo comprobó que entiende cuándo y cuántas veces se ejecuta el efecto?

¿Cómo versiono y publico los cambios?
-------------------------------------

#. Versione local y remotamente la rama de desarrollo en el repositorio *dashboard*:

   .. code-block:: bash

      git add .
      git commit -m "Indicadores con datos de Open-Meteo mediante useEffect"
      git push origin desarrollo

#. Genere la solicitud de cambios (*pull request*) hacia la rama principal y apruebe los cambios.
#. Ejecute la transpilación y el despliegue del sitio web:

   .. code-block:: bash

      npm run deploy

   - De ser necesario, elimine, corrija o comente las secciones de código identificadas por el transpilador.
   - Vuelva a ejecutar el comando de despliegue.

#. Compruebe el resultado en el navegador, con la URL: `https://<username>.github.io/dashboard`

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿La petición a Open-Meteo funciona igual en el sitio publicado que en el servidor de desarrollo? ¿Cuántas peticiones observa ahora en la pestaña **Network**?
#. ¿Qué decisiones tomó al integrar `useEffect` en el dashboard y cómo las justificaría ante un compañero de equipo?

Referencias
===========

* MDN. (n.d.). Using the Fetch API - Web APIs. Retrieved November 10, 2026 from https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
* MUI. (n.d.). Card - Material UI. Retrieved November 10, 2026 from https://mui.com/material-ui/react-card/
* Open-Meteo. (n.d.). Weather Forecast API. Retrieved November 10, 2026 from https://open-meteo.com/en/docs
* React. (n.d.-a). useEffect. Retrieved November 10, 2026 from https://es.react.dev/reference/react/useEffect
* React. (n.d.-b). Reutilizar lógica con Hooks personalizados. Retrieved November 10, 2026 from https://es.react.dev/learn/reusing-logic-with-custom-hooks
* Transform Tools. (n.d.). JSON to TypeScript. Retrieved November 10, 2026 from https://transform.tools/json-to-typescript