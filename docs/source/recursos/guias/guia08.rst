..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

==============================================
Guía 08: Dashboard del clima con React y MUI
==============================================

.. topic:: Objetivo específico
   :class: objetivo

   Construir la estructura inicial de un dashboard del clima con React, TypeScript y MUI, mediante componentes funcionales reutilizables que se comunican con props tipadas por interfaces, para obtener una interfaz modular, responsiva y desplegada en GitHub Pages.

Actividades en clases
=====================

¿Qué datos ofrece la API de Open-Meteo para mi dashboard?
---------------------------------------------------------

#. Explore la documentación de la `API de Open-Meteo <https://open-meteo.com/en/docs>`_ (Open-Meteo, n.d.) para comprender cómo se construye una solicitud de datos meteorológicos.
#. Abra una pestaña en su navegador (o use cURL desde la terminal) y realice la siguiente solicitud para la ciudad de Guayaquil:

   .. code-block:: bash

      curl "https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&current=temperature_2m,relative_humidity_2m,weather_code&hourly=temperature_2m,precipitation&daily=temperature_2m_max,temperature_2m_min,uv_index_max&timezone=America%2FGuayaquil"

#. Modifique la solicitud anterior con una latitud inválida (por ejemplo, ``latitude=200``) y observe la respuesta de la API.
#. Complete la tabla con los elementos clave de la API que serán útiles para el :term:`dashboard`:

   .. list-table:: Elementos de la API de Open-Meteo
      :header-rows: 1
      :widths: 25 45 30

      * - Elemento
        - ¿Qué encontró en la documentación?
        - Ejemplo
      * - Parámetros de entrada (*input*)
        -
        -
      * - Variables del clima actual (``current``)
        -
        -
      * - Variables por hora (``hourly``)
        -
        -
      * - Variables por día (``daily``)
        -
        -
      * - Formato de respuesta (*response*)
        -
        -
      * - Codificación del clima (``weather_code``)
        -
        -
      * - Respuesta ante un error
        -
        -

#. Relacione cada elemento del dashboard con las variables de la API que necesitará:

   .. list-table:: Casos de uso del dashboard
      :header-rows: 1
      :widths: 25 35 40

      * - Elemento del dashboard
        - Variable(s) de la API
        - ¿Cómo lo mostraría?
      * - Clima actual (temperatura, humedad, estado del cielo)
        -
        -
      * - Gráfico de evolución (temperatura por hora, precipitación)
        -
        -
      * - Pronóstico extendido (próximos 7 días)
        -
        -
      * - Alerta visual (``uv_index_max`` sobre un umbral)
        -
        -
      * - Íconos adaptativos
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué parámetros son obligatorios para que la API responda correctamente y cuáles son opcionales?
#. ¿Por qué es importante el parámetro ``timezone`` al consultar datos por hora o por día?
#. ¿Qué estructura tiene la respuesta JSON? ¿Qué relación existe entre los arreglos ``hourly.time`` y ``hourly.temperature_2m``?
#. ¿Qué información devuelve la API cuando la solicitud es inválida? ¿Cómo debería reaccionar el dashboard ante esa respuesta?
#. ¿Por qué la API devuelve el estado del cielo como un número (``weather_code``) y no como un texto?

¿Cómo preparo el repositorio del dashboard?
-------------------------------------------

#. Cree un repositorio en `GitHub <https://github.com>`_ con el nombre *dashboard*.

   - Agregue un archivo `README.md` con el título de su dashboard y una breve descripción del objetivo del proyecto.
   - Agregue un archivo `.gitignore` con la plantilla de *Node*.

#. Acceda a su proyecto *dashboard* en Codespaces o clónelo en su máquina local.
#. Cree y cámbiese a una rama de desarrollo:

   .. code-block:: bash

      git checkout -b desarrollo

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué archivos y carpetas excluye la plantilla de `.gitignore` para *Node*? ¿Por qué no se versiona la carpeta `node_modules`?
#. ¿Qué ventajas tiene trabajar en una rama de desarrollo en lugar de hacerlo directamente en la rama principal?

¿Cómo creo un proyecto React con TypeScript usando Vite?
--------------------------------------------------------

