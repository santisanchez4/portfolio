# PROMPT_ORIGIN.md
> Archivo de referencia histórica. Documenta el prompt inicial, decisiones de diseño,
> y contexto completo que dio origen a este proyecto.

---

## Origen del proyecto

**Fecha:** Junio 2026
**Herramienta:** Claude (Anthropic) + Claude Code (Plan Mode)
**Repositorio destino:** `github.com/santisanchez4/portfolio`
**URL final:** `https://santisanchez4.github.io/portfolio`

---

## System prompt (reformulación profesional)

> Reformulado a partir del prompt original aplicando la skill `prompt-engineer`
> (arquitectura XML). Persona: diseñador web senior. Este es el "brief" que un
> diseñador de portfolios de élite usaría para construir este sitio.

```xml
<role>
  Eres un Diseñador Web Senior con más de 10 años de experiencia especializado en
  diseño de portfolios profesionales, branding personal y desarrollo frontend.
  Has construido decenas de sitios de presentación para ingenieros y profesionales
  técnicos, y entiendes cómo traducir una trayectoria profesional en una narrativa
  visual creíble, legible y orientada a conversión.
</role>

<context>
  El cliente es Santiago Sanchez, QA Manual & Automation Engineer (4+ años de
  experiencia, fintech de remesas LATAM). Necesita un portfolio personal que se
  enlazará desde su perfil de GitHub y funcionará como su presentación profesional.
  - Stack: Astro (output estático), desplegado en GitHub Pages bajo base `/portfolio`.
  - Audiencia: reclutadores técnicos, líderes de QA/Engineering y potenciales clientes.
  - Propósito: comunicar credibilidad técnica y profesionalismo en menos de 30 segundos.
  - Insumos provistos: CV en PDF (`CV-Santiago-Sanchez-EN-2026.pdf`) y un perfil
    profesional completo (experiencia, skills, educación).
</context>

<objetivos>
  1. Claridad: que un visitante entienda quién es Santiago y qué hace de inmediato.
  2. Credibilidad: transmitir seniority a través de jerarquía, tipografía y orden.
  3. Conversión: guiar a dos acciones — "Ver experiencia" y "Descargar CV".
  4. Legibilidad: ritmo tipográfico cómodo, ancho de medida controlado, foco en el texto.
</objetivos>

<instrucciones>
  1. Establece una jerarquía semántica estricta: un único <h1> (Hero), <h2> por
     sección y <h3> para sub-ítems (empleos, proyectos).
  2. Define un sistema de design tokens en CSS (color, tipografía, radios, espaciado)
     como única fuente de verdad; nada de valores mágicos dispersos.
  3. Construye las secciones en este orden: Hero, About, Experience, Skills,
     Projects, Contact, Footer.
  4. Diseña mobile-first con un único breakpoint en 768px y tamaños fluidos con clamp().
  5. Trata la accesibilidad como requisito, no como extra: contraste AA, foco visible,
     navegación por teclado, alt text y respeto por prefers-reduced-motion.
  6. Optimiza para Lighthouse: HTML semántico, CSS scoped, JS mínimo.
</instrucciones>

<restricciones>
  - Prohibido: Tailwind, shadcn o cualquier librería de UI; React/Vue u otro framework JS.
  - Sin gradientes, sin sombras, sin fondos de color: paleta monocroma (negro como acento).
  - Ancho máximo de contenido 780px; tipografías Inter (cuerpo) y Sora (títulos).
  - JS sólo para el toggle del menú móvil y el smooth scroll.
  - Cards: borde 1px sólido, sin box-shadow; tags tipo pill sin bordes.
</restricciones>

<filosofia_de_diseno>
  Minimalismo tipográfico — estilo "CV digital". Menos es más: el contenido es el
  protagonista y el diseño desaparece para dejarlo brillar. El espacio en blanco,
  el ritmo vertical y una tipografía impecable hacen el trabajo que en otros sitios
  haría el color. Cada elemento debe justificar su existencia.
</filosofia_de_diseno>

<tono>
  Profesional, sobrio y seguro sin caer en la arrogancia. Voz en primera persona,
  directa y concreta. Inglés en el contenido del sitio (audiencia internacional).
</tono>

<formato_de_salida>
  - Componentes Astro (.astro) con <style> scoped por componente + tokens globales.
  - HTML semántico y accesible (landmarks, headings, aria donde aplique).
  - Entregable desplegable en GitHub Pages con score de Lighthouse alto en todas las
    categorías (Performance, Accessibility, Best Practices, SEO).
</formato_de_salida>
```

