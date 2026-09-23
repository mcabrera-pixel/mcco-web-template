# Plantilla de web MCCO (Estándar Web v2)

Crea una web nueva de MCCO Group que nace cumpliendo el estándar: Astro 7 + `@mcco/web-kit` + Cloudflare Pages, con verificación automática (`mcco-check`) en CI y deploy en cada merge a `main`.

## Crear un sitio nuevo (15 minutos)

1. **GitHub → "Use this template"** → nombre `web-<dominio-sin-tld>-cl` (ej. `web-condron-cl`). Visibilidad: pública salvo que el sitio tenga lógica de negocio.
2. Clonar y editar **`site.yaml`** (todo lo marcado `EDITAR:`), `AGENTS.md` §7 y `DESIGN.md` si la división tiene identidad propia. Reemplazar `public/og.webp` y `public/logo.png`.
3. `npm ci && npm run build && npm run check` → debe salir **CUMPLE**. Mientras haya `EDITAR:` en el contenido, `mcco-check` falla a propósito.
4. Cloudflare: `npx wrangler login && npx wrangler pages project create <cloudflare.project> --production-branch main`. En el dashboard: Custom domain (apex + www) y la **checklist de zona** (§9 del estándar: AI Crawl Control = allow, robots gestionado OFF, Crawler Hints ON, Block AI bots OFF).
5. Secrets del repo: `gh secret set CLOUDFLARE_API_TOKEN` y `gh secret set CLOUDFLARE_ACCOUNT_ID` (token con permiso *Cloudflare Pages: Edit*). Variables de las funciones (Web3Forms, Turnstile, Resend) en Cloudflare Pages → Settings.
6. Primer push a `main` → `deploy.yml` construye, verifica, despliega y hace smoke. Listo.
7. Alta en Google Search Console y Bing Webmaster Tools (importar GSC) + enviar `sitemap.xml`. Agregar el sitio a `data/sites.json` del kit (PR) para que aparezca en el bloque Grupo MCCO de las demás webs.

## Estructura

```
site.yaml                 manifiesto (única fuente de verdad)
AGENTS.md · CLAUDE.md     contrato para agentes (Codex / Claude)
DESIGN.md                 identidad visual (tokens, tipografía, anti AI-slop)
astro.config.mjs          mccoConfig() del kit
src/pages/                páginas (.astro con layouts del kit)
src/content/blog|casos|guias/   Markdown con frontmatter validado
src/styles/theme.css      valores de tokens del sitio
public/                   estáticos; _headers/_redirects generados por mcco-headers
functions/api/contact.js  formulario (Pages Function del kit)
.github/workflows/        ci.yml (PR) · deploy.yml (main) → workflows reutilizables del kit
```

## Comandos

| Comando | Qué hace |
|---|---|
| `npm run dev` | servidor local |
| `npm run build` | `mcco-headers` + `astro build` → `dist/` |
| `npm run check` | `mcco-check` (reglas R1-R10 del estándar) sobre `dist/` |
| `npm run parity -- --old https://dominio` | compara producción vs `dist/` ruta por ruta (migraciones) |

## Referencias

- Estándar Web MCCO v2: `mcco-engineering-standards/docs/reglas/sitios-web.md` (regla) y `docs/sitios-web/ESTANDAR-WEB-MCCO-v2.md` (documento completo).
- Kit: https://github.com/mcabrera-pixel/mcco-web-kit
