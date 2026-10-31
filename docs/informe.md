# Informe de desarrollo — e-actividad 3.1

**Proyecto:** Arrive Aruba · Marketplace (Edición CSS)
**Asignatura:** Frontend (FrontEndIVIVFI2026C) · **Docente:** Prof. Anggelo Huz
**Actividad:** e-actividad 3.1 — Diseño web con HTML &amp; CSS (10 %)
**Equipo:** Martin Alejandro Morfe Carballo · Martin Morfe Flores.°.
**Fecha de entrega:** 31 de octubre de 2026

---

## 1 · Resumen

Se construyó un marketplace de ropa resort de seis páginas sobre la identidad
de la marca Arrive Aruba (proyecto propio del primer autor), aplicando las
propiedades esenciales de CSS: sistema de tokens con custom properties,
maquetación con Grid y Flexbox, efectos con `transition`/`transform`,
animaciones con `@keyframes` y una base de datos del navegador (Web Storage
API) que almacena las reservas del formulario. El sitio incorpora además la
fotografía propia de la marca (carpeta `assets/`), presentada con CSS puro:
mezcla con `background-blend-mode`, encuadre con `object-fit`, movimiento
Ken Burns con `@keyframes` y velos degradados. El sitio es estático: se abre
sin instalar nada y se publica tal cual en GitHub.

## 2 · Cómo funciona la web

El sitio tiene seis páginas con un armazón común (cabecera fija, contenido
principal y pie). Los estilos se organizan en **tres capas** que se cargan
en cascada en todas las páginas:

1. **`css/base.css`** — el sistema de diseño: tokens de color, espaciado,
   sombras, movimiento y tipografía, más el reset y las utilidades.
2. **`css/componentes.css`** — los componentes compartidos: cabecera,
   navegación, botones, tarjetas, chips de filtro, campos de formulario,
   tablas y pie.
3. **`css/{pagina}.css`** — una hoja fina por página con sus estilos
   exclusivos (hero de inicio, rejilla del catálogo, ficha del producto…).

Las **fotografías de la marca** viven en `assets/img/` y se insertan desde
el HTML; su presentación — mezcla con la paleta, encuadre, movimiento y
velos — es enteramente CSS, como se detalla en la sección 3.6.

**Flujo de una reserva, de punta a punta:** el visitante llega a la landing
(`index.html`), recorre el catálogo (`catalogo.html`), entra al detalle de
la prenda destacada (`producto.html`) y desde allí —o desde el menú— llega
al formulario (`formulario.html`). El formulario valida los datos en el
navegador; al enviar, `js/formulario.js` llama al módulo `js/bd.js`, que
guarda la reserva en `localStorage`. La página `historial.html` lee esa
misma base de datos y dibuja cada reserva con su botón de eliminar.

## 3 · La actividad a nivel de código

### 3.1 · Tokens de diseño (`css/base.css`)

Toda la identidad vive en un solo bloque `:root`. Cambiar un valor aquí
repinta el sitio completo — así se mantiene la coherencia visual:

```css
:root {
  --azul-zafiro: #386395;
  --crema-cannoli: #f0f0e5;
  --tinta: #1c2b3a;           /* texto: zafiro oscurecido, nunca negro puro */
  --sombra-hover: 0 18px 40px rgba(56, 99, 149, 0.22);
  --curva-suave: cubic-bezier(0.22, 0.61, 0.36, 1);
}
```

Las sombras usan el zafiro de la marca (nunca negro), y el movimiento se
estandarizó con una sola curva `ease-out` calma.

### 3.2 · La ventana del avión (`css/inicio.css`)

La ventanilla del avión es el motivo de marca. Hoy su interior muestra la
**fotografía real de Aruba** mezclada con el degradado del atardecer
mediante `background-blend-mode`; el sol y el vaivén siguen siendo CSS:

```css
.ventana__mar {
  border-radius: 50%;
  background:
    radial-gradient(circle at 62% 30%, rgba(245, 161, 105, 0.55) 0%, transparent 26%),
    linear-gradient(180deg, #5f8bb8 0%, #386395 46%, #2c4e78 62%, #1c2b3a 100%),
    url("../assets/img/aruba-hero.jpg") center / cover no-repeat;
  background-blend-mode: screen, soft-light, normal;
  animation: mecerse 12s ease-in-out infinite alternate;
}
```

Si la fotografía aún no carga, los dos degradados dibujan el mismo
atardecer: la ventanilla nunca queda vacía.

