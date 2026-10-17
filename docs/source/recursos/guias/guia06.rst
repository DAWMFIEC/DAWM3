..
   Copyright (c) 2026 Allan Avendaño
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

===============================================================
Guía 06: Introducción a JavaScript con la extensión WikiTracker
===============================================================

.. topic:: Objetivo específico
   :class: objetivo

   Construir una extensión de Google Chrome con Manifest V3 que registre las interacciones del usuario en artículos de Wikipedia y las presente en un dashboard, mediante la ejecución, observación y análisis de scripts JavaScript que usan variables, funciones, objetos, eventos del DOM, mensajes entre contextos y almacenamiento en el navegador, sin frameworks ni servidores externos.

Actividades en clases
=====================

¿Qué necesita Chrome para reconocer WikiTracker como una extensión?
-------------------------------------------------------------------

1. Cree una carpeta llamada `wikitracker` y, dentro de ella, los archivos `manifest.json`, `popup.html` y `popup.js` con el siguiente contenido.

   .. code-block:: json
      :caption: manifest.json

      {
        "manifest_version": 3,
        "name": "WikiTracker",
        "version": "1.0.0",
        "description": "Registra tu recorrido por Wikipedia y lo muestra en un dashboard.",
        "action": {
          "default_popup": "popup.html",
          "default_title": "WikiTracker"
        }
      }

   .. code-block:: html
      :caption: popup.html

      <!DOCTYPE html>
      <html lang="es-EC">
      <head>
        <meta charset="UTF-8">
        <title>WikiTracker</title>
      </head>
      <body style="width: 300px; font-family: system-ui, sans-serif;">
        <h1>📚 WikiTracker</h1>
        <p id="bienvenida"></p>
        <script src="popup.js"></script>
      </body>
      </html>

   .. code-block:: javascript
      :caption: popup.js

      'use strict';

      const NOMBRE = 'WikiTracker';
      const version = chrome.runtime.getManifest().version;
      let mensaje;

      const hoy = new Date();
      const fechaLegible = hoy.toLocaleDateString('es-EC', {
        weekday: 'long',
        day: 'numeric',
        month: 'long',
      });

      if (hoy.getHours() < 12) {
        mensaje = 'Buenos días';
      } else {
        mensaje = 'Buenas tardes';
      }

      const bienvenida = document.querySelector('#bienvenida');
      bienvenida.textContent = `${mensaje}. Hoy es ${fechaLegible}. ${NOMBRE} v${version}`;

      console.log('[WikiTracker] popup cargado', { NOMBRE, version, hora: hoy.getHours() });

2. Abra `chrome://extensions`, active el **Modo de desarrollador**, pulse **Cargar descomprimida** y seleccione la carpeta `wikitracker`. Fije la extensión con el ícono 🧩, haga clic en ella, luego clic derecho dentro del popup → **Inspeccionar** → pestaña **Console**. Ejecute una por una las siguientes instrucciones y anote lo que obtiene.

   .. code-block:: javascript

      typeof NOMBRE
      typeof hoy.getHours()
      typeof bienvenida
      typeof noExiste
      NOMBRE = 'Otro'
      '5' + 3
      '5' * 3
      chrome.runtime.getManifest()

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué información de `manifest.json` reconoce Chrome al cargar la extensión? ¿Qué ocurre si elimina la clave ``"manifest_version"`` y vuelve a cargarla?
2. ¿Por qué cree que algunas variables se declararon con ``const`` y otra con ``let``? ¿Qué sucedió al intentar reasignar ``NOMBRE``?
3. ¿Qué diferencias observa entre los resultados de ``'5' + 3`` y ``'5' * 3``? ¿Qué riesgo podría implicar este comportamiento al leer datos escritos por un usuario?
4. Si cambia ``"version"`` a ``"1.0.1"`` en el manifiesto y recarga la extensión, ¿qué cambia en el popup? ¿Por qué no fue necesario modificar `popup.js`?
5. ¿Qué ventaja o desventaja tendría usar ``innerHTML`` en lugar de ``textContent`` para mostrar el mensaje de bienvenida?
6. ¿Qué ocurre si agrega ``<script>console.log('hola')</script>`` directamente dentro de `popup.html`? ¿Por qué cree que Chrome se comporta así con las extensiones?

