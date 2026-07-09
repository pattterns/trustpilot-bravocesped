# Widget de Trustpilot para Bravo Césped — Instrucciones

Widget "casero" (sin dependencias externas) que muestra las reseñas reales de
https://es.trustpilot.com/review/www.bravocesped.com integradas con la paleta y
tipografía de la web de Bravo Césped.

## Archivos

- `widget-trustpilot.html` → **el widget**. Todo en uno (HTML + CSS + JS). Es el único archivo que se pega en Webflow.
- `demo-trustpilot.html` → página de previsualización local con un selector de variantes (lavanda / verde / crema / oscuro). Solo para desarrollo.

## Datos incluidos

Extraídos el 9 de julio de 2026 del JSON-LD público de la página de Trustpilot:

- Puntuación global: **4,5 / 5**
- Total: **27 opiniones** en Trustpilot (distribución global: 5★ → 24 · 4★ → 1 · 3★ → 1 · 2★ → 0 · 1★ → 1)
- El widget muestra las **18 reseñas positivas más recientes** (4-5★); las negativas se filtran automáticamente en el script (`r >= 4`).

## Cómo ver la demo en local

```bash
cd "/Users/usuario/Desktop/ARCHIVO/PROYECTOS CURSOR/Bravo truspilot"
python3 -m http.server 8765
```

Abre http://localhost:8765/demo-trustpilot.html en el navegador.
Arriba a la izquierda tienes botones para cambiar de variante de color.

## Cómo instalarlo en Webflow

1. Abre `widget-trustpilot.html` y copia **todo el contenido** del archivo.
2. En el Designer de Webflow, arrastra un elemento **Embed Code** (`</>`) donde quieras la sección de reseñas (por ejemplo, reemplazando o junto al bloque `testimonials` actual).
3. Pega el contenido dentro del Embed Code y guarda.
4. Publica.

> El widget está autocontenido: incluye su propio `<style>` (scoping por `#bravo-tp-widget`) y su `<script>`. No depende de ninguna librería externa, así que no rompe los estilos de Webflow.

## Variantes de color

El color de fondo se cambia con el atributo `data-tp-variant` en el div `#bravo-tp-widget`:

```html
<div id="bravo-tp-widget" data-tp-variant="lavanda">
```

Valores disponibles:

| Valor      | Fondo                          | Uso recomendado                              |
|------------|--------------------------------|----------------------------------------------|
| `lavanda`  | `#ddd2fa` (morado claro)       | Por defecto, encaja con la sección actual    |
| `verde`    | `#dce8be` (verde muy suave)    | Sobre fondos clarros / secciones "naturales" |
| `crema`    | `#ffffd8` (amarillo muy suave) | Sobre fondos claros                          |
| `oscuro`   | `#103a2e` (verde oscuro brand) | Secciones con fondo oscuro / alto contraste  |

## Actualizar las reseñas

El JS lleva el array `REVIEWS` con todas las opiniones. Para añadir una nueva:

1. Abre la página de Trustpilot, copia el JSON-LD (o copia a mano los campos).
2. Añade un objeto al array `REVIEWS` con esta forma:

```js
{"r":5,"name":"Nombre Cliente","date":"2026-07-01","title":"Título de la reseña","body":"Texto completo de la reseña."}
```

3. Actualiza el contador en el HTML (texto `Basado en **N** opiniones` y enlace `Ver las N opiniones`).
4. Actualiza la puntuación `.bravo-tp__score` y el SVG de estrellas (offset del gradiente `is-half`: 50% = medio punto, 0%/100% = estrella entera) si cambia la nota media.
5. Vuelve a pegar el Embed Code en Webflow y publica.

## Notas

- Las estrellas usan el mismo SVG de la web (`#C7B5F1` en lavanda).
- La tipografía usa `Cabinetgrotesk` (la que ya carga Webflow) con fallback a system fonts si no estuviera disponible.
- El carrusel es nativo (scroll-snap horizontal). En móvil se ocultan las flechas y se hace scroll con el dedo.
- Accesible: `aria-label` en estrellas, navegación por teclado en botones, respeta `prefers-reduced-motion`.
- Los enlaces apuntan a Trustpilot (opiniones y escribir opinión), abriendo en pestaña nueva.
