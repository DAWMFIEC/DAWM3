..
   Copyright (c) 2026 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

====================================
Guía extra: Conversión de CSV a JSON
====================================

.. topic:: Objetivo específico
   :class: objetivo

   Transformar datos meteorológicos de formato CSV a JSON mediante herramientas en línea y scripts de Python, ajustando los tipos de datos y comprobando la estructura, los valores faltantes y la consistencia de los registros, para preparar datos reutilizables en aplicaciones web.

Actividades en clases
=====================

¿Cómo está organizado el archivo CSV y qué variables necesito?
--------------------------------------------------------------

#. Cree una carpeta llamada *csv2json* en su computador o en GitHub Codespaces.
#. Dentro de la carpeta, cree el archivo `clima.csv` con los datos por hora exportados desde la API de Open-Meteo (Open-Meteo, n.d.):

   .. dropdown:: clima.csv
      :color: success

      .. code-block:: text

         latitude,longitude,elevation,utc_offset_seconds,timezone,timezone_abbreviation
         -2.1875,-79.875,6.0,-18000,America/Guayaquil,GMT-5

         time,temperature_2m (°C),relative_humidity_2m (%),apparent_temperature (°C),precipitation (mm),weather_code (wmo code),wind_speed_10m (km/h)
         2026-11-09T00:00,24.1,86,27.0,0.00,3,6.5
         2026-11-09T01:00,23.9,87,26.8,0.00,3,6.1
         2026-11-09T02:00,23.7,88,26.5,0.10,51,5.8
         2026-11-09T03:00,23.6,89,26.4,0.20,51,5.4
         2026-11-09T04:00,23.4,90,26.2,,3,5.0
         2026-11-09T05:00,23.3,90,26.1,0.00,2,4.9
         2026-11-09T05:00,23.3,90,26.1,0.00,2,4.9
         2026-11-09T06:00,23.4,88,26.3,0.00,2,5.2
         2026-11-09T07:00,24.2,84,27.1,0.00,1,6.0
         2026-11-09T08:00,25.3,79,28.4,0.00,1,7.3
         2026-11-09T09:00,26.5,74,29.6,0.00,0,8.1

#. Abra el archivo en un editor de texto y luego en una hoja de cálculo (Excel, LibreOffice Calc o Google Sheets). Compare cómo se ve en cada herramienta.
#. (Opcional) Obtenga un archivo real con la misma estructura desde su navegador, con la URL:

   .. code-block:: text

      https://api.open-meteo.com/v1/forecast?latitude=-2.1962&longitude=-79.8862&hourly=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,weather_code,wind_speed_10m&timezone=America%2FGuayaquil&forecast_days=1&format=csv

#. Complete la tabla con la información de cada columna e indique si la variable es necesaria para un dashboard del clima que muestre la temperatura, la humedad, la precipitación y el estado del cielo:

   .. list-table:: Variables del archivo CSV
      :header-rows: 1
      :widths: 30 15 25 30

      * - Columna
        - Unidad
        - Tipo de dato esperado
        - ¿Necesaria? (Sí/No) ¿Por qué?
      * - ``time``
        -
        -
        -
      * - ``temperature_2m (°C)``
        -
        -
        -
      * - ``relative_humidity_2m (%)``
        -
        -
        -
      * - ``apparent_temperature (°C)``
        -
        -
        -
      * - ``precipitation (mm)``
        -
        -
        -
      * - ``weather_code (wmo code)``
        -
        -
        -
      * - ``wind_speed_10m (km/h)``
        -
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Cuántos bloques de información tiene el archivo? ¿Qué describen las dos primeras líneas y qué separa ese bloque de los datos por hora?
#. ¿Qué carácter separa los valores? ¿Qué problema aparecería si la hoja de cálculo de su computador usa `;` como separador (Shafranovich, 2005)?
#. ¿Qué información aporta el texto entre paréntesis en los encabezados? ¿Es conveniente conservarlo como parte del nombre de la variable?
#. ¿En qué formato está escrita la fecha y hora de la columna `time`? ¿Indica la zona horaria? ¿Dónde se encuentra esa información en el archivo?
#. Sin usar ninguna herramienta, identifique a simple vista los registros que presentan algún problema. ¿Qué problemas encontró?
#. ¿Por qué en un archivo CSV no se puede distinguir si `3` es un número o un texto?

¿Cómo convierto el CSV a JSON con una herramienta en línea?
-----------------------------------------------------------

