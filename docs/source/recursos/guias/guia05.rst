..
   Copyright (c) 2025 Allan Avendaño Sudario
   Licensed under Creative Commons Attribution-ShareAlike 4.0 International License
   SPDX-License-Identifier: CC-BY-SA-4.0

# Un diseño, dos caminos: construyendo la misma página con Flexbox y con Grid

```{admonition} Datos de la sesión
:class: note

- **Asignatura:** Desarrollo de aplicaciones web y móviles
- **Tema:** Maquetación con CSS Flexbox y CSS Grid
- **Duración:** 1 hora 30 minutos
- **Modalidad:** Taller práctico en laboratorio (individual o en parejas)
- **Herramientas:** Editor de código (VS Code recomendado), navegador basado en Chromium o Firefox con DevTools
```

## Resultados de aprendizaje

Al finalizar la sesión, el estudiante será capaz de:

1. Aplicar **Flexbox** para distribuir y alinear elementos a lo largo de **un eje**: filas o columnas.
2. Aplicar **Grid** para distribuir elementos en **dos dimensiones** mediante columnas, filas y áreas con nombre.
3. Construir un mismo diseño con ambas técnicas y comparar el código resultante.
4. Elegir con criterio la técnica más adecuada para cada componente de una interfaz.

## Dinámica de la clase

La sesión parte de una **página original**, *EcoRuta*: un sitio ficticio de turismo sostenible que solo tiene colores, tipografía y espaciado. **No tiene ninguna regla de distribución**, así que el navegador apila todos los elementos uno debajo de otro, en el flujo normal.

A partir de esa página, el estudiante construye **dos versiones** sin modificar el HTML ni la hoja `base.css`:

- `flexbox.html` + `css/flexbox.css`, que solo usa **Flexbox**.
- `grid.html` + `css/grid.css`, que solo usa **Grid**.

La meta es que **ambas versiones se vean exactamente iguales** al diseño objetivo. En cada actividad se resuelve la misma zona de la página primero con Flexbox y luego con Grid, y se comparan los resultados.

```{figure} _static/img/ecoruta_base.png
:alt: Página original de EcoRuta sin reglas de distribución; todos los elementos aparecen apilados
:width: 70%

Punto de partida: la página original. Solo tiene colores y tipografía, y todos los elementos aparecen apilados en el flujo normal.
```

```{figure} _static/img/ecoruta_objetivo_escritorio.png
:alt: Diseño objetivo de EcoRuta en escritorio, con cabecera, hero centrado, tres tarjetas, barra lateral, galería de 3 columnas y pie de 3 columnas
:width: 70%

Diseño objetivo en escritorio (1280 px). Ambas versiones, Flexbox y Grid, deben producir exactamente esta imagen.
```

```{list-table} Distribución del tiempo
:header-rows: 1
:widths: 8 62 15 15

* - N.º
  - Actividad
  - Zona de la página
  - Tiempo
* - 0
  - ¿Qué hace el navegador cuando no le damos instrucciones de distribución?
  - Página completa
  - 10 min
* - 1
  - ¿Cómo alineamos y centramos elementos en uno o en dos ejes?
  - Cabecera y *hero*
  - 15 min
* - 2
  - ¿Cómo logramos tarjetas de igual ancho e igual altura?
  - Tarjetas
  - 20 min
* - 3
  - ¿Cómo describimos la estructura general de la página?
  - Contenedor, barra lateral y pie
  - 15 min
* - 4
  - ¿Qué ocurre cuando los elementos deben ocupar varias filas?
  - Galería
  - 15 min
* - 5
  - ¿Cómo se adapta cada versión a una pantalla pequeña?
  - *Responsive*
  - 10 min
* - 
  - Cierre: ¿cuándo Flexbox, cuándo Grid?
  - 
  - 5 min
```

---

## Actividad 0. ¿Qué hace el navegador cuando no le damos instrucciones de distribución?

**Tiempo estimado:** 10 minutos

**Propósito:** Construir la página original, comprender el **flujo normal** del documento y preparar las dos versiones de trabajo.

### Paso 1. Crear la estructura del proyecto

```{code-block} text
:caption: Estructura de carpetas

ecoruta/
├── index.html      ← página original (solo base.css)
├── flexbox.html    ← base.css + flexbox.css
├── grid.html       ← base.css + grid.css
└── css/
    ├── base.css    ← colores, tipografía y espaciado (no se modifica)
    ├── flexbox.css ← vacío al inicio
    └── grid.css    ← vacío al inicio
```

### Paso 2. Escribir el HTML de la página original

