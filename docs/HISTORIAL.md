# Historial del rediseño

Contexto de cómo se llegó a la web actual, para quien la retome más adelante.

## Punto de partida (22 de septiembre de 2026)

La web original de agosan.net estaba hecha con Trickle: React y Babel compilando en el navegador, más Tailwind desde un CDN. Consecuencias:

- Carga lenta y dependiente de servidores de terceros (`app.trickle.so`, `resource.trickle.so`, `cdn.tailwindcss.com`).
- SEO mínimo: sin datos estructurados, sin canonical, sin favicon, sin sitemap.
- El contenido se pintaba con JavaScript, así que los buscadores lo veían tarde o no lo veían.
- Diseño de plantilla: degradado rojo y tarjetas blancas.

Se conservaron **el contenido y las cuatro imágenes** (descargadas de Trickle y optimizadas a WebP en `assets/img`, con los originales en `assets/src`).

## Decisiones de diseño

- **Concepto:** taller técnico y etiqueta de inspección. Fondo carbón, rojo extintor, amarillo de chaleco reflectante y esquinas rectas. Tipografías Big Shoulders (títulos), Archivo (texto) e IBM Plex Mono (datos técnicos), todas alojadas en el sitio.
- **Un solo tema:** oscuro. No hay modo claro; el fuego y las brasas necesitan fondo oscuro.
- **Elementos propios del sector:** brasas que se apagan con polvo químico, servicios como etiquetas colgadas con una llama que se apaga y muestra "OK", cifras como manómetros, intervalos de prueba hidrostática como una regla de años y la recarga como una orden de trabajo que se va marcando.
- **Se descartó a propósito:** GSAP y cualquier framework. Con CSS y JavaScript normal alcanzaba, y así el mantenimiento es más simple.

## Auditorías realizadas y fallos corregidos

**Visual (375, 768, 920 y 1440 px):**
- El acento de "¿POR QUÉ ELEGIRNOS?" pisaba la línea superior: se subió el interlineado de los títulos de sección a 1.
- El texto legal del pie heredaba un ancho máximo de 44 caracteres y quedaba estrecho.
- Los pasos de recarga habían perdido su número: ahora la casilla muestra el número hasta que se marca.

**Lógica y teclado:**
- La sombra de la cabecera quedaba siempre activa en móvil porque el hero es más alto que la pantalla. Se cambió por un marcador invisible al inicio del hero.
- En pantallas bajas el fuego se apagaba nada más cargar. Ahora solo se apaga si el usuario baja de verdad.

**Revisión de código (`/code-review high`) y seguridad:**
- `js/fire.js`: la reignición gradual del fuego no se cancelaba al volver a apagarlo, así que el fuego reaparecía antes de tiempo. Se guarda y se cancela el temporizador.
- `404.html`: con rutas relativas, la página de error salía sin estilos en URLs con subcarpetas. Ahora usa rutas absolutas.
- Escaneo sin hallazgos: no hay `innerHTML`, `eval`, enlaces inseguros, recursos de terceros ni secretos.

## Resultados

PageSpeed Insights sobre https://mxp09.github.io/agosan.net/ (22 de septiembre de 2026):

| | Rendimiento | Accesibilidad | Prácticas | SEO |
|---|---|---|---|---|
| Móvil | 99 | 100 | 100 | 100 |
| Escritorio | 100 | 100 | 100 | 100 |

HTML validado con `html-validate` sin errores.

## Pendiente

1. **Subir a Cloudflare Pages.** Resuelve la caché de archivos (GitHub Pages no permite configurarla y PageSpeed lo marca), porque Cloudflare sí lee `_headers`.
2. **Dominio (último paso, después de Cloudflare):** agosan.net sigue apuntando a la web antigua. Cuando el sitio esté en Cloudflare Pages, se conecta el dominio desde su panel. No hace falta archivo `CNAME` (eso era solo para GitHub Pages).
3. **Activar la analítica:** la CSP ya permite Cloudflare Web Analytics (`static.cloudflareinsights.com` para el script y `cloudflareinsights.com` para los datos), en el `<meta>` de `index.html` y `404.html` y en `_headers`. Falta activarla en el panel de Cloudflare Pages (Metrics > Web Analytics). No usa cookies, así que no necesita aviso de consentimiento.
4. **CSS que bloquea el renderizado:** se podría incrustar el CSS crítico en el HTML, pero habría que relajar la CSP o mantener un hash que cambia con cada edición del CSS. Con 99 en móvil la mejora no compensa. Se decidió no hacerlo.

Resuelto: el 6674-8792 sí tiene WhatsApp (confirmado por el cliente el 23 de septiembre de 2026).

## Herramientas usadas

Skills y plugins de Claude Code: `ui-ux-pro-max` (dirección de diseño), `animate` (criterios de animación), `design-taste-frontend` y `no-ai-slop` (evitar diseño y textos genéricos), `playwright` (capturas y pruebas), más `/code-review` y `/security-review`.

Herramientas de línea de comandos: `npx serve`, `npx html-validate`, `npx lighthouse` y Pillow (Python) para optimizar las imágenes y generar la imagen de redes sociales.
