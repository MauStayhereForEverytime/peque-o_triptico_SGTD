# Sesión 01 · Avances del tríptico SGTD — 2026-09-16

## Objetivo

Recrear como landing page (HTML + CSS, sin JavaScript), lo más fiel posible, el tríptico del
**Sistema de Gestión de Trámite Documentario (SGTD)** de la Municipalidad Provincial de Maynas,
conservando la estructura de tríptico (dos caras de tres paneles).

Referencias usadas (`REFERENCIAS/`):

| Archivo | Contenido |
|---|---|
| `contenido1.png` | Cara exterior: portada, anteportada y hoja doble |
| `contenido2.png` | Cara interior: los tres paneles internos |
| `Lanzamiento_Sistema_Tramite_Documentario_Maynas_2026.pptx` | Se extrajo el escudo de Maynas (PNG con transparencia) |
| `modelo de 3 caras con errores.jpeg` | Solo como contexto (versión anterior descartada) |

## Resultado

```
index.html                  ← landing (todo el contenido del tríptico)
assets/css/triptico.css     ← estilos (retícula escalable, responsive e impresión)
assets/img/escudo-maynas.png← escudo extraído del PPTX (image8.png)
```

Para verlo basta con abrir `index.html` en el navegador. La única dependencia externa es Google Fonts
(Fira Sans Condensed + Caveat); sin conexión se usan las fuentes del sistema como respaldo.

### Mapa de paneles

| Cara | Panel | Contenido |
|---|---|---|
| Exterior | 1 · Portada | Escudo + marca, título "Sistema de Gestión de Trámite Documentario", pastilla SGTD, laptop con la pantalla *Seguimiento de trámite*, sellos (Digital / Seguimiento / Transparencia) y lema manuscrito |
| Exterior | 2 · Anteportada | ¿Cómo consultar tu trámite? (3 pasos), recuadro "Accede al sistema aquí" (URL + QR), "Puedes ingresar desde" (celular, computadora, tótem) y recuadro del video de demostración (QR) |
| Exterior | 3 · Hoja doble | ¿Qué es el SGTD?, "Más información, más confianza", Beneficios para el ciudadano (5) y franja azul con la frase "Un municipio moderno…" |
| Interior | 4 · Recorrido | Recorrido del documento (6 pasos con flechas), aviso "El SGTD no reemplaza a Mesa de Partes" y laptop con el panel de inicio del sistema |
| Interior | 5 · Información | ¿Qué información puedes consultar?, hoja de ruta de ejemplo (tabla real en HTML) y 6 datos consultables |
| Interior | 6 · Seguridad | Escena de seguridad (escudo con candado y nodos), 6 controles y franja de cierre con la marca institucional |

## Decisiones técnicas

- **Retícula escalable sin JS.** Cada panel es un *container query* (`container-type: inline-size`).
  La variable `--u` equivale a 1 px de la referencia (panel de 512 × 1024), así que
  `calc(24 * var(--u))` = 24 px del diseño original. El tríptico escala como una pieza impresa en
  cualquier ancho.
- **Escritorio (≥ 1180 px):** las dos caras se muestran como hojas abiertas (3 columnas, proporción
  1:2 por panel) con una sombra de pliegue entre paneles.
- **Tablet y móvil (< 1180 px):** los paneles se apilan (máx. 560 px). En pantallas ≤ 520 px la
  retícula pasa a 420 unidades para que el texto sea legible, se ocultan los saltos de línea copiados
  de la referencia y se reacomodan la tabla, los datos consultables y la franja de cierre.
- **Impresión (`Ctrl + P`):** A4 horizontal, una cara por hoja (2 páginas) y sin barra ni rótulos.
  Se verificó generando un PDF con Chrome headless.
- **Íconos:** sprite SVG en línea (`<symbol>`), dibujados en estilo lineal para parecerse a la
  referencia. Cero archivos de íconos externos.
- **Ilustraciones en vez de fotos.** Laptops, pantallas del sistema, hoja de ruta, escudo de
  seguridad, fachada de la portada y siluetas de la ciudad están hechos con HTML/CSS/SVG.