```{code-block} html
:caption: index.html
:linenos:

<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>EcoRuta – Turismo sostenible</title>
  <link rel="stylesheet" href="css/base.css">
</head>
<body>
  <header class="cabecera">
    <a href="#" class="logo">EcoRuta</a>
    <nav class="menu" aria-label="Navegación principal">
      <ul>
        <li><a href="#">Inicio</a></li>
        <li><a href="#destinos">Destinos</a></li>
        <li><a href="#galeria">Galería</a></li>
        <li><a href="#contacto">Contacto</a></li>
      </ul>
    </nav>
  </header>

  <section class="hero">
    <div class="hero-contenido">
      <h1>Viaja sin dejar huella</h1>
      <p>Rutas sostenibles por la Costa, la Sierra, la Amazonía y Galápagos.</p>
      <a href="#destinos" class="boton">Explorar destinos</a>
    </div>
  </section>

  <div class="contenedor">
    <main class="principal">
      <h2 id="destinos">Destinos destacados</h2>
      <div class="tarjetas">
        <article class="tarjeta">
          <h3>Quilotoa</h3>
          <p>Caminata de borde de cráter con vista a la laguna turquesa.</p>
          <a href="#" class="enlace">Ver ruta</a>
        </article>
        <article class="tarjeta">
          <h3>Mindo</h3>
          <p>Bosque nublado, avistamiento de aves, mariposarios y cascadas
             escondidas entre la vegetación. Ideal para fotografía de naturaleza.</p>
          <a href="#" class="enlace">Ver ruta</a>
        </article>
        <article class="tarjeta">
          <h3>Yasuní</h3>
          <p>Navegación en canoa por la reserva de mayor biodiversidad.</p>
          <a href="#" class="enlace">Ver ruta</a>
        </article>
      </div>

      <h2 id="galeria">Galería</h2>
      <div class="galeria">
        <img src="https://picsum.photos/seed/eco1/400/300" alt="Laguna de montaña">
        <img src="https://picsum.photos/seed/eco2/400/300" alt="Sendero en el bosque">
        <img src="https://picsum.photos/seed/eco3/400/300" alt="Playa al atardecer">
        <img src="https://picsum.photos/seed/eco4/400/300" alt="Río amazónico">
        <img src="https://picsum.photos/seed/eco5/400/300" alt="Volcán nevado">
        <img src="https://picsum.photos/seed/eco6/400/300" alt="Cascada">
      </div>
    </main>

    <aside class="lateral">
      <h3>Próximas salidas</h3>
      <ul>
        <li>18 oct – Quilotoa</li>
        <li>25 oct – Mindo</li>
        <li>8 nov – Yasuní</li>
      </ul>
      <h3>Consejo verde</h3>
      <p>Lleva tu propia botella reutilizable.</p>
    </aside>
  </div>

  <footer class="pie" id="contacto">
    <div class="pie-columnas">
      <div class="columna">
        <h4>EcoRuta</h4>
        <p>Turismo responsable desde 2026.</p>
      </div>
      <div class="columna">
        <h4>Contacto</h4>
        <p>info@ecoruta.ec<br>+593 99 000 0000</p>
      </div>
      <div class="columna">
        <h4>Síguenos</h4>
        <p>Instagram · Facebook · TikTok</p>
      </div>
    </div>
    <p class="copy">© 2026 EcoRuta. Proyecto académico.</p>
  </footer>
</body>
</html>
```

```{note}
Las imágenes provienen de `picsum.photos`. Si el laboratorio no tiene Internet, la página sigue funcionando: cada imagen se muestra como un bloque verde claro, porque `base.css` le asigna un color de fondo.
```

### Paso 3. Escribir la hoja de estilos base

Esta hoja define **cómo se ve** cada elemento: colores, fuentes, rellenos y bordes. **No define dónde se ubica.** Ninguna de las dos versiones la modificará.