### 3.3 · El módulo de base de datos (`js/bd.js`)

`localStorage` guarda pares clave–valor; el módulo serializa el arreglo de
pedidos a JSON y expone cuatro operaciones (`listar`, `guardar`,
`eliminar`, `total`). El guardado agrega identificador y fecha:

```js
guardar(pedido) {
  var pedidos = leerTodo();
  var registro = {
    id: generarId(),
    fecha: new Date().toLocaleString("es-VE", { dateStyle: "medium", timeStyle: "short" }),
    /* …datos del formulario… */
  };
  pedidos.push(registro);
  escribirTodo(pedidos);   /* localStorage.setItem(CLAVE, JSON.stringify(...)) */
}
```

### 3.4 · Dibujado del historial (`js/historial.js`)

Las tarjetas de «Mis pedidos» se construyen con `createElement` y
`textContent` — nunca `innerHTML` con datos del usuario — y la eliminación
usa **delegación de eventos**: un solo escuchador en el contenedor atiende
los botones de todas las tarjetas:

```js
contenedor.addEventListener("click", function (evento) {
  var boton = evento.target.closest(".pedido__eliminar");
  if (!boton) return;
  var tarjeta = boton.closest(".pedido");
  if (tarjeta && confirm("¿Eliminar esta reserva?")) {
    BD.eliminar(tarjeta.getAttribute("data-id"));
    tarjeta.classList.add("pedido--saliendo");
    tarjeta.addEventListener("transitionend", renderizar, { once: true });
  }
});
```

### 3.5 · Validación del formulario (`js/formulario.js` — colaborador)

