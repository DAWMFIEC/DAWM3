..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

=========================================================================
Guía 11: React - Visualización de datos y comunicación entre componentes
=========================================================================

.. topic:: Objetivo específico
   :class: objetivo

   Incorporar componentes de gestión y visualización de datos de MUI X al dashboard del clima mediante la comunicación de  componentes personalizados con la actualización dinámica del dashboard a partir de los datos del API de Open-Meteo.

Actividades en clases
=====================

¿Qué datos por hora necesito de Open-Meteo?
-------------------------------------------

#. Acceda a su proyecto *dashboard* en Codespaces o en su máquina local, cámbiese a la rama de desarrollo y levante el servidor:

   .. code-block:: bash

      git checkout desarrollo
      npm install
      npm run dev

#. Acceda al sitio `Open-Meteo API <https://open-meteo.com/en/docs>`_ (Open-Meteo, n.d.) y configure la consulta:

   - En **Location and Time**, use las coordenadas de Guayaquil (latitud ``-2.1962``, longitud ``-79.8862``), la zona horaria *America/Guayaquil* y un día de pronóstico (*Forecast days*: 1).
   - En **Current Weather**, mantenga los indicadores de la guía anterior: `Temperature (2 m)`, `Relative Humidity (2 m)`, `Apparent Temperature` y `Wind Speed (10 m)`.
   - En **Hourly Weather Variables**, seleccione **dos variables por hora**: `Temperature (2 m)` y `Wind Speed (10 m)`.
   - En **Settings**, mantenga las unidades: temperatura en `Celsius °C`, velocidad del viento en `km/h` y precipitación en `Millimeter`.

#. Abra en su navegador la URL generada, que debe ser similar a:

   .. code-block:: text

      https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&hourly=temperature_2m,wind_speed_10m&timezone=America%2FGuayaquil&forecast_days=1

#. Complete la tabla con las nuevas claves del JSON de respuesta:

   .. list-table:: Datos por hora en la respuesta de Open-Meteo
      :header-rows: 1
      :widths: 30 45 25

      * - Clave
        - ¿Qué contiene?
        - Tipo de dato
      * - ``hourly_units``
        -
        -
      * - ``hourly``
        -
        -
      * - ``hourly.time``
        -
        -
      * - ``hourly.temperature_2m``
        -
        -
      * - ``hourly.wind_speed_10m``
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué diferencia existe entre la estructura de ``current`` y la de ``hourly``? ¿Por qué ``hourly`` contiene arreglos?
#. ¿Cuántos elementos tiene cada arreglo de ``hourly``? ¿Qué relación existe entre la posición de un valor en ``hourly.temperature_2m`` y la posición en ``hourly.time``?
#. ¿Qué cambia en la respuesta si retira el parámetro ``forecast_days=1``? ¿Por qué conviene limitarlo para un gráfico?
#. ¿Qué tipo de gráfico y qué tipo de tabla serían adecuados para mostrar dos variables a lo largo de las horas del día?

¿Cómo actualizo las interfaces y la URL del hook?
-------------------------------------------------

#. Acceda a `Transform Tools / JSON to TypeScript <https://transform.tools/json-to-typescript>`_ (Transform Tools, n.d.) y pegue el nuevo JSON de respuesta.
#. Compare el resultado con las interfaces del archivo `src/types/DashboardTypes.ts` y actualícelo:

   - Agregue las interfaces `HourlyUnits` y `Hourly`.
   - Agregue las propiedades `hourly_units` y `hourly` a la interfaz `OpenMeteoResponse`.

   .. code-block:: typescript

      export interface OpenMeteoResponse {
          ...
          current: Current
          hourly_units: ...
          hourly: ...
      }

      ...

      export interface HourlyUnits { ... }

      export interface Hourly { ... }

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 11-12,33-43

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
             hourly_units: HourlyUnits
             hourly: Hourly
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

         export interface HourlyUnits {
             time: string
             temperature_2m: string
             wind_speed_10m: string
         }

         export interface Hourly {
             time: string[]
             temperature_2m: number[]
             wind_speed_10m: number[]
         }

#. En el archivo `src/hooks/useFetchData.ts`, reemplace el valor de la constante `API_URL` por la nueva URL.

   .. code-block:: typescript

      const API_URL = 'https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&hourly=temperature_2m,wind_speed_10m&timezone=America%2FGuayaquil&forecast_days=1';