```{code-block} css
:caption: css/base.css
:linenos:

/* =========================================================
   base.css — Estilos visuales (colores, tipografía, espaciado)
   NO contiene ninguna regla de distribución (layout).
   ========================================================= */
* { box-sizing: border-box; }
body {
  margin: 0;
  font-family: system-ui, sans-serif;
  line-height: 1.5;
  color: #1f2933;
  background: #f5f7f2;
}
img { display: block; max-width: 100%; }
h2 { color: #1b4332; }

/* Cabecera */
.cabecera { min-height: 64px; padding: 0 2rem; background: #1b4332; }
.logo { color: #fff; font-size: 1.5rem; font-weight: 700; text-decoration: none; }
.menu ul { list-style: none; margin: 0; padding: 0; }
.menu a { color: #d8f3dc; text-decoration: none; }

/* Hero */
.hero {
  min-height: 320px;
  padding: 2rem 1rem;
  background: #2d6a4f;
  color: #fff;
  text-align: center;
}
.hero-contenido { width: 90%; max-width: 600px; }
.hero h1 { margin: 0 0 .5rem; font-size: 2.5rem; }
.boton {
  display: inline-block;
  margin-top: 1rem;
  padding: .75rem 1.5rem;
  border-radius: 4px;
  background: #95d5b2;
  color: #1b4332;
  font-weight: 600;
  text-decoration: none;
}

/* Contenedor, contenido principal y barra lateral */
.contenedor { max-width: 1100px; margin: 0 auto; padding: 2rem 1rem; }
.lateral { padding: 1rem; border-radius: 8px; background: #fff; }

/* Tarjetas */
.tarjetas { margin-bottom: 2rem; }
.tarjeta { padding: 1rem; border-radius: 8px; background: #fff; }
.tarjeta h3 { margin: 0 0 .5rem; }
.tarjeta p { margin: 0 0 1rem; }
.enlace { color: #2d6a4f; font-weight: 600; }

/* Galería */
.galeria img {
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
  border-radius: 6px;
  background: #b7e4c7;
}

/* Pie de página */
.pie { padding: 2rem; background: #1b4332; color: #d8f3dc; }
.pie-columnas { max-width: 1100px; margin: 0 auto; }
.columna h4 { margin: 0 0 .5rem; color: #fff; }
.copy {
  margin: 1.5rem 0 0;
  padding-top: 1rem;
  border-top: 1px solid #40916c;
  text-align: center;
}
```

### Paso 4. Preparar las dos versiones

1. Copie `index.html` dos veces y nombre las copias `flexbox.html` y `grid.html`.
2. En `flexbox.html`, agregue una segunda hoja de estilos debajo de `base.css`:

   ```{code-block} html
   <link rel="stylesheet" href="css/base.css">
   <link rel="stylesheet" href="css/flexbox.css">
   ```

3. En `grid.html`, haga lo mismo con `css/grid.css`.
4. Cree `css/flexbox.css` y `css/grid.css` vacíos.
5. Abra `flexbox.html` y `grid.html` en **dos ventanas lado a lado**. Durante toda la clase debe compararlas.

### Paso 5. Explorar el flujo normal

Con DevTools abierto (`F12` → *Elements*), seleccione `<header>`, `<a class="logo">`, `<ul>`, `<article>` e `<img>`. En el panel *Computed*, anote el valor de `display` de cada uno.

```{admonition} Preguntas guía y de reflexión
:class: tip

1. **Analizar:** ¿Por qué los elementos `<article>` aparecen uno debajo de otro, mientras que los enlaces `<a>` de un párrafo aparecen uno junto a otro? ¿Qué relación tiene esto con los valores `block` e `inline` de `display`?
2. **Analizar:** La hoja `base.css` no contiene ninguna regla de ubicación. ¿Qué ventaja tiene separar los estilos **visuales** de los estilos de **distribución** en archivos distintos?
3. **Comparar:** Observe la figura del diseño objetivo. Para cada zona (cabecera, *hero*, tarjetas, estructura principal, galería y pie), anote si el problema es de **una dimensión** (solo una fila o una columna) o de **dos dimensiones** (filas y columnas a la vez). Contraste su predicción al final de la clase.
4. **Investigar:** ¿Qué es el *flujo normal* (*normal flow*) según MDN? ¿Por qué se dice que Flexbox y Grid crean un nuevo **contexto de formato** para sus hijos?
```

---

## Actividad 1. ¿Cómo alineamos y centramos elementos en uno o en dos ejes?

**Tiempo estimado:** 15 minutos

**Propósito:** Distribuir la cabecera (logo a la izquierda y menú a la derecha) y centrar el contenido del *hero*, primero con Flexbox y luego con Grid.

### Paso 1. Cabecera y menú con Flexbox

```{code-block} css
:caption: css/flexbox.css — Cabecera
:linenos:

/* Actividad 1: cabecera y hero */
.cabecera {
  display: flex;                   /* los hijos se colocan en una fila */
  justify-content: space-between;  /* logo al inicio y menú al final del eje principal */
  align-items: center;             /* centrado en el eje transversal (vertical) */
}
.menu ul { display: flex; gap: 1.5rem; }
```

### Paso 2. Cabecera y menú con Grid

```{code-block} css
:caption: css/grid.css — Cabecera
:linenos:

/* Actividad 1: cabecera y hero */
.cabecera {
  display: grid;
  grid-template-columns: auto auto;  /* dos columnas tan anchas como su contenido */
  justify-content: space-between;    /* las columnas se separan a los extremos */
  align-items: center;
}
.menu ul { display: grid; grid-auto-flow: column; gap: 1.5rem; }
```

Compare ambas ventanas: la cabecera debe verse igual.

