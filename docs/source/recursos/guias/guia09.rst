..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

===========================================
Guía 09: React - Eventos y Hooks (useState)
===========================================

.. topic:: Objetivo específico
   :class: objetivo

   Gestionar la interactividad del dashboard del clima mediante eventos del usuario y el hook useState, para actualizar el estado de la interfaz de forma reactiva a partir de la selección de una ciudad.

Actividades en clases
=====================

¿Cómo agrego un selector de ciudades al dashboard?
--------------------------------------------------

#. Acceda a su proyecto *dashboard* en Codespaces o en su máquina local, cámbiese a la rama de desarrollo y levante el servidor:

   .. code-block:: bash

      git checkout desarrollo
      npm install
      npm run dev

#. Cree el archivo `src/components/SelectorUI.tsx` y, con base en la documentación del componente `Select <https://mui.com/material-ui/react-select/>`_ (MUI, n.d.), agregue el componente funcional `SelectorUI`:

   .. dropdown:: SelectorUI.tsx
      :color: success

      .. code-block:: tsx

         import FormControl from '@mui/material/FormControl';
         import InputLabel from '@mui/material/InputLabel';
         import Select from '@mui/material/Select';
         import MenuItem from '@mui/material/MenuItem';

         export default function SelectorUI() {
             return (
                 <FormControl fullWidth>
                     <InputLabel id="city-select-label">Ciudad</InputLabel>
                     <Select
                         labelId="city-select-label"
                         id="city-simple-select"
                         label="Ciudad">
                         <MenuItem disabled><em>Seleccione una ciudad</em></MenuItem>
                         <MenuItem value={'guayaquil'}>Guayaquil</MenuItem>
                         <MenuItem value={'quito'}>Quito</MenuItem>
                         <MenuItem value={'manta'}>Manta</MenuItem>
                         <MenuItem value={'cuenca'}>Cuenca</MenuItem>
                     </Select>
                 </FormControl>
             );
         }