#. Ingrese a `CSVJSON <https://csvjson.com/csv2json>`_ (CSVJSON, n.d.), en la opción **CSV to JSON**.
#. Pegue **todo** el contenido de `clima.csv` en el área de entrada, pulse **Convert** y observe el resultado.
#. Ahora pegue solo el contenido desde la línea de encabezados (`time,temperature_2m (°C),...`) hasta el final y vuelva a convertir.
#. Pruebe las siguientes opciones de la herramienta y observe los cambios en el resultado:

   - **Separator**: *Auto-detect* y *Comma*.
   - **Parse numbers**: activado y desactivado.
   - **Output**: *Array* y *Hash*.
   - **Minify**: activado y desactivado.

#. Con las opciones **Parse numbers** activado, **Output** *Array* y **Minify** desactivado, guarde el resultado en el archivo `clima_csvjson.json` dentro de la carpeta *csv2json*.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué sucedió al convertir el archivo completo, con las líneas de metadatos? ¿Qué claves tomaron los objetos?
#. ¿Qué diferencia observa en los valores cuando **Parse numbers** está activado y desactivado?
#. ¿Qué estructura genera la salida *Array* y cuál la salida *Hash*? ¿Cuál sería más útil para recorrer los registros en orden?
#. ¿Qué valor quedó en `precipitation (mm)` para el registro de las `2026-11-09T04:00`? ¿Es adecuado para representar un dato faltante?
#. ¿Qué tipo de dato quedó en la columna `time`? ¿La herramienta reconoce fechas?
#. ¿Qué ventajas y riesgos tiene subir datos a una herramienta en línea? ¿Lo haría con datos personales o confidenciales?

¿Cómo convierto el CSV a JSON con un script y ajusto los tipos de datos?
------------------------------------------------------------------------

#. Revise el ejemplo de conversión de `GeeksforGeeks <https://www.geeksforgeeks.org/python/convert-csv-to-json-using-python/>`_ (GeeksforGeeks, 2025) y la documentación de los módulos `csv <https://docs.python.org/3/library/csv.html>`_ y `json <https://docs.python.org/3/library/json.html>`_ de Python (Python Software Foundation, n.d.-a, n.d.-b).
#. Cree el archivo `convertir.py` en la carpeta *csv2json* con una primera versión del conversor:

   .. code-block:: python

      import csv
      import json


      def csv_a_json(ruta_csv, ruta_json):
          registros = []

          with open(ruta_csv, encoding='utf-8') as archivo_csv:
              lector = csv.DictReader(archivo_csv)
              for fila in lector:
                  registros.append(fila)

          with open(ruta_json, 'w', encoding='utf-8') as archivo_json:
              json.dump(registros, archivo_json, indent=4, ensure_ascii=False)

          print(f'{len(registros)} registros convertidos')


      csv_a_json('clima.csv', 'clima.json')

#. Ejecute el script desde la terminal y abra el archivo `clima.json` generado:

   .. code-block:: bash

      python convertir.py

#. Modifique la función `csv_a_json` para omitir las tres primeras líneas del archivo (metadatos y línea en blanco) antes de crear el `DictReader`:

   .. code-block:: python

      LINEAS_METADATOS = ...


      def csv_a_json(ruta_csv, ruta_json):
          registros = []

          with open(ruta_csv, encoding='utf-8') as archivo_csv:
              for _ in range( ... ):
                  next( ... )
              lector = csv.DictReader(archivo_csv)
              ...

   .. dropdown:: Solución
      :color: success

      .. code-block:: python
         :emphasize-lines: 1,8-9

         LINEAS_METADATOS = 3


         def csv_a_json(ruta_csv, ruta_json):
             registros = []

             with open(ruta_csv, encoding='utf-8') as archivo_csv:
                 for _ in range(LINEAS_METADATOS):
                     next(archivo_csv)
                 lector = csv.DictReader(archivo_csv)
                 for fila in lector:
                     registros.append(fila)
             ...

#. Ejecute nuevamente el script y observe el tipo de dato de cada valor en `clima.json`.
#. Complete la tabla con el tipo de dato que debe tener cada variable seleccionada en la primera actividad:

   .. list-table:: Ajuste de tipos de datos
      :header-rows: 1
      :widths: 25 20 20 35

      * - Variable en JSON
        - Tipo en el CSV
        - Tipo en Python
        - Tipo en JSON
      * - ``fecha_hora``
        -
        -
        -
      * - ``temperatura``
        -
        -
        -
      * - ``humedad``
        -
        -
        -
      * - ``precipitacion``
        -
        -
        -
      * - ``codigo_clima``
        -
        -
        -