### Paso 3. Centrar el *hero* con Flexbox

```{code-block} css
:caption: css/flexbox.css — Hero
:linenos:

.hero {
  display: flex;
  justify-content: center;   /* centro horizontal (eje principal) */
  align-items: center;       /* centro vertical (eje transversal) */
}
```

### Paso 4. Centrar el *hero* con Grid

```{code-block} css
:caption: css/grid.css — Hero
:linenos:

.hero {
  display: grid;
  place-items: center;   /* centra en ambos ejes con una sola declaración */
}
```

### Paso 5. Visualizar los contenedores en DevTools

1. En *Elements*, haga clic en la insignia **`flex`** junto a `<header>` en `flexbox.html`, y en la insignia **`grid`** en `grid.html`.
2. Observe las superposiciones de color: en Flexbox se marcan los **ejes**; en Grid, las **líneas, filas y columnas**.
3. En `flexbox.html`, agregue temporalmente `flex-direction: column` a `.cabecera`. ¿Qué propiedad centra ahora horizontalmente: `justify-content` o `align-items`? Luego quite la declaración.

```{admonition} Preguntas guía y de reflexión
:class: tip

1. **Analizar:** En Flexbox, ¿por qué `justify-content` y `align-items` intercambian su efecto cuando cambia `flex-direction`? ¿Ocurre lo mismo en Grid?
2. **Comparar:** Para la cabecera, Flexbox necesitó solo `display: flex`, mientras que Grid necesitó **declarar las columnas** (`grid-template-columns: auto auto`). ¿Qué dice esto sobre quién decide el tamaño: el **contenido** (Flexbox) o el **contenedor** (Grid)?
3. **Analizar:** Agregue un quinto enlace al menú en ambas versiones. ¿Qué versión se adapta sin cambiar el CSS? ¿Por qué funciona `grid-auto-flow: column`?
4. **Investigar:** `place-items` es una propiedad abreviada (*shorthand*). ¿Qué dos propiedades agrupa? ¿Tiene efecto en un contenedor `flex`?
5. **Buenas prácticas:** ¿Por qué `gap` es preferible a poner `margin` en cada `<li>` para separar los enlaces del menú?
```

---

## Actividad 2. ¿Cómo logramos tarjetas de igual ancho e igual altura?

**Tiempo estimado:** 20 minutos

**Propósito:** Repartir el espacio entre tres tarjetas, igualar sus alturas y alinear los enlaces "Ver ruta" en la parte inferior, con ambas técnicas.

### Paso 1. Tarjetas en fila con Flexbox

```{code-block} css
:caption: css/flexbox.css — Tarjetas
:linenos:

/* Actividad 2: tarjetas */
.tarjetas { display: flex; gap: 1.5rem; }
.tarjeta { flex: 1; }   /* las tres tarjetas reparten el espacio por igual */
```

### Paso 2. Tarjetas en columnas con Grid

```{code-block} css
:caption: css/grid.css — Tarjetas
:linenos:

/* Actividad 2: tarjetas */
.tarjetas {
  display: grid;
  grid-template-columns: repeat(3, 1fr);   /* tres columnas de igual fracción */
  gap: 1.5rem;
}
```

Observe que, en ambas versiones, **las tres tarjetas tienen la misma altura**, aunque el texto de *Mindo* es más largo. ¿Por qué? En Flexbox, el valor por defecto de `align-items` es `stretch`. En Grid, también lo es: cada elemento se estira hasta la altura de su fila.

### Paso 3. Alinear los enlaces al fondo con Flexbox

Cada tarjeta puede ser, al mismo tiempo, **elemento** de un contenedor y **contenedor** de sus propios hijos:

```{code-block} css
:caption: css/flexbox.css — Interior de la tarjeta
:emphasize-lines: 3,4,6
:linenos:

.tarjeta {
  flex: 1;
  display: flex;
  flex-direction: column;   /* h3, p y enlace se apilan en vertical */
}
.enlace { margin-top: auto; }   /* el margen automático absorbe el espacio libre */
```

### Paso 4. Alinear los enlaces al fondo con Grid

```{code-block} css
:caption: css/grid.css — Interior de la tarjeta
:linenos:

.tarjeta {
  display: grid;
  grid-template-rows: auto 1fr auto;   /* título | párrafo flexible | enlace */
}
```

### Paso 5. Experimentar con el reparto del espacio

Cambie temporalmente los valores de la **segunda** tarjeta (*Mindo*) en DevTools y complete la tabla:

| Flexbox (`.tarjeta:nth-child(2)`) | Grid (`.tarjetas`) | ¿Qué sucede? |
|---|---|---|
| `flex: 2` | `grid-template-columns: 1fr 2fr 1fr` | |
| `flex: 0 0 200px` | `grid-template-columns: 1fr 200px 1fr` | |
| *(agregar una cuarta tarjeta en el HTML)* | *(agregar una cuarta tarjeta en el HTML)* | |

Restaure los valores originales antes de continuar.

```{admonition} Preguntas guía y de reflexión
:class: tip

1. **Analizar:** `flex: 1` es una forma abreviada. ¿Qué valores de `flex-grow`, `flex-shrink` y `flex-basis` representa? Verifíquelo en el panel *Computed*.
2. **Comparar:** Al agregar una cuarta tarjeta, ¿qué ocurrió en cada versión? En Flexbox, ¿quién decide cuántas columnas hay: el contenedor o la cantidad de hijos? ¿Y en Grid?
3. **Comparar:** Para fijar el enlace al fondo, Flexbox usó `margin-top: auto` en el **hijo** y Grid usó `grid-template-rows` en el **padre**. ¿Cuál de las dos soluciones le parece más explícita? ¿Cuál sería más fácil de mantener si la tarjeta tuviera una imagen adicional?
4. **Investigar:** ¿Qué es `subgrid`? ¿Qué problema resolvería si se quisiera que los **títulos** de las tres tarjetas también quedaran alineados entre sí, aunque tuvieran distinta cantidad de líneas?
5. **Buenas prácticas:** Ninguna versión usa alturas fijas (`height` o `min-height`) para igualar las tarjetas. ¿Por qué es una mala práctica fijar la altura de un componente cuyo contenido es dinámico?
```

---

## Actividad 3. ¿Cómo describimos la estructura general de la página?

**Tiempo estimado:** 15 minutos

**Propósito:** Construir el esqueleto de dos columnas (contenido principal y barra lateral de 260 px) y el pie de tres columnas.

### Paso 1. Estructura con Flexbox

```{code-block} css
:caption: css/flexbox.css — Estructura y pie
:linenos:

/* Actividad 3: estructura y pie */
.contenedor {
  display: flex;
  gap: 2rem;
  align-items: flex-start;   /* la barra lateral mide solo lo que necesita */
}
.principal { flex: 1; min-width: 0; }   /* ocupa todo el espacio restante */
.lateral { flex: 0 0 260px; }           /* ancho fijo: no crece ni se encoge */

.pie-columnas { display: flex; gap: 2rem; }
.columna { flex: 1; }
```

### Paso 2. Estructura con Grid y áreas con nombre

```{code-block} css
:caption: css/grid.css — Estructura y pie
:linenos:

/* Actividad 3: estructura y pie */
.contenedor {
  display: grid;
  grid-template-columns: 1fr 260px;
  grid-template-areas: "principal lateral";   /* el "plano" de la página */
  gap: 2rem;
  align-items: start;
}
.principal { grid-area: principal; min-width: 0; }
.lateral { grid-area: lateral; }

.pie-columnas {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
}
```

### Paso 3. Inspeccionar la cuadrícula

1. En `grid.html`, active la insignia **`grid`** de `.contenedor`.
2. En la pestaña *Layout* de DevTools, active **"Show area names"** y **"Show line numbers"**.
3. ¿Cuántas líneas verticales hay para dos columnas?

### Paso 4. Experimentar: mover la barra lateral a la izquierda

Intente llevar la barra lateral a la izquierda **sin tocar el HTML**:

- **Flexbox:** agregue `order: -1` a `.lateral`.
- **Grid:** cambie a `grid-template-columns: 260px 1fr` y `grid-template-areas: "lateral principal"`.

Restaure los valores originales.

```{admonition} Preguntas guía y de reflexión
:class: tip

1. **Analizar:** Quite temporalmente `align-items` en ambas versiones. ¿Qué le sucede a la altura de la barra lateral? Mencione un caso de diseño en el que ese estiramiento sí sea deseable.
2. **Comparar:** En Flexbox, el ancho de cada columna se declara en los **hijos** (`flex`). En Grid, se declara en el **padre** (`grid-template-columns`). ¿Qué enfoque permite entender la estructura completa leyendo una sola regla?
3. **Analizar:** ¿Para qué sirve `min-width: 0` en `.principal`? Elimínelo en ambas versiones y agregue al primer párrafo de una tarjeta una URL muy larga sin espacios. ¿Qué sucede con el ancho de la columna principal y con la barra lateral?
4. **Investigar:** Reordenar con `order` o con `grid-template-areas` cambia el orden **visual**, pero no el orden del HTML. Consulte la sección de accesibilidad de MDN: ¿qué problema puede causar esto para quien navega con teclado o con lector de pantalla?
5. **Buenas prácticas:** ¿Por qué se suele recomendar Grid para la **macroestructura** de una página y Flexbox para los **componentes** internos?
```