- **Accesibilidad:** jerarquía de encabezados (h1 en portada, h2 por panel, h3 por ítem), tabla
  semántica en la hoja de ruta, `aria-label` en las ilustraciones y foco visible en la navegación.
- Se comprobó por script que ningún panel desborda su altura en escritorio y que no hay scroll
  horizontal en 390 px, 820 px, 1366 px ni 1600 px.

### Diferencias intencionales respecto a la referencia

- "tecnologia" → **"tecnología"** (la referencia no llevaba tilde).
- En "¿Qué es el SGTD?" se resalta el nombre completo del sistema (la referencia resaltaba solo
  "Documentario (SGTD)").
- Las fotos (fachada, mano en la laptop, Iquitos en la franja azul, escena de seguridad) se
  reemplazaron por ilustraciones mientras no haya fotos reales.
- Los QR se muestran como marcadores "QR pendiente" (con los tres cuadros guía).

## Pendientes

### 1. QR y enlaces (aún no existen)

Todos los marcadores llevan el atributo `data-pendiente` y un comentario `PENDIENTE` en `index.html`:

| `data-pendiente` | Dónde | Qué hacer |
|---|---|---|
| `enlace-sgtd` | Anteportada, pastilla con la URL | Cambiar el `<span class="url-pill">` por `<a class="url-pill" href="…">` con la URL definitiva (hoy muestra el texto de la referencia `https://www.munimaynas.gob.pe/sgtd`) |
| `qr-sgtd` | Anteportada, "Accede al sistema aquí" | Reemplazar el contenido del `div.qr` por `<img class="qr__img" src="assets/img/qr-sgtd.png" alt="…">` |
| `enlace-video` | Anteportada, recuadro del video | Envolver el bloque en `<a href="…">` |
| `qr-video` | Anteportada, recuadro del video | Igual que `qr-sgtd` (`assets/img/qr-video.png`) |
| `qr-hoja-ruta` | Hoja de ruta de ejemplo | QR ilustrativo (opcional) |

Búsqueda rápida: `grep -n "data-pendiente" index.html`

### 2. Imágenes que conviene pedir

Ya están preparadas las variables CSS en `:root` (`assets/css/triptico.css`). Basta con cambiar
`none` por `url("../img/archivo.jpg")`:

| Variable | Imagen sugerida | Uso |
|---|---|---|
| `--foto-portada` | Fachada de la Municipalidad Provincial de Maynas (horizontal, buena luz) | Fondo de la portada |
| `--foto-ciudad` | Iglesia Matriz / Plaza de Armas de Iquitos o el edificio municipal | Franjas azules (duotono). Agregar la clase `franja--con-foto` para ocultar la silueta |
| `--foto-oficina` | Oficina o escritorio desenfocado | Fondo inferior del panel "Recorrido" |

Del sistema (opcionales, porque hoy están recreadas en HTML):

- Captura real de la pantalla pública **"Seguimiento de trámite"** (portada).
- Captura real del **panel de inicio** del SGTD (panel Recorrido). El PPTX solo trae la pantalla de
  *login* (`image10.png`), por eso no se usó.
- Una **hoja de ruta real** (PDF o imagen) para validar el formato del ejemplo.

Para usar una captura, reemplazar el `div.ui` o el `div.panelui` dentro de `.laptop__pantalla` por
`<img src="…" alt="…">`.

### 3. Validar contenido

- Los datos de la hoja de ruta (EXP N° 22149-2026-UADAG-SG-MPM, oficinas, fechas) y las cifras del
  panel de inicio (1866 pendientes, etc.) son **de ejemplo**, copiados de la referencia.
- Confirmar dirección y RUC que aparecen en la hoja de ruta (Calle R. Echenique Nro. 350 · RUC 20103846590).
- Confirmar el orden de plegado para imprenta. Cara exterior: portada | anteportada | hoja doble.
  Al imprimir a doble cara, voltear por el borde corto.

## Próximos pasos sugeridos

1. Recibir las URL y los QR definitivos y reemplazar los marcadores.
2. Incorporar las fotos reales y hacer un ajuste fino de contraste en la portada.
3. Revisión del contenido por el área responsable y prueba de impresión física en A4.
