# Portfolio — Santiago Sanchez | QA Manual & Automation

## Project goal
Build a personal portfolio website using **Astro** to be deployed on **GitHub Pages**.
The site serves as a professional presentation linked from my GitHub profile README.

---

## Tech stack
- **Framework**: Astro (static output, no SSR)
- **Styling**: Plain CSS (no Tailwind, no UI libraries) — custom design system via CSS variables
- **Fonts**: Google Fonts — `Inter` for body, `Sora` for headings
- **Icons**: Inline SVG or lucide-icons via CDN (no icon packages)
- **Animations**: CSS transitions only, no JS animation libraries
- **Deployment**: GitHub Pages via `gh-pages` branch or GitHub Actions

---

## Aesthetic direction
**Minimalist professional — clean, typographic, CV-digital style.**

### Design tokens (define in `src/styles/global.css`)
```css
:root {
  --color-bg:        #FFFFFF;
  --color-bg-subtle: #F7F7F5;
  --color-bg-card:   #FAFAFA;
  --color-border:    #E5E5E3;
  --color-text:      #111110;
  --color-text-muted:#6B6B69;
  --color-accent:    #111110;      /* black as accent — no color, pure type */
  --color-tag-bg:    #F0F0EE;
  --color-tag-text:  #444441;

  --font-sans: 'Inter', system-ui, sans-serif;
  --font-display: 'Sora', sans-serif;

  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  --max-width: 780px;
  --section-gap: 96px;
}
```

Rules:
- No colored backgrounds, gradients, or shadows
- Single consistent left-border `2px solid var(--color-border)` for timeline items
- Tags/badges: pill shape, `--color-tag-bg` fill, no borders
- Section titles: uppercase, letter-spacing 0.1em, 11px, muted color — like a document label
- Hover states: subtle `background: var(--color-bg-subtle)` transitions, no color changes
- All cards: border `1px solid var(--color-border)`, no box-shadow

---

## Site structure

```
src/
  layouts/
    BaseLayout.astro       # <html>, head, global nav, footer
  components/
    Nav.astro              # Fixed top nav — name left, links right
    Hero.astro             # Full viewport section
    About.astro
    Experience.astro
    Skills.astro
    Projects.astro
    Contact.astro
    Footer.astro
  pages/
    index.astro            # Imports all sections in order
  styles/
    global.css             # Design tokens + resets + base typography
public/
  favicon.svg
  og-image.png             # Optional — 1200x630 open graph image
astro.config.mjs
```

---

## Sections — content & layout spec

### Nav
- Left: `Santiago Sanchez` in `--font-display`, 15px, weight 600
- Right: anchor links — About · Experience · Skills · Projects · Contact
- Sticky top, `background: rgba(255,255,255,0.92)`, backdrop-filter blur 8px
- Underline animation on hover (width transition from 0 to 100%)

---

### Hero
- Full viewport height (`100dvh`), centered content
- Large display heading (clamp 48px–72px), `--font-display`
- Line 1: `Santiago Sanchez`
- Line 2 (muted, smaller): `QA Manual & Automation Engineer`
- Subtitle (body, muted): `Building reliable software through thoughtful testing. Based in Uruguay.`
- Two CTA buttons side by side:
  - Primary (filled black): `View Experience`
  - Secondary (outlined): `Download CV` → links to `/CV-Santiago-Sanchez-EN-2026.pdf` in `public/`
- Bottom: subtle scroll indicator (animated arrow or dot)

---

### About
- Section label: `ABOUT`
- Two columns (60/40 split on desktop, stacked on mobile):
  - Left: 3–4 short paragraphs about professional identity, values, approach
  - Right: key facts card —
    - 📍 Uruguay
    - 💼 BOZ IT Staffing → Intermex Wire Transfer
    - ⏱ 2+ years in current role
    - 🎓 Software Testing Specialist (BIOS, 2024–2026)
    - 🌐 GitHub: github.com/santisanchez4
- About me text to write:
  > "I'm a QA Manual & Automation Engineer with 4+ years of experience, currently working at BOZ IT Staffing assigned to Intermex Wire Transfer — a leading fintech for international remittances. I specialize in end-to-end test automation using Playwright and TypeScript, REST API testing with Postman, and CI/CD pipeline management in Azure DevOps. I'm passionate about building maintainable test frameworks that genuinely improve team confidence in deployments. Beyond the day job, I develop personal projects in web and gaming tech, including a tournament platform for the LATAM Warzone community."

---

### Experience
- Section label: `EXPERIENCE`
- Vertical timeline, left border line, entries stacked top to bottom

**Entry 1 — Current**
- Title: `QA Manual & Automation Engineer`
- Company: `BOZ IT Staffing` — Client: `Intermex Wire Transfer`
- Period: `April 2024 – Present`
- Tag: `Fintech · LATAM Remittances`
- Bullet points:
  - End-to-end web automation for transfer and onboarding flows (Playwright + TypeScript)
  - REST API functional and regression testing (Postman)
  - CI/CD pipeline creation, maintenance, and review (Azure DevOps)
  - Performance and load testing (k6)
  - Database queries and validations (SQL Server / SSMS)
  - Test Case and Test Plan management (Azure DevOps Boards & Test Plans)
  - Mobile testing with Appium Inspector on native apps
  - Cross-browser and cross-device cloud testing (LambdaTest)
  - API Testing trainer and mentor for the QA team (certified — BOZ IT Staffing)
  - Creation of functional demo videos using AI tools for stakeholder communication

