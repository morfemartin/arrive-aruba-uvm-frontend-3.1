<h1 align="center">ARRIVE ARUBA · MARKETPLACE — EDICIÓN CSS <span style="color:#F5A169">✦</span></h1>

<p align="center">
  Marketplace de ropa resort construido con HTML y CSS, con efectos visuales sobre la identidad
  de la marca Arrive Aruba (proyecto propio del autor) y una base de datos en el navegador.<br>
  <strong>e-actividad 3.1 · Diseño web con HTML & CSS · Frontend</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/HTML5-sem%C3%A1ntico-E34F26?logo=html5&logoColor=white" alt="HTML5">
  <img src="https://img.shields.io/badge/CSS3-efectos%20y%20tokens-1572B6?logo=css3&logoColor=white" alt="CSS3">
  <img src="https://img.shields.io/badge/JavaScript-nativo%20m%C3%ADnimo-F7DF1E?logo=javascript&logoColor=black" alt="JavaScript nativo">
  <img src="https://img.shields.io/badge/Base%20de%20datos-localStorage-386395" alt="localStorage">
  <a href="https://github.com/morfemartin/arrive-aruba-uvm-frontend-3.1/actions/workflows/validate.yml"><img src="https://github.com/morfemartin/arrive-aruba-uvm-frontend-3.1/actions/workflows/validate.yml/badge.svg" alt="Validación HTML en GitHub Actions"></a>
</p>

<p align="center">
  🛬 <a href="https://morfemartin.github.io/arrive-aruba-uvm-frontend-3.1/"><strong>Ver el sitio publicado en GitHub Pages</strong></a>
</p>

---

## Contexto académico

| Campo | Valor |
|---|---|
| Asignatura | Frontend (FrontEndIVIVFI2026C) |
| Docente | Prof. Anggelo Huz |
| Actividad | e-actividad 3.1 — Diseño web con HTML & CSS (10 %) |
| Entrega | 31 de octubre de 2026, 23:59 |
| Modalidad | Equipo de 2 integrantes |

## Equipo y reparto de trabajo

| Integrante | Responsabilidad principal | Huella en los commits |
|---|---|---|
| Martin Alejandro Morfe Carballo | Sistema de diseño (tokens), página de inicio, página de producto, base de datos y documentación técnica | `.gitignore`, `css/base.css`, `css/componentes.css`, `index.html`, `css/inicio.css`, `producto.html`, `css/producto.css`, `js/bd.js`, `historial.html`, `js/historial.js`, `css/historial.css`, `README.md`, workflow, árboles DOM/CSSOM e informe |
| Martin Morfe Flores.°. | Catálogo, formulario de reserva con validación, página nosotros y mockups de diseño | `catalogo.html`, `css/catalogo.css`, `formulario.html`, `js/formulario.js`, `css/formulario.css`, `nosotros.html`, `css/nosotros.css`, `docs/mockups/` |

Cada archivo lleva un comentario de cabecera con la autoría, siguiendo la regla de la actividad.

## Descripción

**Arrive Aruba** es una marca de ropa resort nacida en Aruba. Este repositorio adapta su
identidad visual — paleta zafiro/crema, tipografía editorial y la narrativa de la
«llegada a la isla»— a un marketplace estático de seis páginas, construido para demostrar
el uso de las propiedades esenciales del CSS.

No es la tienda comercial: no procesa pagos. Las reservas se guardan en la base de datos
del navegador (Web Storage API) y cada entrega se revisa página por página.

## Características principales

- **Seis páginas** enlazadas: inicio (landing), catálogo, producto, formulario de reserva,
  nosotros e historial de pedidos.
- **Sistema de diseño con custom properties**: toda la paleta, espaciados, sombras y
  movimiento viven como tokens en `css/base.css`.
- **La ventana del avión**: hero con la fotografía real de Aruba mezclada con el degradado
  del atardecer (`background-blend-mode`), más el sol y el vaivén animados en CSS.
- **Banda de atmósfera**: la fotografía del hero de la marca a pantalla completa con
  movimiento lento *Ken Burns* (`@keyframes`) y velo degradado (`mix-blend-mode`) para que
  el lema se lea siempre.