¿Cómo detecta WikiTracker el artículo que leo y lo que hago en él?
------------------------------------------------------------------

1. Modifique `manifest.json` para registrar un *content script* que se ejecute en los artículos de Wikipedia. Las líneas resaltadas corresponden a la modificación.

   .. code-block:: json
      :caption: manifest.json
      :emphasize-lines: 10-16

      {
        "manifest_version": 3,
        "name": "WikiTracker",
        "version": "1.0.0",
        "description": "Registra tu recorrido por Wikipedia y lo muestra en un dashboard.",
        "action": {
          "default_popup": "popup.html",
          "default_title": "WikiTracker"
        },
        "content_scripts": [
          {
            "matches": ["https://*.wikipedia.org/wiki/*"],
            "js": ["content.js"],
            "run_at": "document_idle"
          }
        ]
      }

2. Cree el archivo `content.js` con el siguiente código.

   .. code-block:: javascript
      :caption: content.js

      'use strict';

      function obtenerIdioma(hostname) {
        return hostname.split('.')[0];
      }

      function tituloDesdeRuta(ruta) {
        const fragmento = ruta.replace('/wiki/', '');
        return decodeURIComponent(fragmento).replaceAll('_', ' ');
      }

      function obtenerTitulo() {
        const encabezado = document.querySelector('#firstHeading');
        return encabezado ? encabezado.textContent.trim() : tituloDesdeRuta(location.pathname);
      }

      function contarPalabras(texto) {
        return texto.trim().split(/\s+/).filter(Boolean).length;
      }

      function esArticulo() {
        return document.body.classList.contains('ns-0');
      }

      const contenido = document.querySelector('#mw-content-text');

      const articulo = {
        titulo: obtenerTitulo(),
        idioma: obtenerIdioma(location.hostname),
        url: location.origin + location.pathname,
        palabras: contarPalabras(contenido?.innerText ?? ''),
      };

      function registrar(tipo, datos = {}) {
        const evento = {
          tipo,
          articulo: articulo.titulo,
          idioma: articulo.idioma,
          fecha: new Date().toISOString(),
          ...datos,
        };
        console.log('[WikiTracker]', evento);
      }

      function porcentajeScroll() {
        const recorrible = document.documentElement.scrollHeight - window.innerHeight;
        if (recorrible <= 0) return 100;
        return Math.min(100, Math.round((window.scrollY / recorrible) * 100));
      }

      function iniciarSeguimiento() {
        let inicioVisible = document.visibilityState === 'visible' ? Date.now() : null;
        let tiempoAcumulado = 0;
        let scrollMaximo = porcentajeScroll();

        registrar('visita', { url: articulo.url, palabras: articulo.palabras });

        window.addEventListener('scroll', () => {
          scrollMaximo = Math.max(scrollMaximo, porcentajeScroll());
        }, { passive: true });

        contenido?.addEventListener('click', (evento) => {
          const enlace = evento.target.closest('a[href^="/wiki/"]');
          if (!enlace) return;

          const destino = tituloDesdeRuta(enlace.pathname);
          if (destino.includes(':')) return;

          registrar('enlace', { destino });
        });

        document.addEventListener('visibilitychange', () => {
          if (document.visibilityState === 'hidden') {
            if (inicioVisible !== null) {
              tiempoAcumulado += Date.now() - inicioVisible;
              inicioVisible = null;
            }
            registrar('lectura', { ms: tiempoAcumulado, scroll: scrollMaximo });
            tiempoAcumulado = 0;
          } else {
            inicioVisible = Date.now();
          }
        });
      }

      if (esArticulo()) {
        console.log('[WikiTracker] Artículo detectado:', articulo);
        iniciarSeguimiento();
      } else {
        console.log('[WikiTracker] Página ignorada:', location.pathname);
      }