#. Agregue al script las funciones de conversión de tipos y la función `convertir_fila`, que:

   - Conserve solo las variables necesarias y les asigne nombres sin unidades ni espacios.
   - Convierta los valores decimales con `float` y los enteros con `int`, y devuelva `None` cuando el valor esté vacío.
   - Convierta la fecha y hora a formato ISO 8601 con la zona horaria de Guayaquil (UTC-05:00), usando el módulo `datetime <https://docs.python.org/3/library/datetime.html>`_ (Python Software Foundation, n.d.-c).

   .. code-block:: python

      import csv
      import json
      from datetime import datetime, timedelta, timezone

      LINEAS_METADATOS = 3
      ZONA_HORARIA = timezone(timedelta(hours= ... ))


      def a_float(valor):
          return ... (valor) if valor.strip() != '' else ...


      def a_int(valor):
          return ... (valor) if valor.strip() != '' else ...


      def a_fecha(valor):
          fecha = datetime.strptime(valor, ... )
          return fecha.replace(tzinfo= ... ).isoformat()


      def convertir_fila(fila):
          return {
              'fecha_hora': a_fecha(fila['time']),
              'temperatura': ... (fila['temperature_2m (°C)']),
              'humedad': ... (fila['relative_humidity_2m (%)']),
              'precipitacion': ... (fila['precipitation (mm)']),
              'codigo_clima': ... (fila['weather_code (wmo code)']),
          }

#. Modifique `csv_a_json` para agregar a la lista el resultado de `convertir_fila(fila)` en lugar de la fila original.

   .. dropdown:: Solución
      :color: success

      .. code-block:: python
         :emphasize-lines: 3,6,9-29,40

         import csv
         import json
         from datetime import datetime, timedelta, timezone

         LINEAS_METADATOS = 3
         ZONA_HORARIA = timezone(timedelta(hours=-5))


         def a_float(valor):
             return float(valor) if valor.strip() != '' else None


         def a_int(valor):
             return int(valor) if valor.strip() != '' else None


         def a_fecha(valor):
             fecha = datetime.strptime(valor, '%Y-%m-%dT%H:%M')
             return fecha.replace(tzinfo=ZONA_HORARIA).isoformat()


         def convertir_fila(fila):
             return {
                 'fecha_hora': a_fecha(fila['time']),
                 'temperatura': a_float(fila['temperature_2m (°C)']),
                 'humedad': a_int(fila['relative_humidity_2m (%)']),
                 'precipitacion': a_float(fila['precipitation (mm)']),
                 'codigo_clima': a_int(fila['weather_code (wmo code)']),
             }


         def csv_a_json(ruta_csv, ruta_json):
             registros = []

             with open(ruta_csv, encoding='utf-8') as archivo_csv:
                 for _ in range(LINEAS_METADATOS):
                     next(archivo_csv)
                 lector = csv.DictReader(archivo_csv)
                 for fila in lector:
                     registros.append(convertir_fila(fila))

             with open(ruta_json, 'w', encoding='utf-8') as archivo_json:
                 json.dump(registros, archivo_json, indent=4, ensure_ascii=False)

             print(f'{len(registros)} registros convertidos')


         csv_a_json('clima.csv', 'clima.json')

#. Ejecute el script y compare el nuevo `clima.json` con `clima_csvjson.json`.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué claves y valores obtuvo en la primera versión del script, sin omitir los metadatos? ¿Por qué?
#. ¿Qué hace `csv.DictReader`? ¿De dónde obtiene los nombres de las claves de cada diccionario?
#. ¿Qué hace la función `next(archivo_csv)`? ¿Qué pasaría si el archivo de origen tuviera cuatro líneas de metadatos en lugar de tres?
#. ¿Qué significan los argumentos `indent=4` y `ensure_ascii=False` de `json.dump`? Pruebe el script sin ellos y compare el resultado.
#. ¿Qué error muestra Python si usa `float(valor)` con el valor vacío del registro de las `04:00`? ¿Por qué se convierte a `None` y en qué valor de JSON se transforma?
#. ¿Por qué la humedad y el código del clima se convierten con `int` y la temperatura y la precipitación con `float`?
#. ¿Qué valor obtuvo en `fecha_hora` para el primer registro? ¿Qué indica el sufijo `-05:00` y por qué es importante incluirlo?
#. ¿Por qué JSON no tiene un tipo de dato propio para fechas (Bray, 2017)? ¿Qué ventaja tiene usar el formato ISO 8601?