El formulario revisa campo por campo antes de guardar: campos obligatorios
no vacíos, formato de correo, teléfono de al menos 7 dígitos, prenda y talla
elegidas, cantidad entre 1 y 10. Cada error se muestra junto a su campo
(`.campo--invalido` + mensaje) sin recargar la página; al pasar la
validación se llama `BD.guardar(...)` y se muestra el mensaje de éxito con
el enlace a «Mis pedidos». El formulario precarga la prenda cuando se llega
desde la ficha del producto (parámetro \`?prenda=\` en la URL).

### 3.6 · La fotografía de marca presentada con CSS

El archivo visual de Arrive Aruba aporta paisaje, atmósfera y editoriales.
Todos los medios entran como **contenido** desde `assets/img/`, y el CSS
los presenta en tres piezas:

1. **Banda de atmósfera del inicio**: la fotografía del hero de la marca
   cubre la pantalla con `object-fit: cover` y se mueve despacio con un
   Ken Burns (`@keyframes` de escala y traslación). Un velo degradado con
   `mix-blend-mode: multiply` garantiza la lectura del lema sobre
   cualquier zona de la foto, y con `prefers-reduced-motion` la imagen
   queda en reposo.
2. **Visor del editorial en la ficha**: tres vistas apiladas que se funden
   por opacidad; el radio `:checked` decide cuál sube al frente — la
   misma técnica sin JavaScript de los filtros del catálogo. Los pulgares
   flotan sobre la fotografía con `backdrop-filter`.
3. **Las tarjetas del catálogo conservan las siluetas SVG propias de la
   marca**: el archivo visual no incluye fotografía de producto por prenda,
   y los gráficos dibujados a mano mantienen la rejilla uniforme.

## 4 · Problemas hallados y cómo se resolvieron

| # | Problema | Solución |
|---|---|---|
| 1 | La cabecera fija (`position: sticky`) tapaba el destino del enlace «saltar al contenido»: el ancla quedaba escondida tras la barra. | Se compensó con `scroll-margin-top` en el contenido principal, para que el navegador deje aire al saltar. |
| 2 | Los módulos ES (`import`/`export`) no funcionan abriendo el sitio con `file://` (política de origen del navegador). | Se usaron scripts clásicos y un objeto global `window.BD`: el sitio funciona con doble clic, sin servidor. |
| 3 | `localStorage` puede contener JSON dañado (por ejemplo, si el usuario lo edita a mano en DevTools) y romper la página al arrancar. | La lectura se protege con `try/catch`: ante datos inválidos, la base arranca limpia en lugar de fallar. |
| 4 | Una primera versión del historial dibujaba las tarjetas con `innerHTML`, lo que permitiría inyectar HTML desde los campos del formulario. | Se reescribió con `createElement` + `textContent`: el texto del usuario nunca se interpreta como marcado. |
| 5 | El archivo visual de la marca no tiene fotografía de producto por prenda: montar los pocos editoriales en las ocho tarjetas las haría repetitivas y desiguales. | Las tarjetas conservan las siluetas SVG propias de marca; la fotografía real se presenta en la ventana del hero, la banda de atmósfera y el visor del editorial, donde habla de la colección en contexto. |
| 6 | El texto sobre crema perdía legibilidad con tonos medios de la paleta. | Se definió una tinta propia (zafiro muy oscurecido, `#1c2b3a`) para contraste alto — el texto nunca es negro puro ni color de acento. |
| 7 | Los pulgares del visor debían leerse sobre la fotografía grande sin taparla. | Se montó una banda flotante con `backdrop-filter` y `z-index: 1`, anclada abajo del panel: la foto respira detrás y el control queda siempre visible. |
| 8 | El hero del proyecto matriz de la marca anima su atmósfera con una secuencia de 36 fotogramas dirigida por JavaScript; esta actividad pide HTML y CSS. | La atmósfera se recreó con un Ken Burns (`@keyframes`) sobre la fotografía aprobada de marca, y `prefers-reduced-motion` deja la imagen fija: movimiento elegante sin una línea de JS. |

## 5 · Propiedades CSS cubiertas

El mapa completo, propiedad por propiedad y con el archivo donde verla,
está en el [README del repositorio](../README.md). El resumen: custom
properties, Grid, Flexbox, `position: sticky`, `backdrop-filter`,
degradados, `transition`, `transform`, `@keyframes` (incluido el Ken Burns
de la atmósfera), pseudo-clases de interacción (`:hover`, `:focus-visible`,
`:checked`), pseudo-elementos, `clamp()`, `aspect-ratio`, `object-fit`,
`background-blend-mode`, `mix-blend-mode`, `filter` y
`@media (prefers-reduced-motion)`.

## 6 · Reparto del trabajo (evaluación individual)

| Integrante | Trabajo realizado |
|---|---|
| **Martin Alejandro Morfe Carballo** | Sistema de diseño y tokens (`base.css`), componentes compartidos, página de inicio con la ventana CSS, ficha de producto, módulo de base de datos `bd.js`, página «Mis pedidos» (`historial.js`), README, workflow de validación, árboles DOM/CSSOM y este informe. |
| **Martin Morfe Flores.°.** | Catálogo con filtros por categoría, formulario de reserva con validación (`formulario.js`), página nosotros y mockups de diseño para Figma. |

Cada archivo del repositorio declara su autoría en el comentario de cabecera.

## 7 · Conclusiones

- Un sistema de tokens convirtió la identidad de marca en un sitio mantenible:
  la cascada de CSS, bien ordenada (base → componentes → página), evita peleas
  de especificidad.
- La Web Storage API demuestra el concepto de persistencia sin servidor: la
  base de datos del navegador basta para una reserva académica y deja el
  patrón listo para migrar a un backend real en actividades futuras.
- Construir sin imágenes obligó a exprimir el CSS (degradados, formas,
  animaciones) y produjo un resultado más ligero y más «de marca».
- La validación automática en GitHub Actions disciplina el proceso: cada push
  revisa el HTML con el validador del W3C antes de dar por buena la entrega.

## 8 · Referencias (normas APA, 7.ª edición)

Google. (2026). *Google Fonts*. https://fonts.google.com/

MDN Web Docs. (2026). *Using the Web Storage API*. Mozilla.
https://developer.mozilla.org/es/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API

MDN Web Docs. (2026). *backdrop-filter*. Mozilla.
https://developer.mozilla.org/es/docs/Web/CSS/backdrop-filter

MDN Web Docs. (2026). *object-fit*. Mozilla.
https://developer.mozilla.org/es/docs/Web/CSS/object-fit

MDN Web Docs. (2026). *mix-blend-mode*. Mozilla.
https://developer.mozilla.org/es/docs/Web/CSS/mix-blend-mode

MDN Web Docs. (2026). *Usando custom properties (variables) en CSS*. Mozilla.
https://developer.mozilla.org/es/docs/Web/CSS/Using_CSS_custom_properties

World Wide Web Consortium. (2026). *CSS Snapshot 2026*. W3C.
https://www.w3.org/TR/CSS/

WHATWG. (2026). *HTML: Living Standard*. https://html.spec.whatwg.org/