- **Editorial de marca en la ficha**: un visor de tres vistas que cambia con `:checked`,
  sin JavaScript, con pulgares flotantes sobre la fotografía (`backdrop-filter`).
- **Efectos CSS** en cada interacción: elevación de tarjetas, subrayado animado del menú,
  chips de filtro, desenfoque de la cabecera (`backdrop-filter`) y fotografías tratadas
  con `filter` y `object-fit`.
- **Base de datos real en el navegador**: el formulario valida y guarda cada reserva con
  `localStorage`; la página «Mis pedidos» lista, muestra y elimina registros.
- **Comentarios explicativos** en el código donde la lógica lo amerita.
- **Validación automática**: cada push ejecuta el W3C Nu Html Checker en GitHub Actions.

## Estructura del repositorio

```text
arrive-aruba-uvm-frontend-3.1/
├── index.html            ← landing: hero con ventana, banda de atmósfera, destacados, categorías, testimonios
├── catalogo.html         ← catálogo con filtros por categoría
├── producto.html          ← ficha de la prenda con visor de vistas (:checked)
├── formulario.html       ← reserva: formulario con validación → base de datos
├── nosotros.html         ← historia, valores y proceso de la marca
├── historial.html        ← «Mis pedidos»: lectura de la base de datos
├── css/
│   ├── base.css          ← tokens de diseño, reset y tipografía
│   ├── componentes.css   ← header, nav, botones, tarjetas, formularios, pie
│   ├── inicio.css        ← estilos de la página de inicio
│   ├── catalogo.css      ← estilos del catálogo
│   ├── producto.css      ← estilos de la ficha de producto
│   ├── formulario.css    ← estilos del formulario de reserva
│   ├── nosotros.css      ← estilos de la página nosotros
│   └── historial.css     ← estilos del historial de pedidos
├── js/
│   ├── bd.js             ← módulo de base de datos (localStorage)
│   ├── formulario.js     ← validación y guardado de reservas
│   └── historial.js      ← dibujado y borrado de pedidos
├── assets/
│   └── img/              ← fotografías de la marca: paisaje del hero y editoriales de la colección
└── docs/
    ├── arbol-dom.md      ← árbol DOM del HTML (entregable)
    ├── arbol-cssom.md    ← árbol CSSOM (entregable)
    ├── informe.md        ← informe de desarrollo (entregable)
    └── mockups/          ← mockups SVG importados a Figma (entregable)
```

## Cómo ver el proyecto — paso a paso

**Opción A · Abrir directamente (la más rápida)**

1. Clona o descarga este repositorio (verde **Code → Download ZIP** si no usas Git).
2. Descomprime la carpeta y entra en ella.
3. Haz doble clic en `index.html`: el sitio se abre en tu navegador.
4. Navega con el menú superior: Catálogo, Reserva, Nosotros y Mis pedidos.

**Opción B · Servidor local (recomendada para probar la base de datos)**

```sh
git clone https://github.com/morfemartin/arrive-aruba-uvm-frontend-3.1.git
cd arrive-aruba-uvm-frontend-3.1
python3 -m http.server 8000
```

Abre `http://localhost:8000` en tu navegador.

**Comprobar la base de datos (1 minuto)**

1. Ve a **Reserva**, completa el formulario y envíalo.
2. Abre **Mis pedidos**: tu reserva aparece listada con fecha y detalles.
3. Recarga la página (F5): la reserva sigue ahí — vive en `localStorage`.
4. Para verla «por dentro»: DevTools (F12) → pestaña **Application** →
   **Local Storage** → clave `arrive-aruba-pedidos`.

> Las tipografías se cargan de Google Fonts; sin conexión el sitio sigue
> funcionando con las fuentes de respaldo del sistema.

## Navegación