**Entry 2**
- Title: `QA Automation Engineer`
- Company: `PAGY.CO`
- Period: `2022 – 2024`
- Bullet points:
  - Web test automation using Selenium WebDriver and Java
  - Page Object Model (POM) design pattern implementation
  - Integration of automated tests into CI/CD pipelines
  - Agile methodology (Scrum)
  - Issue tracking, debugging, and task management

---

### Skills
- Section label: `SKILLS`
- Grid of skill categories, each as a card with a title and pill tags

| Category | Skills |
|---|---|
| Languages | TypeScript, Java |
| Frameworks | Playwright, Selenium, JUnit 5, Maven |
| API Testing | Postman |
| Performance | k6 |
| Databases | SQL Server (SSMS) |
| CI/CD | Azure DevOps Pipelines |
| QA Management | Azure DevOps (Boards, Test Plans, Bugs) |
| Version Control | GitHub |
| Mobile Testing | Appium Inspector, LambdaTest |
| AI Tools | Claude, OpenAI, Gemini, DeepSeek, Ollama |

- Grid: `repeat(auto-fit, minmax(200px, 1fr))`, gap 16px
- Each card: white bg, `1px solid var(--color-border)`, border-radius-md, padding 1rem 1.25rem
- Category title: 11px uppercase muted label
- Tags: pill shape, `--color-tag-bg` fill, 13px

---

### Projects
- Section label: `PROJECTS`
- Two cards side by side (stacked on mobile)

**Project 1**
- Title: `Warzone Tournaments`
- URL: `https://warzonetournaments.com`
- Status badge: `In Development`
- Description: "A tournament platform for the Latin American Warzone community. Features private tournament creation, join flows, and host management. Built with a focus on UI/UX and automated E2E testing."
- Tags: `Astro` · `Playwright` · `TypeScript` · `LATAM`
- Link: external icon → warzonetournaments.com

**Project 2**
- Title: `QA Automation Framework`
- URL: `https://github.com/santisanchez4`
- Status badge: `Professional`
- Description: "End-to-end Playwright/TypeScript automation suite for a fintech wire transfer platform. Covers onboarding, transfer flows, and SPA routing edge cases. Integrated with Azure DevOps CI/CD pipelines."
- Tags: `Playwright` · `TypeScript` · `Azure DevOps` · `Fintech`
- Link: GitHub icon

---

### Contact
- Section label: `CONTACT`
- Clean centered layout, no form
- Heading: `Let's connect`
- Subtext: `Open to new opportunities, collaborations, and conversations about QA and automation.`
- Three contact cards in a row:
  - Email: `santisanchez4@gmail.com`
  - GitHub: `github.com/santisanchez4`
  - Location: `Uruguay 🇺🇾`
- Each card: icon + label + value, same card style as Skills

---

### Footer
- Single line, centered
- `© 2026 Santiago Sanchez · Built with Astro`
- Small, muted text

---

## Responsive breakpoints
- Mobile-first CSS
- Single breakpoint at `768px` for two-column layouts
- Nav collapses to hamburger menu on mobile (simple JS toggle, no library)
- Font sizes use `clamp()` for fluid scaling

---

## GitHub Actions deploy (`.github/workflows/deploy.yml`)
Generate a workflow file that:
1. Triggers on push to `main`
2. Runs `npm install` + `astro build`
3. Deploys `dist/` to GitHub Pages using `actions/deploy-pages@v4`

---

## Files to generate
1. `package.json`
2. `astro.config.mjs` — set `site: 'https://santisanchez4.github.io'`, `base: '/portfolio'`
3. `src/styles/global.css`
4. `src/layouts/BaseLayout.astro`
5. `src/components/Nav.astro`
6. `src/components/Hero.astro`
7. `src/components/About.astro`
8. `src/components/Experience.astro`
9. `src/components/Skills.astro`
10. `src/components/Projects.astro`
11. `src/components/Contact.astro`
12. `src/components/Footer.astro`
13. `src/pages/index.astro`
14. `public/favicon.svg`
15. `.github/workflows/deploy.yml`

---

## Notes for Claude Code
- Do NOT use Tailwind, shadcn, or any UI library
- Do NOT use React or any JS framework — pure Astro components
- All styles in `global.css` via CSS variables + scoped `<style>` blocks in each component
- Keep JS minimal — only for nav toggle and smooth scroll
- The CV PDF (`CV-Santiago-Sanchez-EN-2026.pdf`) should be placed in `public/` by the user manually
- Optimize for Lighthouse score: semantic HTML, proper heading hierarchy, alt text on all images
- Add `<meta og:*>` tags in BaseLayout for social sharing