#. Explore la documentación de `React <https://react.dev/>`_ (React, n.d.) para comprender los conceptos básicos de esta biblioteca.
#. Dentro de la carpeta de su proyecto, abra la terminal y cree un nuevo proyecto con `Vite <https://vitejs.dev/guide/#scaffolding-your-first-vite-project>`_ (Vite, n.d.):

   .. code-block:: bash

      npm create vite@latest .

#. Seleccione el framework :term:`React` y la variante `TypeScript`.
#. Instale las dependencias del proyecto e inicie el servidor de desarrollo:

   .. code-block:: bash

      npm install
      npm run dev

#. Compruebe la vista previa del resultado en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Investigue la utilidad de los archivos en la tabla:

   .. list-table:: Archivos del proyecto
      :header-rows: 1
      :widths: 25 75

      * - Archivo
        - ¿Para qué sirve?
      * - ``index.html``
        -
      * - ``src/main.tsx``
        -
      * - ``src/App.tsx``
        -
      * - ``src/index.css``
        -
      * - ``package.json``
        -
      * - ``vite.config.ts``
        -
      * - ``tsconfig.json``
        -

#. Compare este proyecto con *TubeTrack* (Guía 07). ¿Qué diferencia existe entre los archivos `src/main.ts` y `src/main.tsx`? ¿Qué indica la extensión `.tsx`?
#. ¿Qué significa que React implemente la técnica de :term:`Client Side Rendering (CSR)`? ¿Qué hace el navegador al cargar la página?
#. ¿Qué hace el comando `npm run dev`? ¿Dónde está definido?

¿Cómo modifico el componente principal de la aplicación?
--------------------------------------------------------

#. Reemplace el contenido del archivo `src/App.tsx` para mostrar un mensaje de bienvenida:

   - Elimine las importaciones que ya no se usarán (`useState` y los logos).
   - Mantenga la importación de `./App.css`.
   - Haga que la función `App` devuelva un `div` con un encabezado `h1` con el texto *Bienvenido al Dashboard*.

   .. code-block:: tsx

      import './App.css';

      function App() {
          return (
              ...
          );
      }

      export default App;

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 5-7

         import './App.css';

         function App() {
             return (
                 <div>
                     <h1>Bienvenido al Dashboard</h1>
                 </div>
             );
         }

         export default App;

#. Compruebe la vista previa del resultado en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué es :term:`JSX`? ¿Por qué se puede escribir código similar a HTML dentro de una función de TypeScript?
#. En *TubeTrack* (Guía 07) se usó `innerHTML` con una plantilla de texto. ¿Qué diferencia encuentra con devolver JSX desde la función `App`?
#. Revise `src/main.tsx`. ¿Cómo llega el componente `App` a mostrarse dentro del elemento `#root` de `index.html`?
#. ¿Por qué es necesario `export default App`? ¿Qué archivo lo importa?

¿Cómo organizo el dashboard en una cuadrícula con MUI?
------------------------------------------------------

#. Explore la documentación de `MUI <https://mui.com/material-ui/getting-started/overview/>`_ (MUI, n.d.-a) para comprender cómo integrar esta biblioteca en un proyecto de React.
#. Instale MUI y sus dependencias:

   .. code-block:: bash

      npm install @mui/material @emotion/react @emotion/styled

#. Modifique el archivo `src/App.tsx`:

   - Importe el componente `Grid` desde `@mui/material`.
   - Reemplace el `div` por un `Grid` que contenga un `Grid` por cada elemento del dashboard: *Encabezado*, *Alertas*, *Selector*, *Indicadores*, *Gráfico*, *Tabla* e *Información adicional*.

   .. code-block:: tsx

      import './App.css';
      import { ... } from '@mui/material';

      function App() {
          return (
              <Grid>

                  {/* Encabezado */}
                  <Grid>Elemento: Encabezado</Grid>

                  {/* Alertas */}
                  ...

              </Grid>
          );
      }

      export default App;

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2,6-29

         import './App.css';
         import { Grid } from '@mui/material';

         function App() {
             return (
                 <Grid>

                     {/* Encabezado */}
                     <Grid>Elemento: Encabezado</Grid>

                     {/* Alertas */}
                     <Grid>Elemento: Alertas</Grid>

                     {/* Selector */}
                     <Grid>Elemento: Selector</Grid>

                     {/* Indicadores */}
                     <Grid>Elemento: Indicadores</Grid>

                     {/* Gráfico */}
                     <Grid>Elemento: Gráfico</Grid>

                     {/* Tabla */}
                     <Grid>Elemento: Tabla</Grid>

                     {/* Información adicional */}
                     <Grid>Elemento: Información adicional</Grid>

                 </Grid>
             );
         }

         export default App;