#. Modifique el archivo `src/App.tsx`:

   - Importe el componente `SelectorUI`.
   - Use el componente `SelectorUI` dentro de la sección **Selector**, en lugar del texto *Elemento: Selector*.

   .. code-block:: tsx

      ...
      import ... from ... ;

      function App() {
          return (
              <Grid container ... >

                  ...

                  {/* Selector */}
                  <Grid size={{ xs: 12, md: 3 }}>
                      ...
                  </Grid>

                  ...
              </Grid>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 2,11-13

         ...
         import SelectorUI from './components/SelectorUI';

         function App() {
             return (
                 <Grid container ... >

                     ...

                     {/* Selector */}
                     <Grid size={{ xs: 12, md: 3 }}>
                         <SelectorUI />
                     </Grid>

                     ...
                 </Grid>
             );
         }

#. Compruebe la vista previa del resultado en el navegador y seleccione distintas ciudades.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Investigue la función de cada componente de MUI en la tabla:

   .. list-table:: Componentes del selector
      :header-rows: 1
      :widths: 25 75

      * - Componente
        - ¿Para qué sirve?
      * - ``FormControl``
        -
      * - ``InputLabel``
        -
      * - ``Select``
        -
      * - ``MenuItem``
        -

#. ¿Qué relación existe entre el prop `id` de `InputLabel` y el prop `labelId` de `Select`? ¿Por qué es importante para la accesibilidad?
#. ¿Qué efecto tiene el prop `fullWidth` en el `FormControl`? ¿Qué ocurre si lo elimina?
#. ¿Por qué la primera opción tiene el prop `disabled` y no tiene `value`?
#. Inspeccione la página con las herramientas de desarrollador. ¿Se genera una etiqueta HTML `<select>`? ¿Qué elementos genera MUI en su lugar?
#. En este momento, ¿el componente `App` puede saber qué ciudad seleccionó el usuario? ¿Por qué?

¿Cómo respondo cuando el usuario elige una ciudad?
--------------------------------------------------

#. Modifique el componente `SelectorUI` para reaccionar al cambio de selección:

   - Importe el tipo `SelectChangeEvent` desde `@mui/material/Select`.
   - Dentro del componente, declare la función flecha `handleChange` que reciba el parámetro **event** de tipo `SelectChangeEvent<string>` y muestre en una ventana emergente el valor del elemento que generó el evento.
   - Asigne la función `handleChange` al evento `onChange` del componente `Select`.

   .. code-block:: tsx

      ...
      import Select, { type ... } from '@mui/material/Select';
      ...

      export default function SelectorUI() {

          const handleChange = (event: ... ) => {
              alert( ... );
          };

          return (
              ...
              <Select
                  ...
                  onChange={ ... }>
                  ...
              </Select>
              ...
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 3,8-10,19

         import FormControl from '@mui/material/FormControl';
         import InputLabel from '@mui/material/InputLabel';
         import Select, { type SelectChangeEvent } from '@mui/material/Select';
         import MenuItem from '@mui/material/MenuItem';

         export default function SelectorUI() {

             const handleChange = (event: SelectChangeEvent<string>) => {
                 alert(event.target.value);
             };

             return (
                 <FormControl fullWidth>
                     <InputLabel id="city-select-label">Ciudad</InputLabel>
                     <Select
                         labelId="city-select-label"
                         id="city-simple-select"
                         label="Ciudad"
                         onChange={handleChange}>
                         <MenuItem disabled><em>Seleccione una ciudad</em></MenuItem>
                         <MenuItem value={'guayaquil'}>Guayaquil</MenuItem>
                         <MenuItem value={'quito'}>Quito</MenuItem>
                         <MenuItem value={'manta'}>Manta</MenuItem>
                         <MenuItem value={'cuenca'}>Cuenca</MenuItem>
                     </Select>
                 </FormControl>
             );
         }

#. Compruebe la vista previa del resultado en el navegador.
#. Reemplace `alert(event.target.value)` por `console.log(event)` y revise la consola de las herramientas de desarrollador. Luego, vuelva a la versión con `alert`.
#. Cambie temporalmente `onChange={handleChange}` por `onChange={handleChange()}` y observe el mensaje del transpilador. Luego, revierta el cambio.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. En *TubeTrack* (Guía 07), con Javascript, se usó `buscarInput.addEventListener('input', ...)`. ¿Qué diferencia encuentra con asignar `onChange={handleChange}` directamente en JSX?
#. ¿Qué es un *evento sintético* en React (Certidevs, n.d.)? ¿Qué propiedades del objeto `event` observó en la consola?
#. ¿Por qué se escribe `onChange={handleChange}` y no `onChange={handleChange()}`? ¿Qué mensaje mostró el transpilador?
#. ¿Qué aporta el tipo `SelectChangeEvent<string>`? ¿Qué tipo de dato tiene `event.target.value`?
#. ¿Por qué el nombre del evento se escribe `onChange` (*camelCase*) y no `onchange`, como en HTML?

¿Cómo hago que el componente recuerde la ciudad seleccionada?
-------------------------------------------------------------

#. Antes de usar un hook, pruebe con una variable común. Modifique temporalmente `SelectorUI` con el siguiente código y observe la consola y la página al seleccionar una ciudad:

   .. code-block:: tsx
      :emphasize-lines: 3,6,7,15-17

      export default function SelectorUI() {

          let cityInput = '';

          const handleChange = (event: SelectChangeEvent<string>) => {
              cityInput = event.target.value;
              console.log(cityInput);
          };

          return (
              <FormControl fullWidth>
                  ...
                  </Select>

                  {cityInput && (
                      <p>Información del clima en {cityInput}</p>
                  )}
              </FormControl>
          );
      }

#. Revise la documentación de `useState <https://es.react.dev/reference/react/useState>`_ (React, n.d.) y modifique el componente `SelectorUI`:

   - Importe el :term:`hook` **useState** desde `react`.
   - Reemplace la variable común por una :term:`variable de estado` llamada **cityInput**, con su :term:`función de actualización` **setCityInput** y un valor inicial vacío.
   - Dentro de `handleChange`, llame a `setCityInput` con el valor seleccionado (`event.target.value`).
   - Enlace el prop `value` del `Select` con la variable de estado `cityInput`.
   - Debajo del `Select`, solo si `cityInput` tiene un valor, muestre un párrafo con el texto *Información del clima en [ciudad seleccionada]*, con el nombre de la ciudad en negritas y la primera letra en mayúscula.

   .. code-block:: tsx

      import { ... } from 'react';
      ...

      export default function SelectorUI() {

          const [ ... , ... ] = useState( ... );

          const handleChange = (event: SelectChangeEvent<string>) => {
              ... (event.target.value);
          };

          return (
              <FormControl fullWidth>
                  ...
                  <Select
                      ...
                      value={ ... }
                      onChange={handleChange}>
                      ...
                  </Select>

                  { ... && (
                      <p>
                          Información del clima en <span style={{ ... }}>{ ... }</span>
                      </p>
                  )}
              </FormControl>
          );
      }

   .. dropdown:: Solución
      :color: success

      .. code-block:: tsx
         :emphasize-lines: 1,9,12,22,31-35

         import { useState } from 'react';
         import FormControl from '@mui/material/FormControl';
         import InputLabel from '@mui/material/InputLabel';
         import Select, { type SelectChangeEvent } from '@mui/material/Select';
         import MenuItem from '@mui/material/MenuItem';

         export default function SelectorUI() {

             const [cityInput, setCityInput] = useState('');

             const handleChange = (event: SelectChangeEvent<string>) => {
                 setCityInput(event.target.value);
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

#. Compruebe la vista previa del resultado en el navegador.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. Con la variable común `let cityInput`, ¿qué mostró la consola y qué mostró la página? ¿Por qué el párrafo no apareció?
#. ¿Qué es un :term:`hook` (Adictos al trabajo, 2020)? ¿Qué devuelve `useState('')` y por qué se usa la desestructuración `[cityInput, setCityInput]`?
#. ¿Qué ocurre con el componente cada vez que se llama a `setCityInput`? ¿Por qué no basta con asignar `cityInput = event.target.value`?
#. ¿Qué tipo de dato infiere TypeScript para `cityInput` a partir del valor inicial `''`? ¿Cómo lo declararía de forma explícita?
#. ¿Qué diferencia existe entre un `Select` *controlado* (con `value={cityInput}`) y uno *no controlado*?
#. ¿Cómo funciona la expresión `{cityInput && (...)}`? Compare esta forma de mostrar u ocultar contenido con el `display: none` del mensaje `#sin-resultados` mediante JavaScript y CSS.
#. ¿Qué hace la propiedad `textTransform: 'capitalize'`? ¿Por qué se escribe `style={{ ... }}` con doble llave?
#. ¿Qué cambio haría para que el componente `App` también conozca la ciudad seleccionada y pueda usarla en otras secciones del dashboard?

¿Cómo versiono y publico los cambios?
-------------------------------------

#. Versione local y remotamente la rama de desarrollo en el repositorio *dashboard*:

   .. code-block:: bash

      git add .
      git commit -m "Selector de ciudades con eventos y useState"
      git push origin desarrollo

#. Genere la solicitud de cambios (*pull request*) hacia la rama principal y apruebe los cambios.
#. Ejecute la transpilación y el despliegue del sitio web:

   .. code-block:: bash

      npm run deploy

   - De ser necesario, elimine, corrija o comente las secciones de código identificadas por el transpilador (por ejemplo, el código de prueba con `let cityInput`).
   - Vuelva a ejecutar el comando de despliegue.

#. Compruebe el resultado en el navegador, con la URL: `https://<username>.github.io/dashboard`

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué ajustes tuvo que hacer al código (propio o sugerido por una IA) para que el evento `onChange` actualizara correctamente el estado con `useState`?
#. ¿Cómo demostraría que comprende el código del selector y es responsable de él, aunque parte haya sido generada por una IA? Explique con sus palabras el recorrido desde que el usuario elige una ciudad hasta que aparece el párrafo.

Referencias
===========

* Adictos al trabajo. (2020, February 6). React Hooks - Tutorial. Retrieved November 9, 2026 from https://adictosaltrabajo.com/2020/02/06/react-hooks-tutorial/
* Certidevs. (n.d.). Tutorial React: Eventos en React. Retrieved November 9, 2026 from https://certidevs.com/tutorial-react-eventos-y-manejo-de-eventos
* MUI. (n.d.). Select - Material UI. Retrieved November 9, 2026 from https://mui.com/material-ui/react-select/
* React. (n.d.). useState. Retrieved November 9, 2026 from https://es.react.dev/reference/react/useState