---

## Actividad 4. ¿Qué ocurre cuando los elementos deben ocupar varias filas?

**Tiempo estimado:** 15 minutos

**Propósito:** Construir una galería de seis imágenes en tres columnas y dos filas. Aquí se ve la diferencia fundamental entre una técnica **unidimensional** y una **bidimensional**.

### Paso 1. Galería con Flexbox

Flexbox coloca los elementos en **una sola línea**. Para tener varias filas, hay que permitir el salto de línea y **calcular** el ancho de cada imagen:

```{code-block} css
:caption: css/flexbox.css — Galería
:linenos:

/* Actividad 4: galería */
.galeria { display: flex; flex-wrap: wrap; gap: 1rem; }
.galeria img {
  flex: 0 0 calc((100% - 2 * 1rem) / 3);   /* (ancho − 2 huecos) ÷ 3 columnas */
  min-width: 0;   /* evita que la imagen imponga su ancho natural (400 px) */
}
```

### Paso 2. Galería con Grid

Grid define las columnas en el contenedor, y las filas se crean solas:

```{code-block} css
:caption: css/grid.css — Galería
:linenos:

/* Actividad 4: galería */
.galeria {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}
```

### Paso 3. Provocar la diferencia

En **ambas** versiones, realice las siguientes pruebas y anote lo que observa:

1. Elimine temporalmente `min-width: 0` en `flexbox.css`. ¿Qué pasa con las imágenes? Vuelva a escribirlo.
2. Elimine (o comente) la **sexta** imagen del HTML. Observe la última fila.
3. Restaure la imagen y agregue temporalmente esta regla a **ambas** hojas:

   ```{code-block} css
   .galeria img:first-child {
     grid-column: span 2;   /* solo tiene efecto en Grid */
     grid-row: span 2;
   }
   ```

   ¿Es posible lograr ese mismo efecto con Flexbox? Elimine la regla al terminar.

### Paso 4. Galería que se adapta sola (Grid)

En `grid.css`, pruebe esta definición y redimensione la ventana:

```{code-block} css
grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
```

Anote cuántas columnas aparecen a 1200, 768, 500 y 360 px. Luego **vuelva** a `repeat(3, 1fr)` para mantener la equivalencia con la versión Flexbox.

```{admonition} Preguntas guía y de reflexión
:class: tip

1. **Analizar:** En Flexbox, ¿por qué fue necesario usar `calc()` y considerar la cantidad de huecos (`gap`)? ¿Qué habría que recalcular si el diseño pidiera **cuatro** columnas? ¿Y en Grid?
2. **Analizar:** ¿Por qué, sin `min-width: 0`, la versión Flexbox se rompe? Investigue el concepto de **tamaño mínimo automático** (`min-width: auto`) de los elementos flexibles.
3. **Comparar:** Con cinco imágenes, ¿cómo quedó la última fila en cada versión? Si hubiera usado `flex: 1 1 200px` en lugar de un ancho fijo, ¿qué habría pasado con las imágenes de la última fila?
4. **Investigar:** ¿Qué diferencia hay entre `auto-fit` y `auto-fill` en `repeat()`? Pruebe ambos con solo dos imágenes en la galería.
5. **Buenas prácticas:** A partir de esta actividad, formule con sus palabras la diferencia entre una técnica **unidimensional** y una **bidimensional**.
```

---

## Actividad 5. ¿Cómo se adapta cada versión a una pantalla pequeña?

**Tiempo estimado:** 10 minutos

**Propósito:** Adaptar ambas versiones a pantallas de hasta 768 px y comparar el esfuerzo que requiere cada técnica.

```{figure} _static/img/ecoruta_objetivo_movil.png
:alt: Diseño objetivo de EcoRuta en pantalla pequeña, con todo en una columna y galería de 2 columnas
:width: 35%

Diseño objetivo en una pantalla pequeña (600 px): cabecera centrada, todo en una sola columna y galería de dos columnas.
```

### Paso 1. *Media query* en Flexbox

```{code-block} css
:caption: css/flexbox.css — Responsive
:linenos:

/* Actividad 5: adaptación a pantallas pequeñas */
@media (max-width: 768px) {
  .cabecera { flex-direction: column; gap: .5rem; padding: .5rem 1rem; }
  .contenedor { flex-direction: column; align-items: stretch; }
  .lateral { flex: none; }   /* sin esto, 260px se aplicaría a la ALTURA */
  .tarjetas, .pie-columnas { flex-direction: column; gap: 1rem; }
  .galeria img { flex-basis: calc((100% - 1rem) / 2); }
}
```

### Paso 2. *Media query* en Grid