¿Cómo compruebo la estructura, los valores faltantes y la consistencia del JSON?
--------------------------------------------------------------------------------

#. Cree el archivo `validar.py` en la carpeta *csv2json*, que lea `clima.json` y compruebe:

   - **Estructura**: el tipo de la raíz, la cantidad de registros y que todos los registros tengan las mismas claves.
   - **Valores faltantes**: la cantidad de valores `None` en cada clave.
   - **Consistencia**: humedad dentro del rango de 0 a 100, fechas duplicadas e intervalos de una hora entre registros consecutivos.

   .. code-block:: python

      import json
      from datetime import datetime

      with open( ... , encoding='utf-8') as archivo:
          registros = json. ... (archivo)

      # 1. Estructura
      print('Tipo de la raíz:', type(registros).__name__)
      print('Cantidad de registros:', ... )

      claves = set(registros[0].keys())
      for i, registro in enumerate(registros):
          if set(registro.keys()) != ... :
              print(f'Registro {i}: claves distintas')

      # 2. Valores faltantes
      for clave in claves:
          faltantes = sum(1 for registro in registros if registro[clave] is ... )
          print(f'{clave}: {faltantes} valores faltantes')

      # 3. Consistencia
      for registro in registros:
          if registro['humedad'] is not None and not ... <= registro['humedad'] <= ... :
              print('Humedad fuera de rango:', registro)

      fechas = [registro['fecha_hora'] for registro in registros]
      duplicadas = {fecha for fecha in fechas if fechas.count(fecha) > ... }
      print('Fechas duplicadas:', duplicadas)

      for anterior, actual in zip(fechas, fechas[1:]):
          diferencia = datetime.fromisoformat(actual) - datetime.fromisoformat(anterior)
          if diferencia.total_seconds() != ... :
              print(f'Intervalo irregular entre {anterior} y {actual}: {diferencia}')

   .. dropdown:: Solución
      :color: success

      .. code-block:: python

         import json
         from datetime import datetime

         with open('clima.json', encoding='utf-8') as archivo:
             registros = json.load(archivo)

         # 1. Estructura
         print('Tipo de la raíz:', type(registros).__name__)
         print('Cantidad de registros:', len(registros))

         claves = set(registros[0].keys())
         for i, registro in enumerate(registros):
             if set(registro.keys()) != claves:
                 print(f'Registro {i}: claves distintas')

         # 2. Valores faltantes
         for clave in claves:
             faltantes = sum(1 for registro in registros if registro[clave] is None)
             print(f'{clave}: {faltantes} valores faltantes')

         # 3. Consistencia
         for registro in registros:
             if registro['humedad'] is not None and not 0 <= registro['humedad'] <= 100:
                 print('Humedad fuera de rango:', registro)

         fechas = [registro['fecha_hora'] for registro in registros]
         duplicadas = {fecha for fecha in fechas if fechas.count(fecha) > 1}
         print('Fechas duplicadas:', duplicadas)

         for anterior, actual in zip(fechas, fechas[1:]):
             diferencia = datetime.fromisoformat(actual) - datetime.fromisoformat(anterior)
             if diferencia.total_seconds() != 3600:
                 print(f'Intervalo irregular entre {anterior} y {actual}: {diferencia}')

#. Ejecute el script de validación y registre los resultados en la tabla:

   .. code-block:: bash

      python validar.py

   .. list-table:: Resultados de la validación
      :header-rows: 1
      :widths: 30 35 35

      * - Comprobación
        - Resultado obtenido
        - ¿Qué acción tomaría?
      * - Tipo de la raíz y cantidad de registros
        -
        -
      * - Registros con claves distintas
        -
        -
      * - Valores faltantes por clave
        -
        -
      * - Valores fuera de rango
        -
        -
      * - Fechas duplicadas
        -
        -
      * - Intervalos irregulares
        -
        -

#. Modifique la función `csv_a_json` de `convertir.py` para descartar los registros cuya `fecha_hora` ya fue agregada:

   .. code-block:: python

      with open(ruta_csv, encoding='utf-8') as archivo_csv:
          ...
          vistos = set()
          for fila in lector:
              registro = convertir_fila(fila)
              if registro['fecha_hora'] not in ... :
                  vistos. ... (registro['fecha_hora'])
                  registros.append( ... )

   .. dropdown:: Solución
      :color: success

      .. code-block:: python
         :emphasize-lines: 6-11

         with open(ruta_csv, encoding='utf-8') as archivo_csv:
             for _ in range(LINEAS_METADATOS):
                 next(archivo_csv)
             lector = csv.DictReader(archivo_csv)

             vistos = set()
             for fila in lector:
                 registro = convertir_fila(fila)
                 if registro['fecha_hora'] not in vistos:
                     vistos.add(registro['fecha_hora'])
                     registros.append(registro)