| Página | Qué contiene |
|---|---|
| `index.html` | Hero con la ventana de la marca, prendas destacadas, categorías, materialidad y testimonios |
| `catalogo.html` | Rejilla de productos con filtros por categoría y efectos hover |
| `producto.html` | Ficha técnica de la Camisa Sunset Wharf, tallas y prendas relacionadas |
| `formulario.html` | Formulario de reserva con validación: guarda en la base de datos |
| `nosotros.html` | Historia de la marca, valores y proceso de confección |
| `historial.html` | Las reservas guardadas, con detalle y opción de eliminar |

## Propiedades CSS aplicadas (mapa de la actividad)

| Propiedad / técnica | Dónde verla |
|---|---|
| Custom properties (variables CSS) | `css/base.css` — tokens de toda la identidad |
| Grid y Flexbox | `css/componentes.css` (rejillas, alineaciones) y páginas |
| `position: sticky` | Cabecera fija al hacer scroll |
| `backdrop-filter` | Desenfoque de la cabecera sobre el contenido |
| Gradientes (`linear/radial-gradient`) | Atmósfera del hero y azulejos de productos |
| `transition` y `transform` | Elevación de tarjetas, botones y chips |
| `@keyframes` (animaciones) | Ventana del hero, aparición escalonada, estrella del estado vacío |
| Pseudo-clases (`:hover`, `:focus-visible`, `:checked`) | Menú, foco accesible, tallas radiales |
| Pseudo-elementos (`::after`, `::selection`) | Subrayado del menú, selección de texto |
| `clamp()` y `aspect-ratio` | Tipografía fluida y proporciones de figuras |
| `object-fit` | Fotografías encuadradas sin deformarse (banda de atmósfera, visor) |
| `background-blend-mode` y `mix-blend-mode` | Foto real mezclada con el atardecer del hero; velo sobre la banda |
| `filter` en fotografías | Saturación y contraste afinados sobre los medios de marca |
| Ken Burns con `@keyframes` | Movimiento lento de la fotografía en la banda de atmósfera |
| Galería de vistas con `:checked` | Ficha de producto: cambia de vista sin JavaScript |
| `@media (prefers-reduced-motion)` | Respeto por la preferencia de movimiento reducido |

## Entregables documentales

- **Árbol DOM:** [`docs/arbol-dom.md`](docs/arbol-dom.md)
- **Árbol CSSOM:** [`docs/arbol-cssom.md`](docs/arbol-cssom.md)
- **Informe de desarrollo:** [`docs/informe.md`](docs/informe.md)
- **Mockups y estructura de diseño:** `docs/mockups/` (SVG importados a Figma)
- **Archivo de Figma con las seis maquetas:** [Arrive Aruba · e-actividad 3.1 — Figma](https://www.figma.com/design/BidcCGWUPRx9ITaIYS9Xxc/Arrive-Aruba-%C2%B7-e-actividad-3.1?node-id=0-1)

## Mockups en Figma

Las seis maquetas del sitio se consolidaron en Figma a partir de los SVG de `docs/mockups/`:

**[Arrive Aruba · e-actividad 3.1 — archivo de Figma](https://www.figma.com/design/BidcCGWUPRx9ITaIYS9Xxc/Arrive-Aruba-%C2%B7-e-actividad-3.1?node-id=0-1)**

> Si el enlace de Figma solicita permiso de acceso, las maquetas también pueden
> verse directamente en la carpeta `docs/mockups/` del repositorio.

## Procedencia de los recursos

La identidad visual (paleta, tipografía y narrativa) proviene del proyecto personal de
marca del autor **Arrive Aruba**, adaptada y simplificada a mano para esta actividad.
No se usó ninguna plantilla. Las fotografías (paisaje del hero, atmósfera y editoriales)
provienen del archivo visual propio de la marca; el favicon y las siluetas de las prendas
son gráficos propios dibujados en SVG y CSS. Las tipografías (Cormorant Garamond y
Geologica) son fuentes libres servidas desde Google Fonts con licencia OFL.

## Estado

| Etapa | Estado |
|---|---|
| e-actividad 3.1 (HTML & CSS) | ✅ Entregada — tag `e3.1` |
| Validación W3C | ✅ Automatizada en cada push (GitHub Actions) |
| Sitio publicado | ✅ GitHub Pages — rama `main` desde la raíz |