```{code-block} css
:caption: css/grid.css — Responsive
:linenos:

/* Actividad 5: adaptación a pantallas pequeñas */
@media (max-width: 768px) {
  .cabecera {
    grid-template-columns: auto;
    justify-content: center;
    justify-items: center;
    gap: .5rem;
    padding: .5rem 1rem;
  }
  .contenedor {
    grid-template-columns: 1fr;
    grid-template-areas: "principal" "lateral";
  }
  .tarjetas, .pie-columnas { grid-template-columns: 1fr; gap: 1rem; }
  .galeria { grid-template-columns: repeat(2, 1fr); }
}
```

### Paso 3. Verificar la equivalencia

1. Active el modo dispositivo de DevTools (`Ctrl + Shift + M`) en ambas ventanas.
2. Compare las dos versiones a **1280 px**, **600 px** y **360 px** de ancho.
3. Anote cualquier diferencia y explique a qué se debe.

### Paso 4. Comparar el código

| Criterio | `flexbox.css` | `grid.css` |
|---|---|---|
| ¿Dónde se definen los tamaños: en el padre o en los hijos? | | |
| Uso de `calc()` | | |
| Reglas que fue necesario "deshacer" en la *media query* | | |
| Cambios necesarios para pasar de 3 a 4 columnas en la galería | | |
| Cambios necesarios para mover la barra lateral arriba en móvil | | |

```{admonition} Preguntas guía y de reflexión
:class: tip

1. **Analizar:** En la versión Flexbox, ¿por qué fue necesario `flex: none` en `.lateral` al cambiar a `flex-direction: column`? ¿Qué le sucede a `flex-basis` cuando cambia la dirección del eje principal?
2. **Comparar:** Reduzca la ventana a 320 px. ¿Qué ocurre con el menú en cada versión? ¿Cuál permite que los enlaces salten a otra línea con solo agregar una propiedad (`flex-wrap: wrap`)? ¿Qué tendría que hacer en Grid?
3. **Analizar:** Ambas versiones producen el mismo resultado. ¿Significa eso que son igual de adecuadas? Justifique zona por zona: cabecera, tarjetas, estructura y galería.
4. **Investigar:** Consulte *Can I use* para `gap` en Flexbox, `grid-template-areas`, `subgrid` y `aspect-ratio`. ¿Hay alguna limitación de compatibilidad que deba considerar hoy?
5. **Buenas prácticas:** ¿Sería válido combinar ambas técnicas en un mismo proyecto? Proponga una tercera hoja, `mixto.css`, indicando qué técnica usaría en cada zona y por qué.
```

---

## Cierre: ¿cuándo Flexbox, cuándo Grid?

**Tiempo estimado:** 5 minutos

El profesor guía una puesta en común a partir de la siguiente tabla, construida con los aportes del grupo:

| Zona | Solución con Flexbox | Solución con Grid | Técnica más natural |
|---|---|---|---|
| Cabecera y menú | `justify-content: space-between` | `grid-template-columns: auto auto` | Flexbox (una fila, tamaño dado por el contenido) |
| Centrado del *hero* | `justify-content` + `align-items` | `place-items: center` | Ambas |
| Tarjetas | `flex: 1` + `margin-top: auto` | `repeat(3, 1fr)` + `grid-template-rows` | Ambas; Grid si se requiere alinear filas internas |
| Estructura principal | `flex: 1` / `flex: 0 0 260px` | `grid-template-areas` | Grid (estructura declarada en el padre) |
| Galería | `flex-wrap` + `calc()` + `min-width: 0` | `repeat(3, 1fr)` o `auto-fit` | Grid (dos dimensiones) |

```{admonition} Idea clave
:class: important

**Flexbox** trabaja de **adentro hacia afuera**: el contenido de los elementos determina cómo se reparte el espacio en **un eje**. **Grid** trabaja de **afuera hacia adentro**: el contenedor define una estructura de **filas y columnas**, y los elementos se ubican en ella. No compiten entre sí, sino que se complementan.
```

### Tarea autónoma sugerida

Agregue una cuarta tarjeta (*Galápagos*) y dos imágenes más a la galería en ambas versiones. Documente, con capturas de pantalla, qué reglas tuvo que modificar en `flexbox.css` y en `grid.css` para conservar un diseño equilibrado.

---

## Anexo: hojas de estilo finales de referencia

