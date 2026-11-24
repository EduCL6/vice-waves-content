# vice-waves-content

Canal de contenido remoto de **Vice Waves** (app no oficial de las radios de
GTA VI). La app descarga `manifest.json` al abrir; si la `version` es mayor que
la que tiene instalada, descarga el catálogo apuntado por `url`, lo valida y lo
usa desde entonces. Así las correcciones de tracklists (canciones que faltan,
links de Spotify/Apple Music) llegan a todos los usuarios **sin pasar por
revisión de tienda**.

## Estructura

- `manifest.json` — `{ "version": N, "url": "<URL absoluta de vN.json>" }`.
  La app solo cambia de catálogo si N es mayor que su versión local.
- `v2.json`, `v3.json`, … — catálogos completos (emisoras + canciones + links
  exactos de Spotify y Apple Music). Inmutables: nunca se edita un vN ya
  publicado, siempre se publica el siguiente.

## Flujo de actualización

1. Regenerar el catálogo local: `python3 tools/extract_fandom.py`
   (tracklists desde GTA Wiki) y `python3 tools/enrich_apple.py` (links
   exactos de Apple Music vía iTunes Search API).
2. Copiar el resultado aquí como `v{N+1}.json` subiendo su campo `version`.
3. Actualizar `manifest.json` con la nueva versión y URL.
4. Commit + push. Los usuarios lo reciben al abrir la app.

Regla de oro: este canal solo lleva **datos del catálogo** (canciones,
emisoras, links). Features, anuncios o cambios de comportamiento van en el
binario revisado de la tienda.

El catálogo se compila a partir de la documentación comunitaria de GTA Wiki;
la app muestra la atribución en Ajustes → Fuentes. Contenido de fans, no
afiliado a Rockstar Games ni Take-Two Interactive.
