---
name: editor-landing-esen
description: >-
  Úsala cuando alguien pida cambiar el contenido de la landing del Programa de
  Asistencia Financiera de la ESEN: textos, montos, becas, datos de contacto,
  imágenes o colores de marca. Frases como "cambiá el título del hero", "actualizá
  el costo de la matrícula", "corregí el correo de asistencia financiera",
  "subí esta foto al hero" o "dejá listo el borrador para revisión". Edita SOLO el
  archivo content.json del repositorio; nunca toca index.html ni el layout.
---

# Editor de la landing — ESEN (Programa de Asistencia Financiera)

Esta skill permite editar el contenido de la landing **conversando**, sin tocar
código ni romper el diseño. La landing se arma sola a partir de un único archivo,
`content.json`, que es la **fuente de verdad**. La plantilla (`index.html`) lee ese
archivo y lo pinta. Por eso editar contenido nunca puede dañar el layout.

## Regla de oro

- **Solo se edita `content.json`.** Nunca `index.html`, nunca CSS, nunca estructura.
- **Solo se editan slots que ya existen.** No se inventan campos ni secciones nuevas.
- **Se respeta el tipo de cada slot.** Un texto sigue siendo texto, una lista sigue
  siendo lista, un color sigue siendo un hex válido (`#RRGGBB`).
- Si un pedido implicara cambiar el diseño, agregar una sección, o tocar el HTML:
  **no se hace.** Se explica que esta skill solo cambia contenido y se sugiere
  escalar ese pedido al equipo de TechyWe.

Si al leer un pedido te encontrás "reinterpretándolo" para que entre en una de
estas reglas, esa es la señal de detenerte y pedir aclaración, no de continuar.

## Roles y flujo (borrador → revisión → publicado)

Hay dos roles. **El sistema no los obliga**: se sostienen como acuerdo de trabajo.

1. **Editor** — pide los cambios. Sus cambios se guardan como **borrador**
   (rama `draft` del repo, o archivo `content.draft.json`). El Editor **no publica**.
2. **Aprobador** — revisa el borrador contra lo publicado y decide. Solo el
   Aprobador autoriza publicar.

**Publicar** = escribir el `content.json` aprobado a la rama `main` del repo. El
commit dispara el despliegue automático en Vercel/Netlify y el sitio se actualiza.

Nunca publiques por iniciativa propia. Publicar requiere una confirmación
**explícita** del Aprobador ("publicá", "aprobado, subilo"). Ante la duda, dejá el
cambio en borrador y avisá que queda pendiente de aprobación.

## Cómo procesar un pedido del Editor

1. **Leé el `content.json` actual** del repositorio (vía el conector de GitHub).
2. **Identificá el slot** al que se refiere el pedido usando el mapa de abajo.
   Si es ambiguo entre dos slots, preguntá cuál antes de tocar nada.
3. **Aplicá el cambio** solo sobre ese slot, conservando el resto idéntico.
4. **Mostrá qué cambió**: el valor anterior y el nuevo, en lenguaje claro.
   No muestres el JSON completo salvo que lo pidan.
5. **Guardá el borrador** (rama `draft` / `content.draft.json`). Confirmá que
   queda listo para que el Aprobador lo revise.

## Mapa de slots editables (content.json)

- `meta.title`, `meta.description` — título y descripción de la página (SEO).
- `brand.*` — colores de marca en hex: `navy`, `navyDeep`, `lightBlue`,
  `paleBlue`, `orange`, `ink`, `cloud`. Validá que sea `#` + 6 hex.
- `header.wordmark`, `header.ctaLabel`, `header.ctaHref`.
- `hero.eyebrow`, `hero.titlePrefix`, `hero.titleEmphasis`, `hero.intro`,
  `hero.image`, `hero.imageAlt`.
- `pago.heading`; `pago.opciones[]` (cada una `titulo`, `detalle`);
  `pago.bancosHeading`; `pago.bancos[]` (cada uno `nombre`, `puntos[]`);
  `pago.notas[]`.
- `becas.heading`; `becas.items[]` (cada uno `titulo`, `detalle`).
- `planes.heading`, `planes.texto`.
- `costos.heading`; `costos.items[]` (cada uno `monto`, `concepto`); `costos.nota`.
- `generalidades.heading`; `generalidades.items[]` (textos).
- `contacto.heading`; `contacto.items[]` (`etiqueta`, `email`);
  `contacto.telefonos[]`; `contacto.sitio`, `contacto.sitioHref`;
  `contacto.direccion`; `contacto.redes[]` (`red`, `url`).

Podés **editar, reordenar, agregar o quitar elementos dentro de las listas** que
ya existen (por ejemplo, una beca más en `becas.items` o un teléfono en
`contacto.telefonos`). Lo que no se hace es crear secciones nuevas de nivel
superior ni campos que la plantilla no sabe pintar.

## Imágenes

La skill **no genera ni sube imágenes**. El Editor sube el archivo al repo (carpeta
`assets/`) por su cuenta. La skill solo actualiza la **referencia**: por ejemplo,
poner `hero.image` en `"assets/hero-2025.jpg"` y ajustar `hero.imageAlt` con una
descripción del nuevo archivo. Recordale al Editor que el archivo debe existir en
`assets/` para que se vea.

## Validaciones antes de guardar

- Montos en `costos`: mantené el formato `"$ 1,234.00"` (símbolo, miles con coma,
  dos decimales).
- Correos: formato válido `algo@dominio`. Enlaces: `https://…`.
- Colores: `#` + 6 dígitos hex. Si el valor no valida, no guardés y pedí corrección.
- Nunca dejes un slot de texto vacío si antes tenía contenido, salvo que lo pidan
  explícitamente.

## Qué responder cuando NO se puede

Con tono amable y breve: "Eso cambiaría el diseño / la estructura, y esta skill
solo edita el contenido de la página. Te ayudo con cualquier texto, monto, beca,
contacto o imagen; para cambios de diseño conviene escalarlo a TechyWe."
