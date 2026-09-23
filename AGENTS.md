# AGENTS.md — <nombre del sitio> (EDITAR: copiar de site.yaml → name)

Web de marketing de una división de **MCCO Group SpA** (Chile). Sitio estático generado con **Astro 7** sobre el kit compartido **`@mcco/web-kit`**, desplegado en **Cloudflare Pages**. Cumple el **Estándar Web MCCO v2** (repo `mcco-engineering-standards`, `docs/reglas/sitios-web.md`). Responder y escribir commits en español.

## 1. Contrato del repositorio (idéntico en todas las webs MCCO)

| Ruta | Qué es | Regla |
|---|---|---|
| `site.yaml` | Manifiesto del sitio: dominio, proyecto Cloudflare, contacto, analítica, CSP, prohibidos, servicios/FAQ de la home | Única fuente de verdad. Nada de esto se hardcodea en páginas. |
| `src/pages/` | Páginas. `.astro` usan los layouts del kit. `.html` legado sale byte-idéntico (permitido durante migración, con canonical manual) | Ruta = URL. No renombrar rutas sin 301 en `_redirects`. |
| `src/content/blog|casos|guias/*.md` | Contenido con frontmatter validado por Zod (`@mcco/web-kit/schemas`) | El motor de contenido y los agentes escriben **solo aquí**. Índice, sitemap y llms.txt se regeneran solos. |
| `src/styles/theme.css` + `DESIGN.md` | Identidad visual del sitio (valores de tokens) | El kit fija nombres de tokens, nunca la estética. |
| `public/` | Estáticos tal cual (favicon, og.webp, logo, clave IndexNow) | `_headers` y `_redirects` los genera `mcco-headers` desde `site.yaml`: **no editar a mano** (reglas propias bajo el marcador `# --- reglas del sitio ---`). |
| `functions/api/*.js` | Pages Functions (contacto, cotizador) | Secretos solo en Cloudflare (Settings → Environment variables). Nunca en el repo, nunca en `.dev.vars` commiteado. |
| `.github/workflows/` | `ci.yml` (PR) y `deploy.yml` (main) llaman a los workflows reutilizables del kit | No duplicar lógica de CI aquí. |
| `AGENTS.md`, `CLAUDE.md`, `README.md` | Este contrato, puntero para Claude, guía humana | Mantener al día cuando cambie el sitio. |

El kit se fija por tag en `package.json` (`github:mcabrera-pixel/mcco-web-kit#vX.Y.Z`). Un cambio de layout, cabecera, JSON-LD o regla de verificación se hace **en el kit** y se sube el tag; nunca copiando código del kit al sitio.

## 2. Comandos

```bash
npm ci                 # instalar (Node ≥ 22; .node-version = 24)
npm run dev            # http://localhost:4321
npm run build          # prebuild: mcco-headers → astro build → dist/
npm run check          # mcco-check sobre dist/: OBLIGATORIO verde antes de abrir PR
npm run parity -- --old https://<dominio-en-producción>   # gate de migración (compara prod vs dist)
```

## 3. Flujo de trabajo

1. Rama `feat/…`, `fix/…` o `content/…` desde `main`.
2. Cambios + `npm run build && npm run check` en local.
3. PR → CI ejecuta build + `mcco-check` + preview en Cloudflare (si el repo tiene credenciales). Adjuntar la URL de preview en el PR.
4. Merge a `main` = **producción automática** (`deploy.yml`: build + check + deploy + smoke). Nunca push directo a `main`, nunca `--force`.
5. Si el deploy rompe algo: Cloudflare Pages → Deployments → *Rollback* al anterior; luego arreglar en rama.

## 4. URLs y SEO/GEO

- URL canónica = la que sirve Cloudflare Pages: **sin `.html`**, carpetas **con `/`**. El kit la calcula; en páginas `.html` legado se escribe a mano igual.
- Cada página: 1 `<title>` (≤ 60), 1 meta description (70-160), 1 canonical, 1 H1, Open Graph. Lo verifica `mcco-check` (R2).
- Artículo nuevo: `src/content/blog/<slug>.md` con `title`, `description`, `date`, `capsule` (3-4 frases autocontenidas), `faq` (3-8), `related` (páginas de servicio propias), `sister` (1 web hermana), `sources`. Sin eso no compila.
- Página nueva: `src/pages/<ruta>.astro` con `<Base title description>` (+ `organization={true}` solo en home/institucionales). Enlazarla desde la navegación o la home; el sitemap la incluye solo.
- No invertir en `llms.txt`/WebMCP: se generan solos y no son palanca comprobada. Palancas reales: crawlers desbloqueados en la zona Cloudflare, formato extraíble (cápsula + tabla + FAQ + fuentes), casos reales, entidad consistente.

## 5. Verdad y contenido

- **Cero cifras, clientes, certificaciones o testimonios inventados.** Toda métrica publicada lleva fuente (`metrics[].source` en casos). Sin fuente, no se publica.
- Entidad única desde el kit (`data/entity.json`): MCCO Group SpA · RUT 77.715.147-9 · constituida en 2022 · Suecia 283, of. 402, Providencia, Santiago · un solo cargo de Mario Cabrera: "Gerente General, MCCO Group SpA". "20+ años" solo como experiencia del equipo, nunca de la empresa.
- Clientes nombrables: los de `site.yaml → clients_allowed`. Proyectos bajo NDA: la blacklist del kit hace fallar el build si aparecen.
- `site.yaml → forbidden`: strings que contradicen la verdad del sitio (specs de equipos que no se tienen, marcas ajenas). Agregar ahí cada corrección de verdad para que no vuelva.
- Imágenes: material real > IA. IA solo para atmósfera; nunca genera texto, datos, logos ni personas presentadas como testimonios. Sin avatares IA. Heroes WebP ≤ 150 KB; og.webp 1200×630 ≤ 600 KB.
- JSON-LD `Review`/`AggregateRating`: prohibidos salvo reseñas reales y verificables.

## 6. Qué no hacer

- No editar `dist/`, `public/_headers` ni `public/_redirects` a mano.
- No subir videos al repo: van a Cloudflare R2 y se referencian por URL (`site.yaml → csp.media_src`).
- No agregar un `<script src>` externo sin declarar su origen en `site.yaml → csp.script_src` (la CSP lo bloquea y `mcco-check` falla).
- No copiar componentes del kit al sitio para "ajustarlos": abrir PR en el kit.
- No reformatear archivos completos ni "mejorar" lo adyacente al cambio pedido.

## 7. Datos de este sitio (EDITAR)

- Dominio: (site.yaml → domain) · Proyecto Cloudflare: (cloudflare.project) · Zona Cloudflare: checklist §9 del estándar hecha: sí/no.
- Servicios y páginas de servicio: …
- Casos publicables y clientes nombrables: …
- Particularidades (funciones, cotizador, idiomas, integraciones): …

## 8. Commits

Conventional Commits en español: `feat(blog): …`, `fix(seo): …`, `content(casos): …`, `chore(kit): sube @mcco/web-kit a vX`. Un cambio lógico por commit.