**Por qué funciona:** el rol concreto ("diseñador web senior, 10+ años, branding
personal") ancla decisiones consistentes en vez de un genérico "eres un experto". El
`<context>` da el porqué (audiencia, propósito, base de despliegue) para que ninguna
instrucción quede ambigua. Separar `<objetivos>`, `<restricciones>` y
`<filosofia_de_diseno>` evita que el modelo rellene los vacíos con defaults propios —
la causa más común de fallo. Las instrucciones numeradas convierten un encargo difuso
en pasos verificables, y `<formato_de_salida>` fija el criterio de "terminado".

---

**Solicitud original (contexto histórico):**

> "Necesito crear mi portfolio en GitHub donde hable de mí y de mi experiencia.
> Quiero que sea super profesional. El stack elegido es **Astro**.
> Lo voy a desarrollar con Claude Code en Plan Mode y luego subirlo a GitHub
> para dejarlo como presentación en mi perfil."

**Inputs provistos:**
- CV en PDF: `CV-Santiago-Sanchez-EN-2026.pdf`
- Perfil profesional completo (experiencia, skills, educación)

---

## Improvement Plan

> Evaluación del estado actual del portfolio (`src/components/`, `src/styles/global.css`,
> `src/layouts/BaseLayout.astro`) frente al system prompt de arriba.
> **Este es un plan — nada se implementa en este paso.** Prioridad: 🔴 Alta · 🟡 Media · ⚪ Baja.

### Resumen de la evaluación
El sitio cumple bien la **filosofía de diseño** (monocromo, tipográfico, sin sombras ni color)
y las **restricciones técnicas** (Astro puro, CSS con tokens, JS mínimo). Las brechas principales
están en (1) **jerarquía semántica de headings**, que incumple la instrucción #1 del system prompt,
(2) **consistencia de contenido** entre la documentación y el código, y (3) varias mejoras de
**accesibilidad, UX y performance** que elevarían el score de Lighthouse y la credibilidad.

### Contenido / Consistencia
- [ ] 🔴 **Discrepancia de proyectos:** este documento lista 3 proyectos (incluye **CeliacMap**),
  pero `src/components/Projects.astro` sólo define 2 (Warzone Tournaments, QA Automation Framework).
  Decidir la fuente de verdad y alinear: agregar CeliacMap al componente o quitarlo del doc.
- [ ] 🟡 **Copy de About sin impacto cuantificado:** el texto es sólido pero genérico para branding
  personal. Incorporar métricas/resultados (ej. cobertura, reducción de regresiones, nº de pipelines).
- [ ] 🔴 **Assets ausentes referenciados:** `public/og-image.png` y `public/CV-Santiago-Sanchez-EN-2026.pdf`
  se enlazan pero no existen → preview social rota y botón "Download CV" en 404. Agregarlos.

### Accesibilidad
- [ ] 🔴 **Jerarquía de headings rota:** las secciones usan `<p class="section-label">` como título
  visible en lugar de `<h2>`, y se salta de `<h1>` (Hero) directo a `<h3>` (empleos/proyectos).
  Introducir un `<h2>` real por sección (puede quedar visualmente como el label actual).
- [ ] 🟡 **Falta enlace "skip to content"** al inicio del `<body>` para usuarios de teclado/lector.
- [ ] 🟡 **Menú móvil sin cierre con `Escape`** ni manejo de foco al abrir/cerrar (`Nav.astro`).
- [ ] ⚪ **Contraste de texto muted:** verificar `--color-text-muted` (#6B6B69) sobre blanco
  (~4.6:1, AA límite para texto normal); oscurecer levemente si no pasa con holgura.

### UX
- [ ] 🟡 **Sin "scroll spy":** la nav no resalta la sección activa al hacer scroll. Añadir
  highlight con `IntersectionObserver` (JS mínimo, dentro de las restricciones).
- [ ] ⚪ **Sin afordancia "volver arriba"** en secciones inferiores (botón discreto o ancla en footer).

### Performance
- [ ] 🟡 **Google Fonts render-blocking:** ya usa `display=swap`; considerar `<link rel="preload">`
  de los `.woff2` críticos o self-hosting para eliminar el round-trip a `fonts.gstatic.com`.
- [ ] ⚪ **Imágenes sin pipeline `astro:assets`:** `avatar.png` / `og-image.png` van por `public/`
  sin optimizar. Usar el componente `<Image>` de Astro para formatos modernos y tamaños responsivos.

### Diseño
- [ ] ⚪ **Modo oscuro (opcional):** coherente con el minimalismo si se mantiene monocromo
  (invertir tokens). Evaluar costo/beneficio antes de añadir complejidad.
- [ ] ⚪ **Favicon dependiente de fuente:** `favicon.svg` usa `<text>` con 'Sora', no garantizada
  al renderizar el favicon. Convertir el monograma "SS" a paths vectoriales para consistencia.

---

## Decisiones de diseño (elegidas interactivamente)

| Decisión | Opción elegida | Alternativas descartadas |
|---|---|---|
| Estética | Minimalista profesional — blanco/gris, tipografía limpia, CV digital | Dark tech (neon green), Híbrido dark/gamer |
| Secciones | Hero + About + Experience + Skills + Projects + Contact | Sin Projects, Solo esencial |
| Framework | Astro (static) | — |
| Styling | CSS puro con design tokens | Tailwind, shadcn |
| JS framework | Ninguno — Astro puro | React, Vue |

---

## Design system definido

### Tokens CSS (definidos en `src/styles/global.css`)

```css
:root {
  --color-bg:         #FFFFFF;
  --color-bg-subtle:  #F7F7F5;
  --color-bg-card:    #FAFAFA;
  --color-border:     #E5E5E3;
  --color-text:       #111110;
  --color-text-muted: #6B6B69;
  --color-accent:     #111110;
  --color-tag-bg:     #F0F0EE;
  --color-tag-text:   #444441;

  --font-sans:    'Inter', system-ui, sans-serif;
  --font-display: 'Sora', sans-serif;

  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  --max-width:    780px;
  --section-gap:  96px;
}
```

### Reglas de estilo
- Sin gradientes, sin sombras, sin fondos de color
- Timeline con `border-left: 2px solid var(--color-border)`
- Tags: pill shape, fondo `--color-tag-bg`, sin bordes
- Labels de sección: uppercase, letter-spacing 0.1em, 11px, color muted
- Cards: `border: 1px solid var(--color-border)`, sin box-shadow
- Hover: solo `background: var(--color-bg-subtle)`, sin cambios de color

---

## Arquitectura de archivos

```
portfolio/
├── .github/
│   └── workflows/
│       └── deploy.yml              # CI/CD → GitHub Pages
├── public/
│   ├── favicon.svg                 # Monograma "SS"
│   ├── og-image.png                # (agregar manualmente, 1200×630)
│   └── CV-Santiago-Sanchez-EN-2026.pdf  # (agregar manualmente)
├── src/
│   ├── components/
│   │   ├── Nav.astro               # Sticky, blur, hamburger mobile
│   │   ├── Hero.astro              # 100dvh, heading fluid, 2 CTAs
│   │   ├── About.astro             # Grid 60/40 + facts card
│   │   ├── Experience.astro        # Timeline left-border, 2 roles
│   │   ├── Skills.astro            # Grid auto-fit, 10 categorías
│   │   ├── Projects.astro          # 2 cards: Warzone + QA Framework
│   │   ├── Contact.astro           # 3 cards: email/github/location
│   │   └── Footer.astro
│   ├── layouts/
│   │   └── BaseLayout.astro        # Head, OG meta, fonts, Nav, Footer
│   ├── pages/
│   │   └── index.astro             # Importa todas las secciones
│   └── styles/
│       └── global.css              # Design tokens + reset + utilities
├── astro.config.mjs                # site + base + static output
├── package.json
├── tsconfig.json
├── .gitignore
├── CLAUDE.md                       # Spec completa para Claude Code
└── PROMPT_ORIGIN.md                # Este archivo
```

---

## Contenido por sección

### Hero
- Heading: `Santiago Sanchez`
- Subheading: `QA Manual & Automation Engineer`
- Subtítulo: `Building reliable software through thoughtful testing. Based in Uruguay.`
- CTA primario: `View Experience` → `#experience`
- CTA secundario: `Download CV` → `/portfolio/CV-Santiago-Sanchez-EN-2026.pdf`

### About
Texto base provisto al modelo:
> "I'm a QA Manual & Automation Engineer with 4+ years of experience, currently working
> at BOZ IT Staffing assigned to Intermex Wire Transfer — a leading fintech for
> international remittances. I specialize in end-to-end test automation using Playwright
> and TypeScript, REST API testing with Postman, and CI/CD pipeline management in
> Azure DevOps. I'm passionate about building maintainable test frameworks that genuinely
> improve team confidence in deployments. Beyond the day job, I develop personal projects
> in web and gaming tech, including a tournament platform for the LATAM Warzone community."

### Experience — Roles
1. **QA Manual & Automation Engineer** — BOZ IT Staffing / Intermex Wire Transfer — Abril 2024–Presente
2. **QA Automation Engineer** — PAGY.CO — 2022–2024

### Skills — 10 categorías
Languages · Frameworks · API Testing · Performance · Databases · CI/CD · QA Management · Version Control · Mobile Testing · AI Tools

### Projects
1. **Warzone Tournaments** — `warzonetournaments.com` — In Development
2. **CeliacMap** - `github.com/santisanchez4/CeliacMap` - In Development
3. **QA Automation Framework** — `github.com/santisanchez4` — Professional

### Contact
- `santisanchez4@gmail.com`
- `github.com/santisanchez4`
- `Uruguay 🇺🇾`

---

## Deploy

### GitHub Pages (automático via Actions)
1. Push a `main`
2. El workflow en `.github/workflows/deploy.yml` corre:
   - `npm ci`
   - `astro build`
   - Deploy de `dist/` con `actions/deploy-pages@v4`
3. En GitHub → Settings → Pages → Source: **GitHub Actions**

### URL final
```
https://santisanchez4.github.io/portfolio
```

---

## Tareas manuales pendientes al momento de creación

- [ ] Copiar `CV-Santiago-Sanchez-EN-2026.pdf` a `public/`
- [ ] Crear `public/og-image.png` (1200×630) para redes sociales
- [ ] En GitHub: Settings → Pages → Source → GitHub Actions
- [ ] Agregar link al portfolio en el README del perfil de GitHub

---

## Notas técnicas (aplicadas por Claude Code)

- `import.meta.env.BASE_URL` resuelve a `/portfolio` sin trailing slash —
  se normalizó con helper `withBase()` en `BaseLayout.astro` y `Hero.astro`
  para evitar joins tipo `/portfoliofavicon.svg`
- Un solo `<h1>` en Hero para jerarquía semántica correcta
- Todos los anchors de sección presentes: `#about` `#experience` `#skills` `#projects` `#contact`
- Paths con base prefix correctos para CV, favicon y og:image

---

*Generado con Claude (Anthropic) — Junio 2026*