3. Recargue la extensión con el botón ⟳ de `chrome://extensions`. Visite cada página de la tabla con la consola abierta (**F12** → **Console**) y complete las columnas.

   .. list-table:: Páginas visitadas con WikiTracker
      :header-rows: 1
      :widths: 40 20 20 20

      * - Página
        - ¿Aparece un mensaje de WikiTracker?
        - ¿Artículo detectado o página ignorada?
        - Valor de ``idioma``
      * - ``https://es.wikipedia.org/wiki/Ecuador``
        -
        -
        -
      * - ``https://en.wikipedia.org/wiki/Galápagos_Islands``
        -
        -
        -
      * - ``https://es.wikipedia.org/wiki/Especial:Aleatoria``
        -
        -
        -
      * - ``https://es.wikipedia.org/wiki/Categoría:Provincias_de_Ecuador``
        -
        -
        -
      * - ``https://es.wikipedia.org/wiki/Discusión:Guayaquil``
        -
        -
        -
      * - ``https://www.google.com``
        -
        -
        -

4. En el artículo `Ecuador`, active **Preserve log** en la configuración de la consola. Baje hasta la mitad del artículo, cambie a otra pestaña durante 10 segundos, regrese, haga clic en el enlace a `Quito` y capture la consola con los eventos registrados.

   .. figure:: ../img/wikitracker_consola_eventos.png
      :alt: Consola de Chrome sobre el artículo Ecuador de Wikipedia con los eventos visita, lectura y enlace registrados por WikiTracker
      :align: center

      Eventos registrados por el *content script* en la consola del artículo.

5. Provoque dos casos de error y anote lo que observa en cada uno:

   a. Cambie el nombre de `content.js` a `contenido.js` sin modificar el manifiesto y recargue la extensión en `chrome://extensions`. Luego restaure el nombre original.
   b. Visite un artículo que no existe, por ejemplo ``https://es.wikipedia.org/wiki/Articulo_inexistente_WikiTracker``, y revise los valores de ``titulo`` y ``palabras`` en la consola.

6. Con base en lo observado, complete su propia definición:

   .. centered:: Un *content script* es ______________________ y permite ______________________.

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué relación encuentra entre el patrón ``"matches"`` del manifiesto, la función ``esArticulo`` y los resultados de la tabla del paso 3?
2. Al comparar los eventos de la figura con las acciones que realizó en el paso 4, ¿en qué momento exacto se registra cada evento y qué información aporta cada uno sobre su forma de leer?

¿Cómo guarda WikiTracker mi recorrido y lo presenta en un dashboard?
--------------------------------------------------------------------

1. Regrese al artículo de la actividad anterior, despliegue en la consola uno de los objetos ``evento`` y observe sus propiedades.

   .. figure:: ../img/wikitracker_objeto_evento.png
      :alt: Objeto evento de tipo lectura desplegado en la consola de Chrome con las propiedades tipo, articulo, idioma, fecha, ms y scroll
      :align: center

      Estructura de un evento generado en la actividad anterior.

2. Reemplace `manifest.json` por la versión final y cree el archivo `background.js`.

   .. code-block:: json
      :caption: manifest.json
      :emphasize-lines: 10-13

      {
        "manifest_version": 3,
        "name": "WikiTracker",
        "version": "1.0.0",
        "description": "Registra tu recorrido por Wikipedia y lo muestra en un dashboard.",
        "action": {
          "default_popup": "popup.html",
          "default_title": "WikiTracker"
        },
        "permissions": ["storage"],
        "background": {
          "service_worker": "background.js"
        },
        "content_scripts": [
          {
            "matches": ["https://*.wikipedia.org/wiki/*"],
            "js": ["content.js"],
            "run_at": "document_idle"
          }
        ]
      }

   .. code-block:: javascript
      :caption: background.js

      'use strict';

      const MAX_EVENTOS = 5000;
      let cola = Promise.resolve();

      chrome.runtime.onInstalled.addListener(({ reason }) => {
        console.log('[WikiTracker] extensión instalada o actualizada:', reason);
      });

      async function guardarEvento(evento) {
        const { eventos = [], pausado = false } = await chrome.storage.local.get(['eventos', 'pausado']);

        if (pausado) return { ok: false, motivo: 'registro pausado' };
        if (!evento?.tipo || !evento?.articulo) return { ok: false, motivo: 'evento incompleto' };

        const nuevo = { id: crypto.randomUUID(), ...evento };
        const actualizados = [...eventos, nuevo].slice(-MAX_EVENTOS);

        await chrome.storage.local.set({ eventos: actualizados });
        return { ok: true, total: actualizados.length };
      }

      chrome.runtime.onMessage.addListener((mensaje, remitente, responder) => {
        if (mensaje?.accion !== 'registrar') return false;

        console.log(`[WikiTracker] ${mensaje.evento?.tipo} desde la pestaña ${remitente.tab?.id}`);

        cola = cola
          .then(() => guardarEvento(mensaje.evento))
          .then(responder, (error) => responder({ ok: false, motivo: String(error) }));

        return true;
      });