#. Compruebe que los indicadores de la guía anterior siguen funcionando.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué diferencia existe entre los tipos `number` y `number[]`? ¿Por qué `Hourly.time` es `string[]`?
#. ¿Por qué no fue necesario modificar el hook `useFetchData`, más allá de la URL, para recibir los nuevos datos?
#. ¿Qué error mostraría TypeScript si usa `dataFetcherOutput.hourly` sin haber actualizado la interfaz `OpenMeteoResponse`?

¿Cómo agrego una tabla y un gráfico con MUI X?
----------------------------------------------

#. Instale los paquetes de MUI X para la :term:`gestión y visualización de datos`:

   .. code-block:: bash

      npm install @mui/x-data-grid @mui/x-charts

#. Revise la documentación de `MUI X Data Grid <https://mui.com/x/react-data-grid/>`_ y `MUI X Charts <https://mui.com/x/react-charts/>`_ (MUI, n.d.-a, n.d.-b).
#. Cree el componente funcional `TableUI` en el archivo `src/components/TableUI.tsx`, con datos de ejemplo:

   .. dropdown:: TableUI.tsx
      :color: success

      .. code-block:: tsx

         import Box from '@mui/material/Box';
         import { DataGrid, type GridColDef } from '@mui/x-data-grid';

         function combineArrays(arrLabels: Array<string>, arrValues1: Array<number>, arrValues2: Array<number>) {
             return arrLabels.map((label, index) => ({
                 id: index,
                 label: label,
                 value1: arrValues1[index],
                 value2: arrValues2[index],
             }));
         }

         const columns: GridColDef[] = [
             { field: 'id', headerName: 'ID', width: 90 },
             { field: 'label', headerName: 'Label', width: 125 },
             { field: 'value1', headerName: 'Value 1', width: 125 },
             { field: 'value2', headerName: 'Value 2', width: 125 },
             {
                 field: 'resumen',
                 headerName: 'Resumen',
                 description: 'No es posible ordenar u ocultar esta columna.',
                 sortable: false,
                 hideable: false,
                 width: 100,
                 valueGetter: (_, row) => `${row.label || ''} ${row.value1 || ''} ${row.value2 || ''}`,
             },
         ];

         const arrValues1 = [4000, 3000, 2000, 2780, 1890, 2390, 3490];
         const arrValues2 = [2400, 1398, 9800, 3908, 4800, 3800, 4300];
         const arrLabels = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];

         export default function TableUI() {

             const rows = combineArrays(arrLabels, arrValues1, arrValues2);

             return (
                 <Box sx={{ height: 350, width: '100%' }}>
                     <DataGrid
                         rows={rows}
                         columns={columns}
                         initialState={{
                             pagination: {
                                 paginationModel: {
                                     pageSize: 5,
                                 },
                             },
                         }}
                         pageSizeOptions={[5]}
                         disableRowSelectionOnClick
                     />
                 </Box>
             );
         }

