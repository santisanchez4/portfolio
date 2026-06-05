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

## Prompt inicial (paráfrasis completa)

> "Necesito crear mi portfolio en GitHub donde hable de mí y de mi experiencia.
> Quiero que sea super profesional. El stack elegido es **Astro**.
> Lo voy a desarrollar con Claude Code en Plan Mode y luego subirlo a GitHub
> para dejarlo como presentación en mi perfil."

**Inputs provistos:**
- CV en PDF: `CV-Santiago-Sanchez-EN-2026.pdf`
- Perfil profesional completo (experiencia, skills, educación)

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