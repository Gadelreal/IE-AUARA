# AUARA · Case (intro + caso)

Dos páginas HTML autónomas (sin dependencias ni servidor) para el módulo del caso AUARA de IE University:
la apertura (**intro**) y la página del **caso** con el vídeo de introducción, la línea de tiempo y el ejercicio
de reflexión. Versión del 13/10/2026.

## Qué hay en el repositorio

| Archivo | Para qué sirve |
|---|---|
| `index.html` | Redirige al intro. Es la dirección que se comparte. |
| `auara-case-intro.html` | Pantalla de apertura (botón **Start**). |
| `auara-case-study.html` | Página del caso. |
| `preview.html` | Revisión en **teléfono, tablet y escritorio** con las alturas reales de cada dispositivo. |
| `assets/` | PNG de respaldo de la foto (solo para navegadores sin WebP). |
| `.nojekyll` | Evita que GitHub procese los archivos. |

Los dos HTML del caso deben estar **en la misma carpeta** y con estos nombres: el intro lleva al caso y el caso
vuelve al intro con enlaces relativos.

## Verlo publicado (GitHub Pages)

1. Sube **el contenido** de esta carpeta a la raíz del repositorio (no la carpeta entera).
2. En el repositorio: **Settings → Pages → Build and deployment**: *Deploy from a branch*, rama `main`,
   carpeta `/ (root)`. Guarda.
3. En un par de minutos estará en `https://<usuario>.github.io/<repositorio>/`:
   - caso: `…/` (abre el intro)
   - revisión en dispositivos: `…/preview.html`

> Las páginas **no se pueden abrir con doble clic** desde el disco (`file://`) sin perder el relevo de las
> olas entre el intro y el caso. Publicadas, o con un servidor local (`python3 -m http.server`), funcionan completas.

## Revisión en dispositivos (`preview.html`)

- Elige página (**Intro** / **Case**) y dispositivo (**Phone 390**, **Tablet 768**, **Desktop 1280**, **Fit**).
- **Real height** muestra cada dispositivo con su altura real (844, 1024 y 720 px), reducido para que quepa. La
  página ve el tamaño real: los puntos de corte y la posición del agua son los verdaderos.
- Es una **copia congelada** de las dos páginas en la fecha indicada arriba a la derecha. Si cambian los archivos,
  hay que regenerarla.
- Las animaciones se muestran siempre, también en las páginas reales, aunque el sistema pida *reducir movimiento*
  (ver «Reducir movimiento» más abajo).

## Integración

La página del caso emite eventos en `document` (`CustomEvent`) para conectarla con la plataforma:

| Evento | Cuándo | `detail` |
|---|---|---|
| `case:ready` | Primer fotograma dibujado | — |
| `case:arrived` | Termina la entrada del agua al llegar desde el intro | — |
| `case:navigate` | Flechas de navegación. **Cancelable**: `preventDefault()` para gestionar tú la navegación | `{ href, handoff }` |
| `case:next` | Flecha derecha sin página destino | — |
| `case:watch` | Botón *Watch now* | — |
| `case:menu` | Botón de menú | — |
| `case:reflection-submit` | Envío del ejercicio | `{ value }` (texto, ≤ 250 caracteres) |
| `case:reflection-reset` | *Change the answer* | — |

Configuración, en los propios archivos:

- **Página siguiente:** atributo `data-href=""` del botón `#btn-next` en `auara-case-study.html`.
  Vacío, la flecha reproduce la transición completa y emite `case:next`.
- **Hitos de la línea de tiempo:** elementos `<li class="tl__event">`; alternan solos de lado y el eje se ajusta.
- **Constantes del script del caso:** `WATER_COVER` y `MEAN_RISE_PX` (nivel del agua), `SENT_MSG_MS` (duración del
  mensaje «Your response has been recorded!»).

## Reducir movimiento (accesibilidad)

Por defecto **las animaciones funcionan siempre al cargar**, aunque el sistema operativo del visitante pida reducir
movimiento. El botón de pausa del caso sigue disponible para quien lo necesite.

Para **respetar el ajuste del sistema** (la página arranca en pausa, sin entrada en cascada, sin animar el timeline ni
las ondas del ejercicio, y con transiciones instantáneas) basta con cambiar un valor:

- en `auara-case-study.html`, en el script de la cabecera: `window.AUARA_HONOR_REDUCED_MOTION = false` → `true`
- en `auara-case-intro.html`, al principio de su script: la misma línea.

Es una decisión de accesibilidad que conviene confirmar con el cliente antes de publicar de verdad.

## Qué cambia respecto a los archivos entregados

Solo en este paquete (los originales no se tocan):

1. **El logo de IE enlaza a `https://www.ie.edu/`** y abre en pestaña nueva. En los originales apunta a `/`, que
   en GitHub Pages saldría del repositorio. En la plataforma real debería llevar a la página de inicio del curso.
2. **`<meta name="robots" content="noindex, nofollow">`** en todas las páginas, para que una revisión de cliente no
   se indexe en buscadores. Quítalo cuando se publique de verdad.

## Contenido de terceros

- **Logo de IE University y foto de la profesora:** confirma que se pueden publicar antes de hacer el repositorio
  público. Si no, mantenlo **privado** y comparte el acceso con el cliente como colaborador.
- **Tipografías** (Bricolage Grotesque e IBM Plex Mono): licencia SIL Open Font License, incluidas en los HTML.
