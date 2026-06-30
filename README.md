# Landing — Programa de Asistencia Financiera (ESEN)

Landing estática editable de forma conversacional con Claude. El contenido vive
separado del diseño, así que editarlo nunca rompe el layout.

## Estructura

| Archivo | Qué es |
|---|---|
| `content.json` | **Fuente de verdad.** Todo el texto, montos, becas, contactos, imágenes y colores. Lo único que edita la skill. |
| `index.html` | Plantilla. Lee `content.json` y pinta la página. No se edita para cambiar contenido. |
| `assets/` | Imágenes (hero, etc.). Se suben aquí manualmente. |
| `SKILL.md` | Protocolo de la skill de edición (reglas, flujo borrador→publicar, mapa de slots). |
| `preview.html` | Copia autocontenida solo para previsualizar sin servidor. Se regenera; no es fuente de verdad. |

## Publicar

1. Subí el repo a GitHub.
2. Conectalo a Vercel o Netlify (sitio estático, sin build). Cada commit a `main`
   despliega automáticamente.
3. Dejá caer la foto del hero en `assets/` con el nombre que indica `hero.image`.

## Editar con la skill (dentro de Claude.ai)

1. Cargá `SKILL.md` como skill en un Proyecto de Claude.
2. Conectá el repo (conector de GitHub) para que la skill pueda leer y escribir
   `content.json`.
3. El **Editor** pide cambios en lenguaje natural → quedan como borrador.
   El **Aprobador** revisa y autoriza publicar → la skill hace el commit a `main`.

Para forzar la aprobación de verdad (no solo por acuerdo), protegé la rama `main`
en GitHub y publicá vía merge de la rama `draft` con revisión obligatoria.
