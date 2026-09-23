# DESIGN.md — MCCO Copper

> Sistema de diseño para sitios web de marca MCCO Copper SpA. Industrial editorial. Inspirado en Tesla minimal + WIRED magazine + Stripe trust signals + Linear restraint. Adaptado a contexto minero chileno.
>
> Última actualización: 2026-05-06
> Versión: 1.0
> Para usar en: mccocopper.cl, propuestas comerciales, deliverables a clientes mineros

---

## 1. Visual Theme

**Industrial editorial** — la solidez técnica de la minería chilena combinada con la claridad estructural de revistas técnicas.

**Mood:**
- Solid (no flotante, no glass morphism)
- Earned (weathered surfaces, scratch marks, no perfection)
- Direct (no fluff, no hype, datos concretos)
- Chilean (warm tones, andean references, español sin anglicismos)
- Confident (sin gritar, sin pedir disculpa)

**Inspiraciones cruzadas:**
- **Tesla** — minimal industrial premium pero chileno (no californiano)
- **WIRED magazine** — densidad editorial cuando la página tiene mucho contenido técnico
- **Stripe** — trust signals (proof, casos reales, datos auditables)
- **Linear** — restraint en interactions (no over-animated)
- **The Atlantic / Bloomberg** — tipografía editorial premium

**Anti-inspiraciones:**
- Sitios "AI-powered" cualquier (purple gradients, "transform your business")
- SaaS tipo Notion (demasiado friendly, demasiado playful)
- Sitios mineros chilenos legacy (1990s-2010s aesthetics, comic sans, gif)

---

## 2. Color Palette

Paleta de **3 dominantes + 1 accent + functional tokens.**

### Brand colors

```css
:root {
  /* Dominants — usar para 90% del sitio */
  --brand-charcoal:  #1A1A1A;  /* primary surface dark, text on light */
  --brand-cream:     #F4EFE6;  /* primary surface light, text on dark */
  --brand-stone:     #6B6660;  /* secondary text, borders, muted */

  /* Accent — usar < 10% para CTAs, highlights, links */
  --brand-copper:    #B8542A;  /* terracotta industrial — accent agresivo */

  /* Optional metallic — solo para deliverables premium clientes mayores */
  --brand-bronze:    #8B6F47;  /* sub-accent, evitar abusar */

  /* Functional tokens */
  --color-success:   #4A6741;  /* verde minero conservador, no neón */
  --color-warning:   #C8843E;  /* ámbar industrial */
  --color-error:     #8B2828;  /* rojo conservador, no fluorescente */

  /* Surfaces */
  --surface-page:    var(--brand-cream);
  --surface-card:    #FFFFFF;
  --surface-overlay: rgba(26, 26, 26, 0.85);
  --surface-dark:    var(--brand-charcoal);

  /* Text */
  --text-primary:    var(--brand-charcoal);
  --text-secondary:  var(--brand-stone);
  --text-on-dark:    var(--brand-cream);
  --text-link:       var(--brand-copper);
}
```

### Reglas de aplicación

- **Hero:** charcoal sobre cream OR cream sobre charcoal (depende caso). NUNCA ambos juntos en mismo viewport sin razón.
- **CTAs primary:** copper sobre cream (alto contraste).
- **CTAs secondary:** charcoal outline sobre cream, sin fill.
- **Borders:** stone con opacity 30-50%, NUNCA copper como border (chillón).
- **Backgrounds:** cream default, charcoal para sections de "depth" (testimonials, footer).
- **Gradients:** PROHIBIDOS excepto overlay sutil charcoal-to-transparent en imágenes (para legibilidad de overlay text).

### Anti-patterns específicos MCCO

- ❌ Purple, lavender, indigo, violet — slop trigger
- ❌ Glass morphism / frosted backdrop-filter
- ❌ Neon green / electric blue — fuera de contexto industrial chileno
- ❌ Más de 4 colores en la misma section
- ❌ Copper como dominante (es accent, < 10%)

---

## 3. Typography

### Familias

```css
:root {
  /* Display — solo H1, H2 critical, hero */
  --font-display: "Recoleta", "Tiempos Headline", Georgia, serif;

  /* Body — texto general */
  --font-body: "IBM Plex Sans", "Inter Tight", system-ui, sans-serif;

  /* Mono — code, IDs, números técnicos, datos */
  --font-mono: "JetBrains Mono", "IBM Plex Mono", "Berkeley Mono", monospace;

  /* Numérica/data tabular */
  --font-tabular: var(--font-mono);  /* tabular-nums siempre para tablas de números */
}
```

**Por qué Recoleta:** display serif chileno-friendly (warmer que neutral grotesks), proyecta editorial premium sin ser "luxury alienado".