3. En `content.js`, reemplace la función ``registrar`` por la siguiente versión. Las líneas resaltadas corresponden a la modificación.

   .. code-block:: javascript
      :caption: content.js (función registrar)
      :emphasize-lines: 1,10-15

      async function registrar(tipo, datos = {}) {
        const evento = {
          tipo,
          articulo: articulo.titulo,
          idioma: articulo.idioma,
          fecha: new Date().toISOString(),
          ...datos,
        };

        try {
          const respuesta = await chrome.runtime.sendMessage({ accion: 'registrar', evento });
          console.log('[WikiTracker] enviado:', tipo, respuesta);
        } catch (error) {
          console.warn('[WikiTracker] no se pudo enviar el evento:', error.message);
        }
      }

4. Reemplace `popup.html` y `popup.js`, y cree `popup.css`.

   .. code-block:: html
      :caption: popup.html

      <!DOCTYPE html>
      <html lang="es-EC">
      <head>
        <meta charset="UTF-8">
        <title>WikiTracker</title>
        <link rel="stylesheet" href="popup.css">
      </head>
      <body>
        <header>
          <h1>📚 WikiTracker</h1>
          <p id="bienvenida"></p>
        </header>

        <main>
          <p id="resumen" class="resultado" aria-live="polite">Cargando…</p>

          <label>
            <input id="pausar" type="checkbox"> Pausar el registro
          </label>

          <button id="abrirDashboard" type="button">Abrir dashboard</button>
        </main>

        <script src="popup.js"></script>
      </body>
      </html>

   .. code-block:: css
      :caption: popup.css

      body {
        width: 300px;
        margin: 0;
        padding: 12px;
        font-family: system-ui, sans-serif;
        font-size: 14px;
      }
      h1 { font-size: 18px; margin: 0 0 4px; }
      .resultado { background: #f1f5ff; padding: 8px; border-radius: 6px; }
      label { display: flex; gap: 6px; align-items: center; margin: 8px 0; }
      button { font: inherit; padding: 6px 10px; width: 100%; cursor: pointer; }

   .. code-block:: javascript
      :caption: popup.js

      'use strict';

      const NOMBRE = 'WikiTracker';
      const version = chrome.runtime.getManifest().version;
      const hoy = new Date();
      const mensaje = hoy.getHours() < 12 ? 'Buenos días' : 'Buenas tardes';

      document.querySelector('#bienvenida').textContent = `${mensaje}. ${NOMBRE} v${version}`;

      const resumen = document.querySelector('#resumen');
      const checkPausar = document.querySelector('#pausar');

      function esDeHoy(fechaISO) {
        return new Date(fechaISO).toDateString() === new Date().toDateString();
      }

      async function mostrarResumen() {
        try {
          const { eventos = [], pausado = false } = await chrome.storage.local.get(['eventos', 'pausado']);
          const visitasHoy = eventos.filter((e) => e.tipo === 'visita' && esDeHoy(e.fecha)).length;

          resumen.textContent =
            `Hoy has abierto ${visitasHoy} artículo(s). ` +
            `Registros guardados: ${eventos.length}.`;
          checkPausar.checked = pausado;
        } catch (error) {
          console.error('[WikiTracker]', error);
          resumen.textContent = 'No se pudieron leer los datos.';
        }
      }

      checkPausar.addEventListener('change', async () => {
        await chrome.storage.local.set({ pausado: checkPausar.checked });
      });

      document.querySelector('#abrirDashboard').addEventListener('click', () => {
        chrome.tabs.create({ url: chrome.runtime.getURL('dashboard.html') });
      });

      mostrarResumen();

5. Cree los archivos del dashboard: `dashboard.html`, `dashboard.css`, `estadisticas.js` y `dashboard.js`.

   .. code-block:: html
      :caption: dashboard.html

      <!DOCTYPE html>
      <html lang="es-EC">
      <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>WikiTracker · Dashboard</title>
        <link rel="stylesheet" href="dashboard.css">
      </head>
      <body>
        <header>
          <h1>📚 WikiTracker</h1>
          <p id="actualizado" aria-live="polite"></p>
        </header>

        <main>
          <section class="tarjetas" aria-label="Indicadores principales">
            <article class="tarjeta"><h2>Artículos únicos</h2><p id="kpiArticulos">0</p></article>
            <article class="tarjeta"><h2>Tiempo de lectura</h2><p id="kpiTiempo">0</p></article>
            <article class="tarjeta"><h2>Enlaces seguidos</h2><p id="kpiEnlaces">0</p></article>
            <article class="tarjeta"><h2>Scroll promedio</h2><p id="kpiScroll">0 %</p></article>
          </section>

          <section class="panel">
            <h2>Artículos con más tiempo de lectura</h2>
            <ul id="topArticulos" class="barras"></ul>
          </section>

          <section class="panel">
            <h2>Visitas por idioma</h2>
            <ul id="idiomas" class="barras"></ul>
          </section>

          <section class="panel ancho">
            <h2>Mi recorrido reciente</h2>
            <ol id="recorrido" class="recorrido"></ol>
          </section>

          <section class="panel ancho">
            <h2>Últimos registros</h2>
            <div class="tabla-scroll">
              <table>
                <thead>
                  <tr><th>Fecha</th><th>Tipo</th><th>Artículo</th><th>Detalle</th></tr>
                </thead>
                <tbody id="tablaEventos"></tbody>
              </table>
            </div>
          </section>
        </main>

        <footer>
          <button id="exportar" type="button">Exportar JSON</button>
          <button id="borrar" type="button" class="peligro">Borrar todos los datos</button>
        </footer>

        <script src="estadisticas.js"></script>
        <script src="dashboard.js"></script>
      </body>
      </html>

   .. code-block:: css
      :caption: dashboard.css

      :root {
        --fondo: #f6f7fb;
        --superficie: #ffffff;
        --texto: #1d2433;
        --suave: #5b6478;
        --acento: #3366cc;
        --peligro: #b3261e;
      }

      * { box-sizing: border-box; }

      body {
        margin: 0;
        padding: 16px;
        font-family: system-ui, sans-serif;
        background: var(--fondo);
        color: var(--texto);
      }

      header, main, footer { max-width: 1100px; margin-inline: auto; }
      h1 { margin: 0; }
      h2 { font-size: 1rem; margin: 0 0 12px; color: var(--suave); }
      #actualizado { color: var(--suave); margin: 4px 0 16px; }

      main {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
        gap: 16px;
      }

      .tarjetas {
        grid-column: 1 / -1;
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
        gap: 16px;
      }

      .tarjeta, .panel {
        background: var(--superficie);
        border-radius: 12px;
        padding: 16px;
        box-shadow: 0 1px 3px rgb(0 0 0 / 0.08);
      }

      .tarjeta p { font-size: 2rem; font-weight: 700; margin: 0; }
      .ancho { grid-column: 1 / -1; }

      .barras { list-style: none; margin: 0; padding: 0; display: grid; gap: 8px; }
      .barras li {
        display: grid;
        grid-template-columns: minmax(0, 1fr) 2fr auto;
        align-items: center;
        gap: 8px;
      }
      .etiqueta { overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
      .pista { background: #e6e9f2; border-radius: 6px; height: 12px; }
      .relleno { background: var(--acento); height: 100%; border-radius: 6px; transition: width 0.4s; }
      .cifra { font-variant-numeric: tabular-nums; color: var(--suave); }

      .recorrido { display: flex; flex-wrap: wrap; gap: 6px; list-style: none; margin: 0; padding: 0; }
      .recorrido li { background: #e8efff; padding: 4px 10px; border-radius: 999px; }
      .recorrido li + li::before { content: '→ '; color: var(--suave); }

      .tabla-scroll { overflow-x: auto; }
      table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
      th, td { text-align: left; padding: 6px 8px; border-bottom: 1px solid #e6e9f2; }

      footer { display: flex; gap: 8px; justify-content: flex-end; margin-top: 16px; }
      button { font: inherit; padding: 8px 14px; border-radius: 8px; border: 1px solid #c9cfdd; background: var(--superficie); cursor: pointer; }
      .peligro { color: var(--peligro); border-color: var(--peligro); }
      .vacio { color: var(--suave); font-style: italic; }

   .. code-block:: javascript
      :caption: estadisticas.js

      'use strict';

      function filtrarPorTipo(eventos, tipo) {
        return eventos.filter((evento) => evento.tipo === tipo);
      }

      function sumar(numeros) {
        return numeros.reduce((acumulado, n) => acumulado + n, 0);
      }

      function articulosUnicos(eventos) {
        const claves = filtrarPorTipo(eventos, 'visita').map((e) => `${e.idioma}:${e.articulo}`);
        return new Set(claves).size;
      }

      function tiempoTotal(eventos) {
        return sumar(filtrarPorTipo(eventos, 'lectura').map((e) => e.ms));
      }

      function agruparYSumar(lista, obtenerClave, obtenerValor = () => 1) {
        return lista.reduce((grupos, item) => {
          const clave = obtenerClave(item);
          grupos[clave] = (grupos[clave] ?? 0) + obtenerValor(item);
          return grupos;
        }, {});
      }

      function tiempoPorArticulo(eventos) {
        return agruparYSumar(filtrarPorTipo(eventos, 'lectura'), (e) => e.articulo, (e) => e.ms);
      }

      function visitasPorIdioma(eventos) {
        return agruparYSumar(filtrarPorTipo(eventos, 'visita'), (e) => e.idioma);
      }

      function topN(objeto, n = 5) {
        return Object.entries(objeto)
          .sort(([, a], [, b]) => b - a)
          .slice(0, n);
      }

      function scrollPromedio(eventos) {
        const maximos = filtrarPorTipo(eventos, 'lectura').reduce((acc, e) => {
          acc[e.articulo] = Math.max(acc[e.articulo] ?? 0, e.scroll);
          return acc;
        }, {});
        const valores = Object.values(maximos);
        return valores.length ? Math.round(sumar(valores) / valores.length) : 0;
      }

      function recorrido(eventos, n = 10) {
        return filtrarPorTipo(eventos, 'visita').slice(-n).map((e) => e.articulo);
      }

      function formatearDuracion(ms) {
        const totalSegundos = Math.round(ms / 1000);
        const horas = Math.floor(totalSegundos / 3600);
        const minutos = Math.floor((totalSegundos % 3600) / 60);
        const segundos = totalSegundos % 60;
        return horas > 0
          ? `${horas} h ${minutos} min`
          : `${minutos} min ${String(segundos).padStart(2, '0')} s`;
      }

      function calcularResumen(eventos) {
        return {
          articulos: articulosUnicos(eventos),
          enlaces: filtrarPorTipo(eventos, 'enlace').length,
          tiempo: tiempoTotal(eventos),
          scroll: scrollPromedio(eventos),
        };
      }

   .. code-block:: javascript
      :caption: dashboard.js

      'use strict';

      const $ = (selector) => document.querySelector(selector);

      function crearElemento(etiqueta, texto, clase) {
        const elemento = document.createElement(etiqueta);
        if (texto !== undefined) elemento.textContent = texto;
        if (clase) elemento.className = clase;
        return elemento;
      }

      function crearBarra(etiqueta, valor, maximo, textoValor) {
        const li = document.createElement('li');
        const pista = crearElemento('div', undefined, 'pista');
        const relleno = crearElemento('div', undefined, 'relleno');

        relleno.style.width = `${maximo > 0 ? (valor / maximo) * 100 : 0}%`;
        pista.append(relleno);

        li.title = `${etiqueta}: ${textoValor}`;
        li.append(crearElemento('span', etiqueta, 'etiqueta'), pista, crearElemento('span', textoValor, 'cifra'));
        return li;
      }

      function renderizarBarras(contenedor, pares, formatear) {
        if (pares.length === 0) {
          contenedor.replaceChildren(crearElemento('li', 'Aún no hay datos.', 'vacio'));
          return;
        }
        const maximo = pares[0][1];
        contenedor.replaceChildren(...pares.map(([clave, valor]) => crearBarra(clave, valor, maximo, formatear(valor))));
      }

      function describirEvento(evento) {
        switch (evento.tipo) {
          case 'enlace':  return `→ ${evento.destino}`;
          case 'lectura': return `${formatearDuracion(evento.ms)} · ${evento.scroll} %`;
          case 'visita':  return `${evento.palabras ?? '?'} palabras · ${evento.idioma}`;
          default:        return '';
        }
      }

      function crearFila(evento) {
        const tr = document.createElement('tr');
        const fecha = new Date(evento.fecha).toLocaleString('es-EC', { dateStyle: 'short', timeStyle: 'short' });

        for (const valor of [fecha, evento.tipo, evento.articulo, describirEvento(evento)]) {
          tr.append(crearElemento('td', valor));
        }
        return tr;
      }

      function renderizar(eventos) {
        const resumen = calcularResumen(eventos);

        $('#kpiArticulos').textContent = resumen.articulos;
        $('#kpiTiempo').textContent = formatearDuracion(resumen.tiempo);
        $('#kpiEnlaces').textContent = resumen.enlaces;
        $('#kpiScroll').textContent = `${resumen.scroll} %`;

        renderizarBarras($('#topArticulos'), topN(tiempoPorArticulo(eventos)), formatearDuracion);
        renderizarBarras($('#idiomas'), topN(visitasPorIdioma(eventos), 10), (v) => `${v} visita(s)`);

        $('#recorrido').replaceChildren(...recorrido(eventos).map((titulo) => crearElemento('li', titulo)));
        $('#tablaEventos').replaceChildren(...eventos.slice(-20).reverse().map(crearFila));

        $('#actualizado').textContent =
          `${eventos.length} registros · actualizado a las ${new Date().toLocaleTimeString('es-EC')}`;
      }

      async function cargarYRenderizar() {
        const { eventos = [] } = await chrome.storage.local.get('eventos');
        renderizar(eventos);
      }

      chrome.storage.onChanged.addListener((cambios, area) => {
        if (area === 'local' && cambios.eventos) {
          renderizar(cambios.eventos.newValue ?? []);
        }
      });

      $('#exportar').addEventListener('click', async () => {
        const { eventos = [] } = await chrome.storage.local.get('eventos');
        const archivo = new Blob([JSON.stringify(eventos, null, 2)], { type: 'application/json' });
        const url = URL.createObjectURL(archivo);

        const enlace = crearElemento('a');
        enlace.href = url;
        enlace.download = `wikitracker-${new Date().toISOString().slice(0, 10)}.json`;
        enlace.click();
        setTimeout(() => URL.revokeObjectURL(url), 1000);
      });

      $('#borrar').addEventListener('click', async () => {
        if (!confirm('¿Seguro que desea borrar todo su historial de WikiTracker?')) return;
        await chrome.storage.local.remove('eventos');
      });

      cargarYRenderizar();

6. Recargue la extensión y las pestañas de Wikipedia. Visite al menos cinco artículos siguiendo enlaces, abra el popup y pulse **Abrir dashboard**. Coloque el dashboard y un artículo en dos ventanas lado a lado y siga otro enlace. Luego, en `chrome://extensions`, abra la consola del **service worker** y ejecute:

   .. code-block:: javascript

      chrome.storage.local.get(null).then(console.log);

7. Complete la tabla con su hipótesis antes de investigar y con la definición después de consultar la documentación oficial.

   .. list-table:: Componentes de WikiTracker: hipótesis frente a investigación
      :header-rows: 1
      :widths: 30 35 35

      * - Componente
        - ¿Qué cree que significa?
        - Definición después de investigar
      * - ``"service_worker": "background.js"``
        -
        -
      * - ``chrome.runtime.sendMessage``
        -
        -
      * - ``return true`` en ``onMessage``
        -
        -
      * - ``chrome.storage.local``
        -
        -
      * - ``async`` / ``await``
        -
        -
      * - ``cola.then(...)``
        -
        -
      * - ``reduce``
        -
        -
      * - ``new Set(...)``
        -
        -
      * - ``chrome.storage.onChanged``
        -
        -
      * - ``Blob``
        -
        -

8. Observe el esquema del flujo de datos de WikiTracker y complete los bloques y las flechas vacías.

   .. figure:: ../img/wikitracker_esquema_flujo.png
      :alt: Esquema para completar con cinco bloques vacíos A a E conectados por flechas numeradas del 1 al 4 que representan el flujo de datos de WikiTracker
      :align: center

      Esquema del flujo de datos de WikiTracker para completar.

   * ¿Qué archivo de la extensión corresponde al bloque **A**, el único que se ejecuta dentro de la página de Wikipedia?
   * ¿Qué parte de la extensión representa el bloque **B** y en qué consola puede observar sus mensajes?
   * ¿Qué API de Chrome se usa en la flecha **1** y qué objeto viaja por ella?
   * ¿Dónde quedan guardados los datos en el bloque **C** y qué instrucción usó para consultarlos?
   * ¿Qué archivos corresponden a los bloques **D** y **E**, y qué evento hace que el bloque **E** se actualice sin recargar la página?

9. Investigue cada caso, ejecútelo en la consola que corresponda (artículo de Wikipedia, popup o dashboard) y clasifíquelo.

   .. list-table:: Clasificación de expresiones usadas en WikiTracker
      :header-rows: 1
      :widths: 50 25 25

      * - Caso
        - Tipo de dato que devuelve
        - ¿Síncrono o asíncrono?
      * - ``location.hostname.split('.')``
        -
        -
      * - ``document.querySelector('#firstHeading')``
        -
        -
      * - ``document.body.classList.contains('ns-0')``
        -
        -
      * - ``Date.now()``
        -
        -
      * - ``new Set(['es', 'es', 'en']).size``
        -
        -
      * - ``topN({ Ecuador: 120000, Quito: 45000 })``
        -
        -
      * - ``chrome.storage.local.get('eventos')``
        -
        -
      * - ``chrome.runtime.sendMessage({ accion: 'otra' })``
        -
        -
      * - ``JSON.stringify({ tipo: 'visita' })``
        -
        -

Preguntas guía y de reflexión
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. ¿Qué cambió en la consola del artículo después de reemplazar la función ``registrar``? ¿Qué información nueva aparece en la respuesta de cada evento?
2. ¿Qué ocurre con los datos del dashboard si cierra Chrome por completo y lo vuelve a abrir? ¿Qué parte del código explica ese comportamiento?
3. Abra cinco artículos a la vez con clic central. Luego elimine temporalmente la variable ``cola`` y llame directamente a ``guardarEvento(...).then(responder)``. Repita la prueba. ¿Coincide el número de visitas registradas en ambos casos? ¿A qué atribuye la diferencia?
4. Ejecute en la consola del dashboard ``chrome.storage.local.set({ eventos: [{ tipo: 'visita', articulo: '<img src=x onerror=alert(1)>', idioma: 'es', fecha: new Date().toISOString() }] })``. ¿Qué muestra el dashboard? ¿Qué habría pasado si ``crearElemento`` usara ``innerHTML``?
5. WikiTracker mide qué lee una persona, durante cuánto tiempo y hasta dónde. ¿Qué podría deducirse de ese historial? ¿Qué debería saber el usuario antes de instalar una extensión así y qué cambiaría si los datos se enviaran a un servidor?

Referencias
===========

* Extensions / Get started. (n.d.). Chrome for Developers. Retrieved October 16, 2026, from https://developer.chrome.com/docs/extensions/get-started
* JavaScript. (n.d.). MDN Web Docs. Retrieved October 16, 2026, from https://developer.mozilla.org/es/docs/Web/JavaScript
