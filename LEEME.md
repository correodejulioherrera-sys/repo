# Los signos de la madera · appweb de realidad aumentada

Apuntas con el móvil a un signo pintado del tronco y ese signo «despierta»: aparece su escena en 3D encima de la madera y una voz cuenta su parte de la historia. Son cinco signos y cinco capítulos; al despertar los cinco sale el cierre.

Hecha con **MindAR 1.2.5** (seguimiento de imagen) y **A-Frame 1.5** (3D), sin instalar nada: todo es HTML y JavaScript.

## Qué hay en la carpeta

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La experiencia AR (cámara). La historia se edita al principio del archivo. |
| `targets.mind` | Los cinco marcadores compilados (uno por zona de signos). |
| `simulador.html` | Vista previa sin cámara, sobre la foto del tronco. Útil para revisar textos y escenas. |
| `marcadores/` | Las imágenes con las que se compiló `targets.mind`, por si quieres rehacerlas. |

## Ponerla en marcha

La cámara del móvil solo funciona en páginas con **https**. Sube la carpeta completa a cualquier hosting estático con https (GitHub Pages, Netlify, Vercel o tu propio servidor) y abre `index.html` en el móvil.

**Probarla sin ir al tronco:** abre la foto original del tronco a pantalla completa en el ordenador y apunta el móvil a cada signo. Los marcadores salen de esa misma foto, así que se reconocen bien.

Añade `?depurar` a la dirección (`index.html?depurar`) para ver en pantalla qué signos está detectando.

## Cómo se usa

- **De cerca** (el signo ocupa buena parte de la pantalla): despierta al momento.
- **Desde más lejos o con varios signos a la vista**: aparece un círculo en el centro; el signo que se mantenga dentro del círculo un segundo es el que despierta.
- Cada signo se reconoce hasta **1,5–2 m** aproximadamente. Si el público va a estar más lejos, hay que hacer marcadores con fotos tomadas desde esa distancia.

## Cambiar la historia

Todo está en el bloque `window.HISTORIA` al principio de `index.html` (y el mismo bloque en `simulador.html`, si quieres que la vista previa coincida):

- `titulo`, `intro`, `final`: textos de portada y cierre.
- `capitulos[].titulo` y `capitulos[].texto`: lo que se lee y se narra en cada signo. **Los textos actuales son de ejemplo.**
- `audio: 'audio/lluvia.mp3'`: usa una locución grabada en vez de la voz del móvil.
- `modelo: 'modelos/volcan.glb'`: sustituye la escena incluida por un modelo propio (exportado desde Maya, Cinema 4D o Blender). Si el .glb trae animaciones, se reproducen solas. `modeloEscala` y `modeloPosicion` lo colocan: 1 unidad ≈ 60 cm sobre el tronco, el origen es el signo, `y` hacia arriba y `z` hacia quien mira.

## Mejorar el reconocimiento en el sitio

Los marcadores actuales salen de una sola foto. Para que funcione con cualquier luz del día:

1. Haz una foto de frente a cada zona, con luz uniforme (sin manchas de sol duras), parecida al encuadre de las imágenes de `marcadores/` (el tronco en horizontal, los signos y algo de madera alrededor).
2. Compílalas en el compilador oficial de MindAR (https://hiukim.github.io/mind-ar-js-doc/tools/compile) **en el mismo orden**: lluvia, espiral, montaña, vasija, mar.
3. Sustituye `targets.mind`.

Si cambias mucho el encuadre, ajusta en `marcadores.zonas` el `ancla` (dónde nace el 3D dentro de la imagen, con el ancho de la imagen = 1 y el centro en 0,0) y el `giro`.

## Ajustes

En `ajustesAR`:

- `seguimientoSimultaneo`: signos que se siguen a la vez (3 por defecto). Pon 1 si un móvil modesto va lento; entonces no hay círculo de selección.
- `filtroBeta`: si el 3D tiembla, bájalo; si va con retraso al mover el móvil, súbelo.
- `permanencia`: segundos que hay que mantener un signo en el círculo.

## Prueba realizada

Con una cámara simulada a partir de la foto (navegador Chromium), se detectó el signo correcto en las 8 vistas cercanas probadas, incluidas una de lado y oscura, otra girada y desenfocada y otra a más distancia, en unos 2-3 s. Con varios signos en pantalla, el círculo eligió el signo centrado. Con el tronco entero en el encuadre (muy lejos) solo reconoció un signo suelto: ahí hace falta acercarse.