**Por qué IBM Plex Sans:** body grotesk técnico pero humano. Inter Tight (variant) si Plex no disponible, NUNCA Inter regular.

**Por qué JetBrains Mono:** monoespaciado moderno con buena legibilidad para números (RUTs, montos, coordenadas técnicas).

### Type scale

```css
:root {
  /* Type scale — clamp() para responsive automático */
  --type-xs:   clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);   /* 12-14px caption */
  --type-sm:   clamp(0.875rem, 0.85rem + 0.125vw, 0.9375rem); /* 14-15px small */
  --type-base: clamp(1rem, 0.95rem + 0.25vw, 1.125rem);     /* 16-18px body */
  --type-lg:   clamp(1.125rem, 1.05rem + 0.375vw, 1.25rem); /* 18-20px lead */
  --type-xl:   clamp(1.5rem, 1.3rem + 1vw, 1.875rem);       /* 24-30px h3 */
  --type-2xl:  clamp(2rem, 1.7rem + 1.5vw, 2.5rem);         /* 32-40px h2 */
  --type-3xl:  clamp(2.75rem, 2.2rem + 2.75vw, 4rem);       /* 44-64px h1 */
  --type-display: clamp(3.5rem, 2.5rem + 5vw, 6rem);        /* 56-96px hero */

  /* Line heights */
  --leading-tight: 1.1;    /* display, h1, h2 */
  --leading-snug:  1.3;    /* h3, lead */
  --leading-normal: 1.5;   /* body */
  --leading-relaxed: 1.7;  /* texto largo, articles */

  /* Letter spacing */
  --tracking-tight:   -0.02em;  /* display, h1 */
  --tracking-normal:  0;        /* body */
  --tracking-wide:    0.05em;   /* uppercase tags, eyebrow text */
  --tracking-wider:   0.1em;    /* nav links, all-caps section labels */
}
```

### Reglas de aplicación

- **H1 (hero):** Recoleta, type-display, leading-tight, tracking-tight
- **H2:** Recoleta, type-2xl o 3xl, leading-tight
- **H3:** IBM Plex Sans Bold, type-xl, leading-snug
- **Body:** IBM Plex Sans Regular, type-base, leading-normal
- **Lead paragraph (después h1/h2):** type-lg, leading-relaxed, max-width 60ch
- **Caption / footnote:** type-sm, color stone
- **All-caps eyebrow** (above titles): type-xs, tracking-wider, uppercase
- **Números data:** font-tabular SIEMPRE para alineación correcta
- **Code/IDs:** font-mono, type-sm

### Antipatterns

- ❌ Inter en cualquier weight (banned per anti-ai-slop)
- ❌ Roboto, Arial, system-ui (banned)
- ❌ Más de 3 weights por familia (peso de carga)
- ❌ Mezclar > 2 families en mismo viewport
- ❌ Tipografía ALL CAPS en párrafos largos (solo eyebrow/labels)

---

## 4. Components

### Buttons

```css
/* Primary CTA */
.btn-primary {
  background: var(--brand-copper);
  color: var(--brand-cream);
  padding: 0.875rem 1.75rem;  /* generous padding */
  font-family: var(--font-body);
  font-weight: 600;
  font-size: var(--type-base);
  letter-spacing: -0.005em;
  border: none;
  border-radius: 2px;  /* sutil, no muy redondeado — industrial */
  cursor: pointer;
  transition: background 0.2s ease, transform 0.1s ease;
}
.btn-primary:hover {
  background: #A0451F;  /* copper darkened */
  transform: translateY(-1px);
}
.btn-primary:active {
  transform: translateY(0);
}

/* Secondary CTA */
.btn-secondary {
  background: transparent;
  color: var(--brand-charcoal);
  padding: 0.875rem 1.75rem;
  font-family: var(--font-body);
  font-weight: 500;
  font-size: var(--type-base);
  border: 1.5px solid var(--brand-charcoal);
  border-radius: 2px;
  cursor: pointer;
  transition: background 0.2s ease, color 0.2s ease;
}
.btn-secondary:hover {
  background: var(--brand-charcoal);
  color: var(--brand-cream);
}

/* Ghost (links secundarios) */
.btn-ghost {
  background: transparent;
  color: var(--brand-charcoal);
  text-decoration: underline;
  text-underline-offset: 4px;
  text-decoration-thickness: 1.5px;
}
```

**Reglas:** UN primary por viewport. NUNCA dos primary CTAs lado a lado con peso visual idéntico (Impeccable lo flagea como ambiguous priority).

### Cards