#. Compruebe la vista previa del resultado en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Para qué sirven los paquetes `@emotion/react` y `@emotion/styled` que se instalaron junto con MUI?
#. ¿Qué ventajas ofrece una biblioteca de componentes como MUI frente a escribir todos los estilos en CSS, como en *TubeTrack*?
#. ¿Por qué los elementos todavía se muestran uno debajo del otro, si se está usando un componente `Grid`?
#. ¿Cómo se escriben los comentarios dentro de JSX? ¿Por qué no funciona `<!-- comentario -->`?

¿Cómo ubico los elementos y hago el dashboard responsivo?
---------------------------------------------------------

#. Modifique el `Grid` principal de `src/App.tsx` (MUI, n.d.-b):

   - Conviértalo en contenedor con el :term:`prop` `container`.
   - Ajuste un espaciado de 5 unidades entre los elementos con el prop `spacing`.
   - Centre los elementos con las propiedades `justifyContent` y `alignItems` dentro del prop `sx`.

   .. code-block:: tsx
      :emphasize-lines: 5

      ...

      function App() {
          return (
              <Grid ... spacing={ ... } sx={{ justifyContent: ... , alignItems: ... }}>
                  ...
              </Grid>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 5

         ...

         function App() {
             return (
                 <Grid container spacing={5} sx={{ justifyContent: 'center', alignItems: 'center' }}>
                     ...
                 </Grid>
             );
         }

#. Revise la documentación de `fluid grids <https://mui.com/material-ui/react-grid/#fluid-grids>`_ y use el prop `size` en cada elemento para aplicar la distribución:

   - Pantallas extra pequeñas (`xs`): todos los elementos ocupan 12 columnas.
   - Pantallas medianas (`md`) en adelante:

     - El encabezado ocupa 12 columnas.
     - Las alertas ocupan 12 columnas.
     - El selector ocupa 3 columnas y los indicadores 9 columnas.
     - El gráfico ocupa 6 columnas y la tabla 6 columnas.
     - La información adicional ocupa 12 columnas.

   .. code-block:: tsx

      {/* Encabezado */}
      <Grid size={{ xs: ... , md: ... }}>Elemento: Encabezado</Grid>

      ...

      {/* Selector */}
      <Grid size={{ xs: ... , md: ... }}>Elemento: Selector</Grid>

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 9,12,15,18,21,24,27

         import './App.css';
         import { Grid } from '@mui/material';

         function App() {
             return (
                 <Grid container spacing={5} sx={{ justifyContent: 'center', alignItems: 'center' }}>

                     {/* Encabezado */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Encabezado</Grid>

                     {/* Alertas */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Alertas</Grid>

                     {/* Selector */}
                     <Grid size={{ xs: 12, md: 3 }}>Elemento: Selector</Grid>

                     {/* Indicadores */}
                     <Grid size={{ xs: 12, md: 9 }}>Elemento: Indicadores</Grid>

                     {/* Gráfico */}
                     <Grid size={{ xs: 12, md: 6 }}>Elemento: Gráfico</Grid>

                     {/* Tabla */}
                     <Grid size={{ xs: 12, md: 6 }}>Elemento: Tabla</Grid>

                     {/* Información adicional */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Información adicional</Grid>

                 </Grid>
             );
         }

         export default App;

#. Revise la documentación del prop `sx <https://mui.com/material-ui/customization/how-to-customize/#the-sx-prop>`_ (MUI, n.d.-c) para que el gráfico y la tabla **no se muestren** en pantallas extra pequeñas y se muestren como bloque desde pantallas medianas.

   .. code-block:: tsx

      {/* Gráfico */}
      <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: ... , md: ... } }}>
          Elemento: Gráfico
      </Grid>

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 21,26

         import './App.css';
         import { Grid } from '@mui/material';

         function App() {
             return (
                 <Grid container spacing={5} sx={{ justifyContent: 'center', alignItems: 'center' }}>

                     {/* Encabezado */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Encabezado</Grid>

                     {/* Alertas */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Alertas</Grid>

                     {/* Selector */}
                     <Grid size={{ xs: 12, md: 3 }}>Elemento: Selector</Grid>

                     {/* Indicadores */}
                     <Grid size={{ xs: 12, md: 9 }}>Elemento: Indicadores</Grid>

                     {/* Gráfico */}
                     <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                         Elemento: Gráfico
                     </Grid>

                     {/* Tabla */}
                     <Grid size={{ xs: 12, md: 6 }} sx={{ display: { xs: 'none', md: 'block' } }}>
                         Elemento: Tabla
                     </Grid>

                     {/* Información adicional */}
                     <Grid size={{ xs: 12, md: 12 }}>Elemento: Información adicional</Grid>

                 </Grid>
             );
         }

         export default App;

#. Compruebe la vista previa del resultado en el navegador para diferentes tamaños de pantalla, usando las herramientas de desarrollador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué es un :term:`prop`? ¿Qué diferencia existe entre los props `container`, `spacing`, `size` y `sx`?
#. ¿En cuántas columnas divide MUI el ancho del contenedor? ¿Qué sucede si la suma de los `size` de una fila supera ese número?
#. ¿Qué rango de anchos de pantalla representan los puntos de quiebre `xs` y `md`? ¿Por qué no fue necesario definir `sm`, `lg` ni `xl`?
#. Compare el prop `sx={{ display: { xs: 'none', md: 'block' } }}` con una *media query* de CSS como las de la guía de diseño responsivo. ¿Qué ventajas y desventajas encuentra?
#. Si los elementos no ocupan todo el ancho de la página, revise las reglas de `body` en `src/index.css`. ¿Qué regla lo provoca y cómo lo corregiría?
#. ¿Por qué es conveniente ocultar el gráfico y la tabla en pantallas pequeñas? ¿Qué alternativa propondría para no perder esa información?

¿Cómo creo mi primer componente reutilizable?
---------------------------------------------

#. Cree el archivo `src/components/HeaderUI.tsx` y exporte el :term:`componente funcional` **HeaderUI**, con:

   - La importación del componente `Typography <https://mui.com/material-ui/react-typography/>`_ (MUI, n.d.-d) desde `@mui/material/Typography`.
   - Un componente `Typography` con estilo tipográfico (`variant`) de encabezado de nivel 2 (``h2``), renderizado (`component`) como encabezado de nivel 1 (``h1``) y texto en negrita (`sx`).
   - El texto *Dashboard del Clima*.

   .. code-block:: tsx

      import ... from '@mui/material/Typography';

      export default function HeaderUI() {
          return (
              <Typography
                  variant= ...
                  component= ...
                  sx={{ ... }}>
                  ...
              </Typography>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx

         import Typography from '@mui/material/Typography';

         export default function HeaderUI() {
             return (
                 <Typography
                     variant="h2"
                     component="h1"
                     sx={{ fontWeight: 'bold' }}>
                     Dashboard del Clima
                 </Typography>
             );
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe el componente `HeaderUI`.
   - Use el componente `HeaderUI` dentro de la sección **Encabezado**, en lugar del texto *Elemento: Encabezado*.

   .. code-block:: tsx

      ...
      import ... from ... ;

      function App() {
          return (
              <Grid container ... >

                  {/* Encabezado */}
                  <Grid size={{ xs: 12, md: 12 }}>
                      ...
                  </Grid>

                  ...
              </Grid>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2,9-11

         ...
         import HeaderUI from './components/HeaderUI';

         function App() {
             return (
                 <Grid container ... >

                     {/* Encabezado */}
                     <Grid size={{ xs: 12, md: 12 }}>
                         <HeaderUI />
                     </Grid>

                     ...
                 </Grid>
             );
         }

#. Compruebe la vista previa del resultado en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Por qué el nombre de un componente de React debe empezar con mayúscula? ¿Qué ocurre si lo llama `headerUI`?
#. ¿Qué diferencia existe entre los props `variant="h2"` y `component="h1"` de `Typography`? ¿Qué etiqueta HTML aparece al inspeccionar la página?
#. ¿Qué ventaja tiene separar el encabezado en un archivo propio dentro de `src/components`, en lugar de escribirlo directamente en `App.tsx`?
#. ¿Qué diferencia existe entre modificar el DOM directamente, como en *TubeTrack* con `innerHTML`, y el DOM Virtual que usa React?

¿Cómo envío datos a un componente mediante props tipadas?
---------------------------------------------------------

#. Cree el archivo `src/components/AlertUI.tsx` y exporte el componente funcional **AlertUI**, con:

   - La importación del componente `Alert <https://mui.com/material-ui/react-alert/>`_ (MUI, n.d.-e) desde `@mui/material/Alert`.
   - La :term:`interfaz` **AlertConfig**, con la propiedad `description` de tipo cadena de texto.
   - El parámetro `config` de tipo **AlertConfig** en el componente `AlertUI`.
   - Un componente `Alert` de tipo éxito (``severity="success"``), con estilo contorneado (``variant="outlined"``), que muestre el valor de `config.description`.

   .. code-block:: tsx

      import ... from '@mui/material/Alert';

      interface AlertConfig {
          ... : ... ;
      }

      export default function AlertUI(config: ... ) {
          return (
              <Alert variant= ... severity= ... >{ ... }</Alert>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx

         import Alert from '@mui/material/Alert';

         interface AlertConfig {
             description: string;
         }

         export default function AlertUI(config: AlertConfig) {
             return (
                 <Alert variant="outlined" severity="success">{config.description}</Alert>
             );
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe el componente `AlertUI`.
   - Use el componente `AlertUI` y pase el prop `description` con el valor *No se prevén lluvias*.
   - Convierta el `Grid` de la sección **Alertas** en contenedor, con los elementos alineados horizontalmente al borde derecho y verticalmente al centro.

   .. code-block:: tsx

      ...
      import AlertUI from './components/AlertUI';

      function App() {
          return (
              <Grid container ... >

                  ...

                  {/* Alertas */}
                  <Grid size={{ xs: 12, md: 12 }}
                      ... sx={{ justifyContent: ... , alignItems: ... }}>

                      <AlertUI ... />

                  </Grid>

                  ...
              </Grid>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2,12,14

         ...
         import AlertUI from './components/AlertUI';

         function App() {
             return (
                 <Grid container ... >

                     ...

                     {/* Alertas */}
                     <Grid size={{ xs: 12, md: 12 }}
                         container sx={{ justifyContent: 'flex-end', alignItems: 'center' }}>

                         <AlertUI description="No se prevén lluvias" />

                     </Grid>

                     ...
                 </Grid>
             );
         }

#. Compruebe la vista previa del resultado en el navegador.
#. Extienda la interfaz **AlertConfig** para que el componente padre (`App`) también pueda definir los props `severity` y `variant` de la alerta:

   - Importe el tipo `AlertColor` desde `@mui/material/Alert` para la propiedad `severity`.
   - Defina un tipo literal de cadena `AlertVariant` con las opciones ``standard``, ``outlined`` y ``filled``.
   - Declare ambas propiedades como opcionales, con valores por defecto ``success`` y ``outlined``.
   - Desde `App.tsx`, muestre una alerta de advertencia con estilo relleno, por ejemplo: *Índice UV alto*.

   .. code-block:: tsx

      import Alert, { type ... } from '@mui/material/Alert';

      type AlertVariant = ... ;

      interface AlertConfig {
          description: string;
          severity ... : ... ;
          variant ... : ... ;
      }

      export default function AlertUI({ description, severity = ... , variant = ... }: AlertConfig) {
          return (
              <Alert variant={ ... } severity={ ... }>{description}</Alert>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 1,3,7,8,11,13

         import Alert, { type AlertColor } from '@mui/material/Alert';

         type AlertVariant = 'standard' | 'outlined' | 'filled';

         interface AlertConfig {
             description: string;
             severity?: AlertColor;
             variant?: AlertVariant;
         }

         export default function AlertUI({ description, severity = 'success', variant = 'outlined' }: AlertConfig) {
             return (
                 <Alert variant={variant} severity={severity}>{description}</Alert>
             );
         }

      .. code-block:: tsx

         <AlertUI description="No se prevén lluvias" />
         <AlertUI description="Índice UV alto" severity="warning" variant="filled" />

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Compare la interfaz `AlertConfig` con la interfaz `Cancion` de *TubeTrack* (Guía 07). ¿Qué tienen en común y para qué se usa cada una?
#. En `App.tsx` se escribe `<AlertUI description="..." />`, pero el componente recibe un parámetro `config`. ¿Cómo llegan los atributos JSX a ese parámetro?
#. ¿Qué mensaje muestra TypeScript si omite el prop `description` o si escribe `severity="peligro"`? ¿Por qué ese error es útil?
#. ¿Qué ventaja tiene la desestructuración `{ description, severity = 'success', variant = 'outlined' }` frente a usar `config.description`?
#. ¿En qué se parece el tipo `AlertVariant` al tipo `Genero` de *TubeTrack*?
#. ¿Cómo comprobaría que `AlertUI` es reutilizable en otras secciones del dashboard, por ejemplo para la alerta del índice UV?
#. Si un asistente de IA le sugiere el tipo `any` para los props de un componente, ¿qué se pierde y cómo justificaría usar una interfaz propia?

¿Cómo versiono y publico el dashboard en GitHub Pages?
------------------------------------------------------

#. Versione local y remotamente la rama de desarrollo en el repositorio *dashboard*:

   .. code-block:: bash

      git add .
      git commit -m "Estructura del dashboard con React y MUI"
      git push origin desarrollo

#. Genere la solicitud de cambios (*pull request*) hacia la rama principal y apruebe los cambios.
#. Desde la línea de comandos, instale el paquete `gh-pages` (gh-pages, n.d.):

   .. code-block:: bash

      npm install gh-pages --save-dev

#. Modifique el archivo `package.json` con la entrada **homepage** (reemplace `<username>` por su usuario de GitHub) y los comandos **predeploy** y **deploy** dentro de **scripts**:

   .. code-block:: json
      :emphasize-lines: 3,7,8

      {
          ...
          "homepage": "https://<username>.github.io/dashboard",
          ...
          "scripts": {
              ...
              "predeploy": "npm run build",
              "deploy": "gh-pages -d dist"
          }
      }

#. Modifique el archivo `vite.config.ts` con la ruta al repositorio remoto:

   .. code-block:: typescript
      :emphasize-lines: 2

      export default defineConfig({
          base: '/dashboard/',
          plugins: [react()],
      })

#. Ejecute la transpilación y el despliegue del sitio web:

   .. code-block:: bash

      npm run deploy

   - De ser necesario, elimine, corrija o comente las secciones de código identificadas por el transpilador.
   - Vuelva a ejecutar el comando de despliegue.

#. Compruebe el resultado en el navegador, con la URL: `https://<username>.github.io/dashboard`

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué contiene la carpeta `dist` después de ejecutar `npm run build`? ¿Por qué se publica esa carpeta y no `src`?
#. ¿Por qué el script `predeploy` se ejecuta automáticamente antes de `deploy`?
#. ¿Qué ocurre con las rutas de los archivos JavaScript y CSS si se omite la opción `base: '/dashboard/'`?
#. ¿Por qué un error de tipos de TypeScript puede impedir el despliegue aunque la página funcione con `npm run dev`?
#. ¿Qué decisiones de esta guía tomó usted y cuáles tomó a partir de sugerencias de una IA? ¿Cómo comprobó que el resultado es consistente, accesible y adaptable a distintos tamaños de pantalla?

Referencias
===========

* gh-pages. (n.d.). npm. Retrieved October 25, 2026 from https://www.npmjs.com/package/gh-pages
* MUI. (n.d.-a). Overview - Material UI. Retrieved October 25, 2026 from https://mui.com/material-ui/getting-started/overview/
* MUI. (n.d.-b). Grid - Material UI. Retrieved October 25, 2026 from https://mui.com/material-ui/react-grid/
* MUI. (n.d.-c). How to customize - Material UI. Retrieved October 25, 2026 from https://mui.com/material-ui/customization/how-to-customize/
* MUI. (n.d.-d). Typography - Material UI. Retrieved October 25, 2026 from https://mui.com/material-ui/react-typography/
* MUI. (n.d.-e). Alert - Material UI. Retrieved October 25, 2026 from https://mui.com/material-ui/react-alert/
* Open-Meteo. (n.d.). Weather Forecast API. Retrieved October 25, 2026 from https://open-meteo.com/en/docs
* React. (n.d.). React. Retrieved October 25, 2026 from https://react.dev/
* Vite. (n.d.). Getting Started. Retrieved October 25, 2026 from https://vitejs.dev/guide/