#. Cree el componente funcional `ChartUI` en el archivo `src/components/ChartUI.tsx`, con datos de ejemplo:

   .. dropdown:: ChartUI.tsx
      :color: success

      .. code-block:: tsx

         import { LineChart } from '@mui/x-charts/LineChart';
         import Typography from '@mui/material/Typography';

         const arrValues1 = [4000, 3000, 2000, 2780, 1890, 2390, 3490];
         const arrValues2 = [2400, 1398, 9800, 3908, 4800, 3800, 4300];
         const arrLabels = ['A', 'B', 'C', 'D', 'E', 'F', 'G'];

         export default function ChartUI() {
             return (
                 <>
                     <Typography variant="h5" component="div">
                         Chart arrLabels vs arrValues1 & arrValues2
                     </Typography>
                     <LineChart
                         height={300}
                         series={[
                             { data: arrValues1, label: 'value1' },
                             { data: arrValues2, label: 'value2' },
                         ]}
                         xAxis={[{ scaleType: 'point', data: arrLabels }]}
                     />
                 </>
             );
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe los componentes `TableUI` y `ChartUI`.
   - Use `ChartUI` en la sección **Gráfico** y `TableUI` en la sección **Tabla**.

   .. code-block:: tsx

      ...
      import ... from './components/TableUI';
      import ... from './components/ChartUI';

      function App() {
          ...
          return (
              <Grid container ... >

                  ...

                  {/* Gráfico */}
                  <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                      ...
                  </Grid>

                  {/* Tabla */}
                  <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                      ...
                  </Grid>

                  ...
              </Grid>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2-3,14,19

         ...
         import TableUI from './components/TableUI';
         import ChartUI from './components/ChartUI';

         function App() {
             ...
             return (
                 <Grid container ... >

                     ...

                     {/* Gráfico */}
                     <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                         <ChartUI />
                     </Grid>

                     {/* Tabla */}
                     <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                         <TableUI />
                     </Grid>

                     ...
                 </Grid>
             );
         }

#. Compruebe la vista previa del resultado en el navegador. Ordene, filtre y pagine la tabla, y pase el cursor sobre el gráfico.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué diferencia existe entre la biblioteca MUI (`@mui/material`) y MUI X (`@mui/x-data-grid`, `@mui/x-charts`)? ¿Qué componentes de MUI X requieren licencia?
#. ¿Qué hace la función `combineArrays`? ¿Por qué cada fila necesita la propiedad `id`?
#. ¿Qué describe cada objeto del arreglo `columns`? ¿Qué hace la función `valueGetter` de la columna *Resumen*?
#. ¿Qué indican los props `series` y `xAxis` del componente `LineChart`? ¿Qué significa `scaleType: 'point'`?
#. ¿Para qué sirve el fragmento `<> ... </>` en `ChartUI`?
#. ¿Por qué los componentes todavía no son reutilizables? ¿Qué tendría que cambiar para que muestren otros datos?

¿Cómo muestro los datos por hora en la tabla y el gráfico?
----------------------------------------------------------

#. Modifique el componente `TableUI` para que reciba los datos por props:

   - Defina la interfaz `TableUIProps` con las propiedades `labels` (arreglo de textos), `values1` y `values2` (arreglos de números), y `header1` y `header2` (textos).
   - Mueva el arreglo `columns` dentro del componente, con las columnas *Hora*, `header1` y `header2`.
   - Elimine los arreglos de ejemplo y construya las filas con los props.

   .. code-block:: tsx

      ...

      interface TableUIProps {
          labels: ... ;
          values1: ... ;
          values2: ... ;
          header1: ... ;
          header2: ... ;
      }

      function combineArrays( ... ) { ... }

      export default function TableUI({ ... }: TableUIProps) {

          const columns: GridColDef[] = [
              { field: 'label', headerName: 'Hora', width: 160 },
              { field: 'value1', headerName: ... , width: 160 },
              { field: 'value2', headerName: ... , width: 160 },
          ];

          const rows = combineArrays( ... , ... , ... );

          return ( ... );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 4-10,21,23-27,29

         import Box from '@mui/material/Box';
         import { DataGrid, type GridColDef } from '@mui/x-data-grid';

         interface TableUIProps {
             labels: string[];
             values1: number[];
             values2: number[];
             header1: string;
             header2: string;
         }

         function combineArrays(arrLabels: Array<string>, arrValues1: Array<number>, arrValues2: Array<number>) {
             return arrLabels.map((label, index) => ({
                 id: index,
                 label: label,
                 value1: arrValues1[index],
                 value2: arrValues2[index],
             }));
         }

         export default function TableUI({ labels, values1, values2, header1, header2 }: TableUIProps) {

             const columns: GridColDef[] = [
                 { field: 'label', headerName: 'Hora', width: 160 },
                 { field: 'value1', headerName: header1, width: 160 },
                 { field: 'value2', headerName: header2, width: 160 },
             ];

             const rows = combineArrays(labels, values1, values2);

             return (
                 <Box sx={{ height: 350, width: '100%' }}>
                     <DataGrid
                         rows={rows}
                         columns={columns}
                         initialState={{
                             pagination: {
                                 paginationModel: {
                                     pageSize: 5,
                                 },
                             },
                         }}
                         pageSizeOptions={[5]}
                         disableRowSelectionOnClick
                     />
                 </Box>
             );
         }

#. Modifique el componente `ChartUI` para que reciba por props el título (`title`), las etiquetas del eje X (`labels`), las dos series de valores (`values1`, `values2`) y sus nombres (`label1`, `label2`):

   .. code-block:: tsx

      ...

      interface ChartUIProps {
          ...
      }

      export default function ChartUI({ ... }: ChartUIProps) {
          return (
              <>
                  <Typography variant="h5" component="div">{ ... }</Typography>
                  <LineChart
                      height={300}
                      series={[
                          { data: ... , label: ... },
                          { data: ... , label: ... },
                      ]}
                      xAxis={[{ scaleType: 'point', data: ... }]}
                  />
              </>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx

         import { LineChart } from '@mui/x-charts/LineChart';
         import Typography from '@mui/material/Typography';

         interface ChartUIProps {
             title: string;
             labels: string[];
             values1: number[];
             values2: number[];
             label1: string;
             label2: string;
         }

         export default function ChartUI({ title, labels, values1, values2, label1, label2 }: ChartUIProps) {
             return (
                 <>
                     <Typography variant="h5" component="div">{title}</Typography>
                     <LineChart
                         height={300}
                         series={[
                             { data: values1, label: label1 },
                             { data: values2, label: label2 },
                         ]}
                         xAxis={[{ scaleType: 'point', data: labels }]}
                     />
                 </>
             );
         }

#. En `src/App.tsx`, pase a `ChartUI` y `TableUI` los datos de `dataFetcherOutput.hourly` y sus unidades de `dataFetcherOutput.hourly_units`, solo si existen datos. Para las etiquetas, muestre solo la hora (por ejemplo, ``14:00``) a partir de `hourly.time`.

   .. code-block:: tsx

      {/* Gráfico */}
      <Grid ... >
          {dataFetcherOutput && (
              <ChartUI
                  title='Temperatura y viento por hora'
                  labels={dataFetcherOutput.hourly.time.map((tiempo) => tiempo. ... )}
                  values1={ ... }
                  values2={ ... }
                  label1={`Temperatura (${ ... })`}
                  label2={`Viento (${ ... })`} />
          )}
      </Grid>

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx

         {/* Gráfico */}
         <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
             {dataFetcherOutput && (
                 <ChartUI
                     title='Temperatura y viento por hora'
                     labels={dataFetcherOutput.hourly.time.map((tiempo) => tiempo.slice(11))}
                     values1={dataFetcherOutput.hourly.temperature_2m}
                     values2={dataFetcherOutput.hourly.wind_speed_10m}
                     label1={`Temperatura (${dataFetcherOutput.hourly_units.temperature_2m})`}
                     label2={`Viento (${dataFetcherOutput.hourly_units.wind_speed_10m})`} />
             )}
         </Grid>

         {/* Tabla */}
         <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
             {dataFetcherOutput && (
                 <TableUI
                     labels={dataFetcherOutput.hourly.time.map((tiempo) => tiempo.slice(11))}
                     values1={dataFetcherOutput.hourly.temperature_2m}
                     values2={dataFetcherOutput.hourly.wind_speed_10m}
                     header1={`Temperatura (${dataFetcherOutput.hourly_units.temperature_2m})`}
                     header2={`Viento (${dataFetcherOutput.hourly_units.wind_speed_10m})`} />
             )}
         </Grid>

#. Compruebe la vista previa del resultado en el navegador y compare los valores de la tabla con los del gráfico y con el JSON de la API.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Por qué el arreglo `columns` se movió dentro del componente `TableUI`?
#. ¿Qué devuelve `tiempo.slice(11)` para el valor ``2026-11-16T14:00``? ¿Qué otra forma conoce para dar formato a una fecha?
#. ¿Por qué se usa la condición `dataFetcherOutput && (...)` antes de renderizar `ChartUI` y `TableUI`?
#. Ahora `ChartUI` y `TableUI` reciben datos por props. ¿Qué componente es el *padre* y cuáles son los *hijos*? ¿En qué dirección viajan los datos?
#. ¿Qué otras variables por hora podría mostrar con los mismos componentes sin modificar su código?

¿Cómo informo al usuario mientras cargan los datos o si ocurre un error?
------------------------------------------------------------------------

#. Modifique el hook `useFetchData` para que, además de los datos, informe si la petición está en curso o si ocurrió un error:

   - Defina la interfaz `FetchDataOutput` con las propiedades `data` (`OpenMeteoResponse` o `null`), `loading` (booleano) y `error` (texto o `null`).
   - Agregue las variables de estado `loading` (valor inicial `true`) y `error` (valor inicial `null`).
   - En `obtenerDatos`, use un bloque `try...catch...finally`: lance un error si la respuesta no es exitosa (`respuesta.ok`), guarde el mensaje en `error` si algo falla y asigne `false` a `loading` al terminar.
   - Retorne un objeto con `data`, `loading` y `error`.

   .. code-block:: typescript

      ...

      interface FetchDataOutput {
          data: ... ;
          loading: ... ;
          error: ... ;
      }

      export default function useFetchData(): FetchDataOutput {

          const [data, setData] = useState<OpenMeteoResponse | null>(null);
          const [loading, setLoading] = useState<boolean>( ... );
          const [error, setError] = useState<string | null>( ... );

          useEffect(() => {
              const obtenerDatos = async () => {
                  try {
                      const respuesta = await fetch(API_URL);
                      if (! ... ) {
                          throw new Error( ... );
                      }
                      const json: OpenMeteoResponse = await respuesta.json();
                      setData(json);
                  } catch (e) {
                      setError( ... );
                  } finally {
                      setLoading( ... );
                  }
              };

              obtenerDatos();
          }, []);

          return { ... };
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 6-10,12,15-16,21-23,27-31,37

         import { useEffect, useState } from 'react';
         import { type OpenMeteoResponse } from '../types/DashboardTypes';

         const API_URL = 'https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&hourly=temperature_2m,wind_speed_10m&timezone=America%2FGuayaquil&forecast_days=1';

         interface FetchDataOutput {
             data: OpenMeteoResponse | null;
             loading: boolean;
             error: string | null;
         }

         export default function useFetchData(): FetchDataOutput {

             const [data, setData] = useState<OpenMeteoResponse | null>(null);
             const [loading, setLoading] = useState<boolean>(true);
             const [error, setError] = useState<string | null>(null);

             useEffect(() => {
                 const obtenerDatos = async () => {
                     try {
                         const respuesta = await fetch(API_URL);
                         if (!respuesta.ok) {
                             throw new Error(`Error ${respuesta.status} al consultar Open-Meteo`);
                         }
                         const json: OpenMeteoResponse = await respuesta.json();
                         setData(json);
                     } catch (e) {
                         setError((e as Error).message);
                     } finally {
                         setLoading(false);
                     }
                 };

                 obtenerDatos();
             }, []);

             return { data, loading, error };
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe los componentes `CircularProgress <https://mui.com/material-ui/react-progress/>`_ y `Alert` de MUI (MUI, n.d.-c).
   - Reemplace `const dataFetcherOutput = useFetchData();` por la desestructuración `const { data, loading, error } = useFetchData();`.
   - En la sección **Indicadores**, muestre un `CircularProgress` mientras `loading` sea verdadero y un `Alert` de tipo error si existe `error`.
   - Reemplace `dataFetcherOutput` por `data` en los indicadores, el gráfico y la tabla.

   .. code-block:: tsx

      ...
      import CircularProgress from '@mui/material/CircularProgress';
      import Alert from '@mui/material/Alert';

      function App() {

          const { ... , ... , ... } = useFetchData();

          return (
              <Grid container ... >

                  ...

                  {/* Indicadores */}
                  <Grid container size={{ xs: 12, md: 9 }}>

                      { ... && <CircularProgress />}
                      { ... && <Alert severity="error">{ ... }</Alert>}

                      <Grid size={{ xs: 12, md: 3 }}>
                          {data &&
                              (<IndicatorUI
                                  title='Temperatura (2m)'
                                  description={`${data.current.temperature_2m} ${data.current_units.temperature_2m}`} />)
                          }
                      </Grid>
                      ...

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2-3,7,17-18,21,24

         ...
         import CircularProgress from '@mui/material/CircularProgress';
         import Alert from '@mui/material/Alert';

         function App() {

             const { data, loading, error } = useFetchData();

             return (
                 <Grid container ... >

                     ...

                     {/* Indicadores */}
                     <Grid container size={{ xs: 12, md: 9 }}>

                         {loading && <CircularProgress />}
                         {error && <Alert severity="error">{error}</Alert>}

                         <Grid size={{ xs: 12, md: 3 }}>
                             {data &&
                                 (<IndicatorUI
                                     title='Temperatura (2m)'
                                     description={`${data.current.temperature_2m} ${data.current_units.temperature_2m}`} />)
                             }
                         </Grid>

                         ...
                     </Grid>

                     ...
                 </Grid>
             );
         }

#. Compruebe el resultado con la limitación de red *Slow 4G* de la pestaña **Network** de las herramientas de desarrollador.
#. Cambie temporalmente la URL de `API_URL` por una con un parámetro inválido (por ejemplo, ``latitude=200``), observe el resultado y luego revierta el cambio.
#. Active temporalmente el modo *Offline* de la pestaña **Network**, recargue la página, observe el resultado y vuelva al modo normal.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Por qué el hook retorna un objeto con tres propiedades en lugar de solo los datos? ¿Qué ventaja tiene la desestructuración `{ data, loading, error }`?
#. Complete la tabla con los valores de los estados en cada momento:

   .. list-table:: Estados del hook useFetchData
      :header-rows: 1
      :widths: 34 22 22 22

      * - Momento
        - ``data``
        - ``loading``
        - ``error``
      * - Primer renderizado
        -
        -
        -
      * - Respuesta exitosa
        -
        -
        -
      * - Respuesta con parámetro inválido
        -
        -
        -
      * - Sin conexión (*Offline*)
        -
        -
        -

#. ¿Qué mensaje obtuvo con ``latitude=200`` y qué mensaje obtuvo sin conexión? ¿Por qué `fetch` no lanza un error por sí solo cuando la API responde con un código 400?
#. ¿Para qué sirve el bloque `finally`? ¿Qué ocurriría si `setLoading(false)` se escribiera solo dentro del `try`?
#. ¿Qué otros componentes de MUI (por ejemplo, `Skeleton`) podría usar para indicar la carga de la tabla y el gráfico?

¿Cómo comunico la ciudad seleccionada al resto del dashboard?
-------------------------------------------------------------

#. Analice el escenario de interacción en el dashboard:

   - El usuario selecciona una ciudad de la lista de opciones (`SelectorUI`).
   - El sistema realiza una petición asincrónica a la API de Open-Meteo con las coordenadas de la ciudad seleccionada (`useFetchData`).
   - Con la respuesta de la API, el sistema actualiza los indicadores, la tabla y el gráfico (`IndicatorUI`, `TableUI` y `ChartUI`).

#. Modifique el componente `src/components/SelectorUI.tsx`:

   - Defina la interfaz `SelectorProps` con la propiedad `onOptionSelect`, una función que recibe un texto y no retorna nada.
   - Reciba el prop `onOptionSelect` en el componente.
   - En `handleChange`, guarde el valor seleccionado en la constante `selectedValue`, actualice el estado `cityInput` y comunique el valor al componente padre con `onOptionSelect`.

   .. code-block:: tsx

      ...

      interface SelectorProps {
          onOptionSelect: ( ... ) => ... ;
      }

      export default function SelectorUI({ ... }: SelectorProps) {

          const [cityInput, setCityInput] = useState('');

          const handleChange = (event: SelectChangeEvent<string>) => {
              const selectedValue = event.target.value;
              setCityInput(selectedValue);
              ... (selectedValue);
          };

          ...
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 7-9,11,16-18

         import { useState } from 'react';
         import FormControl from '@mui/material/FormControl';
         import InputLabel from '@mui/material/InputLabel';
         import Select, { type SelectChangeEvent } from '@mui/material/Select';
         import MenuItem from '@mui/material/MenuItem';

         interface SelectorProps {
             onOptionSelect: (option: string) => void;
         }

         export default function SelectorUI({ onOptionSelect }: SelectorProps) {

             const [cityInput, setCityInput] = useState('');

             const handleChange = (event: SelectChangeEvent<string>) => {
                 const selectedValue = event.target.value;
                 setCityInput(selectedValue);
                 onOptionSelect(selectedValue);
             };

             return (
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
                         <MenuItem value={'manta'}>Manta</MenuItem>
                         <MenuItem value={'cuenca'}>Cuenca</MenuItem>
                     </Select>

                     {cityInput && (
                         <p>
                             Información del clima en <span style={{ textTransform: 'capitalize', fontWeight: 'bold' }}>{cityInput}</span>
                         </p>
                     )}
                 </FormControl>
             );
         }

#. Modifique el hook `src/hooks/useFetchData.ts`:

   - Reemplace la constante `API_URL` por el objeto `CITY_COORDS`, que asocia cada opción del selector con sus coordenadas.
   - Agregue el parámetro `selectedOption` de tipo `string | null`.
   - Dentro del efecto, obtenga las coordenadas de la ciudad seleccionada (Guayaquil por defecto), construya la URL con ellas y reinicie `loading` y `error` antes de cada petición.
   - Agregue `selectedOption` al arreglo de dependencias del `useEffect`.

   .. code-block:: typescript

      ...

      const CITY_COORDS: Record<string, { latitude: number; longitude: number }> = {
          guayaquil: { latitude: -2.1962, longitude: -79.8862 },
          quito: { ... },
          manta: { ... },
          cuenca: { ... },
      };

      ...

      export default function useFetchData(selectedOption: ... ): FetchDataOutput {

          ...

          useEffect(() => {
              const cityConfig = selectedOption != null ? CITY_COORDS[ ... ] : CITY_COORDS['guayaquil'];
              const url = `https://api.open-meteo.com/v1/forecast?latitude=${ ... }&longitude=${ ... }&current=...`;

              const obtenerDatos = async () => {
                  setLoading( ... );
                  setError( ... );
                  ...
              };

              obtenerDatos();
          }, [ ... ]);

          ...
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: typescript
         :emphasize-lines: 4-9,17,24-25,28-29,31,45

         import { useEffect, useState } from 'react';
         import { type OpenMeteoResponse } from '../types/DashboardTypes';

         const CITY_COORDS: Record<string, { latitude: number; longitude: number }> = {
             guayaquil: { latitude: -2.1962, longitude: -79.8862 },
             quito: { latitude: -0.2299, longitude: -78.5249 },
             manta: { latitude: -0.9677, longitude: -80.7089 },
             cuenca: { latitude: -2.9005, longitude: -79.0045 },
         };

         interface FetchDataOutput {
             data: OpenMeteoResponse | null;
             loading: boolean;
             error: string | null;
         }

         export default function useFetchData(selectedOption: string | null): FetchDataOutput {

             const [data, setData] = useState<OpenMeteoResponse | null>(null);
             const [loading, setLoading] = useState<boolean>(true);
             const [error, setError] = useState<string | null>(null);

             useEffect(() => {
                 const cityConfig = selectedOption != null ? CITY_COORDS[selectedOption] : CITY_COORDS['guayaquil'];
                 const url = `https://api.open-meteo.com/v1/forecast?latitude=${cityConfig.latitude}&longitude=${cityConfig.longitude}&current=temperature_2m,relative_humidity_2m,apparent_temperature,wind_speed_10m&hourly=temperature_2m,wind_speed_10m&timezone=America%2FGuayaquil&forecast_days=1`;

                 const obtenerDatos = async () => {
                     setLoading(true);
                     setError(null);
                     try {
                         const respuesta = await fetch(url);
                         if (!respuesta.ok) {
                             throw new Error(`Error ${respuesta.status} al consultar Open-Meteo`);
                         }
                         const json: OpenMeteoResponse = await respuesta.json();
                         setData(json);
                     } catch (e) {
                         setError((e as Error).message);
                     } finally {
                         setLoading(false);
                     }
                 };

                 obtenerDatos();
             }, [selectedOption]); // El efecto se ejecuta cada vez que cambia la opción seleccionada

             return { data, loading, error };
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe el hook `useState`.
   - Declare la variable de estado `selectedOption` (texto o `null`, con valor inicial `null`) y su función de actualización `setSelectedOption`.
   - Pase `selectedOption` al hook `useFetchData`.
   - Pase la función `setSelectedOption` al prop `onOptionSelect` de `SelectorUI`.

   .. code-block:: tsx

      import { ... } from 'react';
      ...

      function App() {

          const [ ... , ... ] = useState<string | null>( ... );

          const { data, loading, error } = useFetchData( ... );

          return (
              ...
              <SelectorUI onOptionSelect={ ... } />
              ...
          );
      }

   .. dropdown:: Solución (App.tsx completo)
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2,16,18,36

         import './App.css';
         import { useState } from 'react';
         import { Grid } from '@mui/material';
         import CircularProgress from '@mui/material/CircularProgress';
         import Alert from '@mui/material/Alert';
         import HeaderUI from './components/HeaderUI';
         import AlertUI from './components/AlertUI';
         import SelectorUI from './components/SelectorUI';
         import IndicatorUI from './components/IndicatorUI';
         import ChartUI from './components/ChartUI';
         import TableUI from './components/TableUI';
         import useFetchData from './hooks/useFetchData';

         function App() {

             const [selectedOption, setSelectedOption] = useState<string | null>(null);

             const { data, loading, error } = useFetchData(selectedOption);

             return (
                 <Grid container spacing={5} sx={{ justifyContent: 'center', alignItems: 'center' }}>

                     {/* Encabezado */}
                     <Grid size={{ xs: 12, md: 12 }}>
                         <HeaderUI />
                     </Grid>

                     {/* Alertas */}
                     <Grid size={{ xs: 12, md: 12 }}
                         container sx={{ justifyContent: 'flex-end', alignItems: 'center' }}>
                         <AlertUI description="No se prevén lluvias" />
                     </Grid>

                     {/* Selector */}
                     <Grid size={{ xs: 12, md: 3 }}>
                         <SelectorUI onOptionSelect={setSelectedOption} />
                     </Grid>

                     {/* Indicadores */}
                     <Grid container size={{ xs: 12, md: 9 }}>

                         {loading && <CircularProgress />}
                         {error && <Alert severity="error">{error}</Alert>}

                         {data && (
                             <>
                                 <Grid size={{ xs: 12, md: 3 }}>
                                     <IndicatorUI
                                         title='Temperatura (2m)'
                                         description={`${data.current.temperature_2m} ${data.current_units.temperature_2m}`} />
                                 </Grid>

                                 <Grid size={{ xs: 12, md: 3 }}>
                                     <IndicatorUI
                                         title='Temperatura aparente'
                                         description={`${data.current.apparent_temperature} ${data.current_units.apparent_temperature}`} />
                                 </Grid>

                                 <Grid size={{ xs: 12, md: 3 }}>
                                     <IndicatorUI
                                         title='Velocidad del viento'
                                         description={`${data.current.wind_speed_10m} ${data.current_units.wind_speed_10m}`} />
                                 </Grid>

                                 <Grid size={{ xs: 12, md: 3 }}>
                                     <IndicatorUI
                                         title='Humedad relativa'
                                         description={`${data.current.relative_humidity_2m} ${data.current_units.relative_humidity_2m}`} />
                                 </Grid>
                             </>
                         )}

                     </Grid>

                     {/* Gráfico */}
                     <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                         {data && (
                             <ChartUI
                                 title='Temperatura y viento por hora'
                                 labels={data.hourly.time.map((tiempo) => tiempo.slice(11))}
                                 values1={data.hourly.temperature_2m}
                                 values2={data.hourly.wind_speed_10m}
                                 label1={`Temperatura (${data.hourly_units.temperature_2m})`}
                                 label2={`Viento (${data.hourly_units.wind_speed_10m})`} />
                         )}
                     </Grid>

                     {/* Tabla */}
                     <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                         {data && (
                             <TableUI
                                 labels={data.hourly.time.map((tiempo) => tiempo.slice(11))}
                                 values1={data.hourly.temperature_2m}
                                 values2={data.hourly.wind_speed_10m}
                                 header1={`Temperatura (${data.hourly_units.temperature_2m})`}
                                 header2={`Viento (${data.hourly_units.wind_speed_10m})`} />
                         )}
                     </Grid>

                     {/* Información adicional */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Información adicional</Grid>

                 </Grid>
             );
         }

         export default App;

#. Compruebe el resultado en el navegador: seleccione distintas ciudades y observe las peticiones en la pestaña **Network**.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Dibuje el recorrido de los datos desde que el usuario elige una ciudad hasta que se actualiza el gráfico. ¿Qué componente o hook interviene en cada paso?
#. ¿Por qué la variable de estado `selectedOption` se declara en `App` y no en `SelectorUI`? Investigue el concepto de *levantar el estado* (React, n.d.).
#. El prop `onOptionSelect` es una función. ¿Qué ocurre cuando `SelectorUI` la llama? ¿Por qué se puede pasar directamente `setSelectedOption`?
#. ¿Qué cambió en el comportamiento del efecto al agregar `selectedOption` al arreglo de dependencias? ¿Cuántas peticiones observa al cambiar de ciudad?
#. ¿Qué ventaja tiene el objeto `CITY_COORDS` frente a escribir una URL distinta para cada ciudad? ¿Qué pasos seguiría para agregar una nueva ciudad?
#. ¿Qué ocurriría si `selectedOption` tuviera un valor que no existe en `CITY_COORDS`? ¿Cómo protegería el código ante ese caso?
#. Si una IA le propone otra forma de comunicar los componentes (por ejemplo, con *Context*), ¿qué criterios usaría para decidir si es adecuada para este dashboard?

¿Cómo versiono y publico los cambios?
-------------------------------------

#. Versione local y remotamente la rama de desarrollo en el repositorio *dashboard*:

   .. code-block:: bash

      git add .
      git commit -m "Tabla y gráfico con MUI X y comunicación entre componentes"
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

#. ¿Qué criterios técnicos consideró para decidir que una tabla y un gráfico de líneas son adecuados para los datos por hora?
#. ¿Cómo aseguró la coherencia visual y funcional entre los componentes sugeridos por una IA y los desarrollados por usted?

Referencias
===========

* MUI. (n.d.-a). Data Grid - MUI X. Retrieved November 16, 2026 from https://mui.com/x/react-data-grid/
* MUI. (n.d.-b). Charts - MUI X. Retrieved November 16, 2026 from https://mui.com/x/react-charts/
* MUI. (n.d.-c). Progress - Material UI. Retrieved November 16, 2026 from https://mui.com/material-ui/react-progress/
* Open-Meteo. (n.d.). Weather Forecast API. Retrieved November 16, 2026 from https://open-meteo.com/en/docs
* React. (n.d.). Compartir estado entre componentes. Retrieved November 16, 2026 from https://es.react.dev/learn/sharing-state-between-components
* Transform Tools. (n.d.). JSON to TypeScript. Retrieved November 16, 2026 from https://transform.tools/json-to-typescript