```css
.card {
  background: var(--surface-card);
  padding: 2rem;
  border: 1px solid rgba(107, 102, 96, 0.15);  /* stone con low opacity */
  border-radius: 4px;
  transition: border-color 0.2s ease;
  /* SIN box-shadow por default — usar selectivamente */
}
.card:hover {
  border-color: rgba(107, 102, 96, 0.4);
}

/* Card con accent (hero feature) */
.card-accent {
  background: var(--brand-charcoal);
  color: var(--brand-cream);
  border: none;
}

/* PROHIBIDO en MCCO */
.card-glass { /* NO USAR */
  /* backdrop-filter: blur(...) — anti-pattern AI slop */
}
```

### Inputs

```css
.input {
  background: var(--surface-card);
  border: 1.5px solid rgba(107, 102, 96, 0.3);
  border-radius: 2px;
  padding: 0.875rem 1rem;
  font-family: var(--font-body);
  font-size: var(--type-base);
  color: var(--text-primary);
  transition: border-color 0.2s ease;
  /* Label asociado siempre, NO placeholder-as-label */
}
.input:focus {
  outline: none;
  border-color: var(--brand-copper);
  /* Sin glow — industrial, no SaaS friendly */
}
.input:invalid:not(:focus):not(:placeholder-shown) {
  border-color: var(--color-error);
}

/* Para inputs de RUT chileno */
.input-rut {
  font-family: var(--font-mono);
  /* Format on blur: 12.345.678-9 */
}

/* Para inputs de monto CLP */
.input-monto {
  font-family: var(--font-tabular);
  /* Format: $3.500.000 — punto separador miles */
}
```

### Navigation

```css
.nav {
  font-family: var(--font-body);
  font-weight: 500;
  font-size: var(--type-sm);
  letter-spacing: var(--tracking-wider);
  text-transform: uppercase;  /* eyebrow style */
}
.nav-link {
  color: var(--text-primary);
  text-decoration: none;
  padding: 0.5rem 0;
  border-bottom: 1.5px solid transparent;
  transition: border-color 0.2s ease;
}
.nav-link:hover,
.nav-link[aria-current="page"] {
  border-bottom-color: var(--brand-copper);
}
```

---

## 5. Layout

### Grid system

12-col responsive con breakpoints:

```css
:root {
  --container-narrow: 64rem;   /* 1024px — para artículos/text-heavy */
  --container-default: 80rem;  /* 1280px — default sites */
  --container-wide: 96rem;     /* 1536px — solo si justifica (data dashboards) */
  --container-edge: 100%;      /* full-bleed */

  /* Spacing scale (base 4px) */
  --space-1: 0.25rem;   /* 4 */
  --space-2: 0.5rem;    /* 8 */
  --space-3: 0.75rem;   /* 12 */
  --space-4: 1rem;      /* 16 */
  --space-6: 1.5rem;    /* 24 */
  --space-8: 2rem;      /* 32 */
  --space-12: 3rem;     /* 48 */
  --space-16: 4rem;     /* 64 */
  --space-24: 6rem;     /* 96 */
  --space-32: 8rem;     /* 128 — section padding vertical default */
}

@media (min-width: 768px) {
  /* tablet */
}

@media (min-width: 1024px) {
  /* desktop */
}

@media (min-width: 1536px) {
  /* wide */
}
```

### Composition rules

- **Hero:** asymmetric — texto en col 1-7, imagen full-bleed en col 7-12 (right-anchored). NUNCA centrado simétrico.
- **Sections:** padding vertical generoso (--space-24 mobile, --space-32 desktop).
- **Cards grid:** evitar 2x2 perfecta. Preferir alturas variables o grid-breaking element.
- **Whitespace:** generous negative space O controlled density. NUNCA promedio aburrido.

---

## 6. Depth / Elevation

```css
:root {
  /* Shadows — usar selectivamente, no por default */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);    /* hover sutil */
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07);    /* cards lifted */
  --shadow-lg: 0 12px 24px rgba(0, 0, 0, 0.1);   /* modals, popovers */
  --shadow-xl: 0 24px 48px rgba(0, 0, 0, 0.15);  /* hero elements */

  /* Z-index */
  --z-base: 0;
  --z-overlay: 10;
  --z-modal: 100;
  --z-toast: 1000;
}
```

**Regla:** preferir borders sobre shadows. La estética industrial de MCCO favorece edges definidos, no flotantes.

---

## 7. Do's and Don'ts

### Do's