```{code-block} css
:caption: css/flexbox.css — Versión completa
:linenos:

/* =========================================================
   flexbox.css — Distribución construida solo con Flexbox
   ========================================================= */

/* Actividad 1: cabecera y hero */
.cabecera {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.menu ul { display: flex; gap: 1.5rem; }
.hero {
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Actividad 2: tarjetas */
.tarjetas { display: flex; gap: 1.5rem; }
.tarjeta {
  flex: 1;
  display: flex;
  flex-direction: column;
}
.enlace { margin-top: auto; }

/* Actividad 3: estructura y pie */
.contenedor {
  display: flex;
  gap: 2rem;
  align-items: flex-start;
}
.principal { flex: 1; min-width: 0; }
.lateral { flex: 0 0 260px; }
.pie-columnas { display: flex; gap: 2rem; }
.columna { flex: 1; }

/* Actividad 4: galería */
.galeria { display: flex; flex-wrap: wrap; gap: 1rem; }
.galeria img {
  flex: 0 0 calc((100% - 2 * 1rem) / 3);
  min-width: 0;
}

/* Actividad 5: adaptación a pantallas pequeñas */
@media (max-width: 768px) {
  .cabecera { flex-direction: column; gap: .5rem; padding: .5rem 1rem; }
  .contenedor { flex-direction: column; align-items: stretch; }
  .lateral { flex: none; }
  .tarjetas, .pie-columnas { flex-direction: column; gap: 1rem; }
  .galeria img { flex-basis: calc((100% - 1rem) / 2); }
}
```

```{code-block} css
:caption: css/grid.css — Versión completa
:linenos:

/* =========================================================
   grid.css — Distribución construida solo con Grid
   ========================================================= */

/* Actividad 1: cabecera y hero */
.cabecera {
  display: grid;
  grid-template-columns: auto auto;
  justify-content: space-between;
  align-items: center;
}
.menu ul { display: grid; grid-auto-flow: column; gap: 1.5rem; }
.hero {
  display: grid;
  place-items: center;
}

/* Actividad 2: tarjetas */
.tarjetas {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1.5rem;
}
.tarjeta {
  display: grid;
  grid-template-rows: auto 1fr auto;
}

/* Actividad 3: estructura y pie */
.contenedor {
  display: grid;
  grid-template-columns: 1fr 260px;
  grid-template-areas: "principal lateral";
  gap: 2rem;
  align-items: start;
}
.principal { grid-area: principal; min-width: 0; }
.lateral { grid-area: lateral; }
.pie-columnas {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 2rem;
}

/* Actividad 4: galería */
.galeria {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 1rem;
}

/* Actividad 5: adaptación a pantallas pequeñas */
@media (max-width: 768px) {
  .cabecera {
    grid-template-columns: auto;
    justify-content: center;
    justify-items: center;
    gap: .5rem;
    padding: .5rem 1rem;
  }
  .contenedor {
    grid-template-columns: 1fr;
    grid-template-areas: "principal" "lateral";
  }
  .tarjetas, .pie-columnas { grid-template-columns: 1fr; gap: 1rem; }
  .galeria { grid-template-columns: repeat(2, 1fr); }
}
```

---

## Referencias

Andrew, R. (2017). *The new CSS layout*. A Book Apart.

Andrew, R. (2021). *Learn CSS: Flexbox*. web.dev. https://web.dev/learn/css/flexbox

Andrew, R. (2021). *Learn CSS: Grid*. web.dev. https://web.dev/learn/css/grid

Atkins, T., Jr., Etemad, E. J., & Atanassov, R. (2018). *CSS Flexible Box Layout Module Level 1* (W3C Candidate Recommendation). World Wide Web Consortium. https://www.w3.org/TR/css-flexbox-1/

Atkins, T., Jr., Etemad, E. J., Atanassov, R., & Brufau, O. (2020). *CSS Grid Layout Module Level 2* (W3C Candidate Recommendation). World Wide Web Consortium. https://www.w3.org/TR/css-grid-2/

Coyier, C. (2013). *A complete guide to Flexbox*. CSS-Tricks. https://css-tricks.com/snippets/css/a-guide-to-flexbox/

Deveria, A. (s. f.). *Can I use: Support tables for HTML5, CSS3, etc.* https://caniuse.com/

House, C. (2016). *A complete guide to CSS Grid*. CSS-Tricks. https://css-tricks.com/snippets/css/complete-guide-grid/

MDN Web Docs. (s. f.-a). *Basic concepts of flexbox*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout/Basic_concepts_of_flexbox

MDN Web Docs. (s. f.-b). *Basic concepts of grid layout*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Basic_concepts_of_grid_layout

MDN Web Docs. (s. f.-c). *Relationship of grid layout to other layout methods*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Relationship_of_grid_layout_with_other_layout_methods

MDN Web Docs. (s. f.-d). *Grid layout and accessibility*. Mozilla. https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout/Grid_layout_and_accessibility

Park, T. (s. f.). *Flexbox Froggy* y *Grid Garden* [Juegos educativos]. Codepip. https://codepip.com/