#. Ejecute nuevamente `convertir.py` y `validar.py`, y compare los resultados con la tabla anterior.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué diferencia existe entre un JSON *válido* (bien formado) y un JSON *consistente* con lo que se espera de los datos?
#. ¿Qué devuelve `json.load` y qué tipos de Python corresponden a un arreglo, un objeto y un `null` de JSON?
#. ¿Por qué el registro duplicado produjo un intervalo irregular de `0:00:00`?
#. Para el valor faltante de precipitación, ¿qué es mejor: descartar el registro, conservarlo con `null` o reemplazarlo por `0`? ¿De qué depende la decisión?
#. ¿Qué otras reglas de consistencia agregaría para estos datos (por ejemplo, para la temperatura o el código del clima)?
#. ¿Por qué es conveniente separar la conversión (`convertir.py`) de la validación (`validar.py`) en scripts distintos?

¿Cómo visualizo el JSON resultante?
-----------------------------------

#. Ingrese a `Online JSON Viewer <https://jsonviewer.stack.hu/>`_ (Online JSON Viewer, n.d.).
#. En la pestaña **Text**, pegue el contenido de `clima.json` y pulse **Format**. Luego, pulse **Remove white space** y observe el cambio.
#. Cambie a la pestaña **Viewer**, despliegue el árbol de datos y seleccione un registro para observar sus propiedades.
#. Repita los pasos anteriores con el archivo `clima_csvjson.json`.
#. Borre una coma o una llave del texto pegado y vuelva a la pestaña **Viewer**. Observe el resultado y luego corrija el error.
#. Complete la tabla comparando ambos resultados:

   .. list-table:: Comparación de resultados
      :header-rows: 1
      :widths: 30 35 35

      * - Característica
        - ``clima_csvjson.json`` (CSVJSON)
        - ``clima.json`` (script de Python)
      * - Cantidad de registros
        -
        -
      * - Nombres de las claves
        -
        -
      * - Tipo de dato de la fecha y hora
        -
        -
      * - Representación del valor faltante
        -
        -
      * - Registros duplicados
        -
        -
      * - Variables incluidas
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

#. ¿Qué representa cada nivel del árbol en la pestaña **Viewer**? ¿Cómo se identifican visualmente los arreglos y los objetos?
#. ¿Qué hacen las opciones **Format** y **Remove white space**? ¿En qué casos conviene usar cada formato?
#. ¿Qué mensaje mostró la herramienta cuando el JSON estaba mal formado?
#. ¿Cuál de los dos archivos JSON utilizaría en una aplicación web y por qué?
#. ¿En qué casos elegiría una herramienta en línea y en qué casos un script para convertir datos? Considere el tamaño de los archivos, la repetición del proceso y la privacidad de los datos.

Referencias
===========

* Bray, T. (Ed.). (2017). The JavaScript Object Notation (JSON) Data Interchange Format (RFC 8259). IETF. https://www.rfc-editor.org/rfc/rfc8259
* CSVJSON. (n.d.). CSV to JSON. Retrieved November 9, 2026 from https://csvjson.com/csv2json
* GeeksforGeeks. (2025). Convert CSV to JSON using Python. Retrieved November 9, 2026 from https://www.geeksforgeeks.org/python/convert-csv-to-json-using-python/
* Online JSON Viewer. (n.d.). Online JSON Viewer. Retrieved November 9, 2026 from https://jsonviewer.stack.hu/
* Open-Meteo. (n.d.). Weather Forecast API. Retrieved November 9, 2026 from https://open-meteo.com/en/docs
* Python Software Foundation. (n.d.-a). csv — CSV File Reading and Writing. Retrieved November 9, 2026 from https://docs.python.org/3/library/csv.html
* Python Software Foundation. (n.d.-b). json — JSON encoder and decoder. Retrieved November 9, 2026 from https://docs.python.org/3/library/json.html
* Python Software Foundation. (n.d.-c). datetime — Basic date and time types. Retrieved November 9, 2026 from https://docs.python.org/3/library/datetime.html
* Shafranovich, Y. (2005). Common Format and MIME Type for Comma-Separated Values (CSV) Files (RFC 4180). IETF. https://www.rfc-editor.org/rfc/rfc4180