- ✅ Usar Recoleta + IBM Plex Sans + JetBrains Mono (las 3 familias y nada más)
- ✅ Charcoal + Cream + Stone como dominantes, copper como accent
- ✅ Asymmetric hero composition (right-anchored o left-anchored)
- ✅ Generous whitespace en sections principales
- ✅ Tabular-nums en tablas de números, RUTs, montos
- ✅ Borders 1.5px con stone color para definición sin shadow
- ✅ Foto real cuando sea posible (chancador, faena, equipo MCCO real)
- ✅ Microcopy contextual debajo de CTAs ("5 min, sin tarjeta")
- ✅ All-caps eyebrow above section titles, uppercase + tracking-wider

### Don'ts

- ❌ Inter, Roboto, Arial, system-ui (banned)
- ❌ Purple, indigo, lavender, violet (banned)
- ❌ Glass morphism, frosted backdrops (banned)
- ❌ Bento boxes 2x2 simétricas (banned)
- ❌ Dual CTAs idénticos peso visual (banned)
- ❌ Stock 3D abstract (esferas holográficas, mesh gradients)
- ❌ "AI-powered", "Transform your X" copy (banned)
- ❌ Más de 3 colores en mismo section
- ❌ Auto-playing music/video con sound
- ❌ Pop-ups intrusivos al cargar (newsletter, chat) en propuestas serias

---

## 8. Responsive Behavior

### Breakpoints

```
Mobile:  < 768px   (1 col, navigation hamburger)
Tablet:  768-1023  (2 col en cards, navigation visible)
Desktop: ≥ 1024    (12-col grid, navigation full)
Wide:    ≥ 1536    (max-width container, no estiramiento extremo)
```

### Hero responsive

- Desktop: imagen full-bleed derecha, texto izquierda
- Tablet: imagen above, texto below (stacked)
- Mobile: imagen above (cropped si full HD muy alto), texto below
- **Si hero tiene video:** desktop only. Mobile/tablet → still image fallback (regla `web-design-animate.md`)

### Typography responsive

- Type scale usa `clamp()` automático (ver sección 3)
- Display font reduce 30-40% en mobile vs desktop
- Body font sube ligeramente en mobile (mejor legibilidad pulgar lectura)

### Touch targets

- Mínimo 44×44px (Apple HIG / Google Material)
- CTAs en mobile: full-width o casi
- Inputs en mobile: padding generoso vertical

---

## 9. Agent Prompt Guide

Cuando le pidas a Claude Code/Cursor/Codex que genere algo aplicando este DESIGN.md, usá este vocabulario:

### Frases que activan el sistema correctamente

- ✅ "Apply the MCCO Copper design system from `DESIGN-mcco-copper.md`"
- ✅ "Use brand-charcoal as primary surface, brand-copper as CTA accent only"
- ✅ "Use Recoleta display + IBM Plex Sans body, never Inter"
- ✅ "Asymmetric hero, right-anchored composition"
- ✅ "Industrial editorial aesthetic, like Tesla minimal meets WIRED magazine"

### Frases que disparan AI slop (evitar)

- ❌ "Make it modern and clean" (vago, default → slop)
- ❌ "Use a nice color palette" (default → purple gradient)
- ❌ "Modern fonts" (default → Inter)
- ❌ "Beautiful design" (vacío)

### Anti-patterns específicos a recordarle a Claude

```
Cuando construyas, NUNCA uses:
- Inter, Roboto, Arial, system-ui fonts
- Purple/indigo/violet colors anywhere
- Glass morphism, backdrop-filter blur on cards
- Bento 2x2 symmetric grid for features
- Two CTAs with identical visual weight
- "AI-powered" badges or "Transform your X" copy
- Stock 3D abstract imagery for hero
```

### Vocabulary específico MCCO

- "Decoration discipline" (apply across colors and motion)
- "Right-anchored hero" (composition reference)
- "Eyebrow label uppercase tracking-wider" (above titles)
- "Tabular-nums for all numeric data" (RUTs, CLP, dates)
- "Industrial editorial" (overall aesthetic direction)
- "Copper as accent only, < 10% of viewport"

---

## Validación post-build

Después de construir cualquier página con este DESIGN.md:

```
/impeccable critique
```

Score esperado: ≥ 32/40 (good).

Si < 28: revisar adherencia al DESIGN.md, posiblemente cleanup pass:

```
/impeccable polish
/impeccable harden
/impeccable adapt
```

Y re-audit.

---

## Versioning

- **v1.0** (2026-05-06): Initial release. Industrial editorial aesthetic. Charcoal/cream/copper palette. Recoleta + IBM Plex.

Cuando se cambie significativamente la identidad de marca:
- Bump version (v1.1, v2.0)
- Mantener archivo de tokens previo en `examples/archive/DESIGN-mcco-copper-v1.0.md`
- Documentar migration path para sitios existentes
