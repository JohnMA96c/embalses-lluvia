# Dashboard — mapa público

Sitio estático (HTML + Leaflet + Chart.js) que lee `data/snapshot.json`. No tiene backend ni base de datos propia: el pipeline de Databricks regenera el JSON periódicamente y aquí solo se visualiza.

## Ver en local

```bash
cd dashboard
python3 -m http.server 8000
# abrir http://localhost:8000
```

(Tiene que servirse por HTTP, no abrirse como archivo `file://`, porque el navegador bloquea el `fetch` de `data/snapshot.json` en local.)

## Desplegar gratis en GitHub Pages

1. Subir esta carpeta `dashboard/` (o todo el repo) a un repositorio de GitHub.
2. Settings → Pages → Source: rama `main`, carpeta `/dashboard` (o `/` si subes solo el contenido de esta carpeta).
3. GitHub publica la URL en 1-2 minutos: `https://<usuario>.github.io/<repo>/`.

## Desplegar gratis en Cloudflare Pages

1. Conectar el repositorio de GitHub en Cloudflare Pages.
2. Build command: (ninguno, es estático) — Output directory: `dashboard`.
3. Deploy. Cloudflare da HTTPS y CDN global gratis en el plan free.

## Conectar el snapshot real

`skill/scripts/publish_snapshot.py` genera `data/snapshot.json` a partir de las tablas Gold de Databricks. La última tarea del Workflow (`publish_snapshot`, ver `pipeline/workflow_definition.json`) hace commit del nuevo JSON al repositorio automáticamente vía la API de GitHub — así el dashboard se actualiza solo, sin servidor que mantener.
