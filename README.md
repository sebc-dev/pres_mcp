# Slidev Template

Template de presentation [Slidev](https://sli.dev) avec layouts personnalises et theme configurable.

## Installation

```bash
curl -fsSL https://raw.githubusercontent.com/sebc-dev/slidev-template/main/create-slidev.sh | bash
```

Le script telecharge le template, installe les dependances npm et prepare le projet.

### Utilisateurs Claude Code

Le template inclut le [skill Slidev](https://github.com/slidevjs/slidev) et une commande `/init` pour finaliser le setup :

```bash
cd mon-projet
claude
# puis dans Claude Code :
/init
```

La commande `/init` installe les dependances et le skill Slidev.

#### Serveurs MCP

Pour ajouter des serveurs MCP partages avec l'equipe, utilisez le scope `project` :

```bash
# Serveur HTTP distant
claude mcp add --scope project --transport http <nom> <url>

# Serveur stdio local
claude mcp add --scope project --transport stdio <nom> -- <commande> [args...]
```

Les serveurs a scope `project` sont stockes dans `.mcp.json` a la racine du projet et peuvent etre archives dans le controle de version. Les autres scopes disponibles :

- `local` (par defaut) : prive, uniquement pour vous dans ce projet
- `project` : partage avec l'equipe via `.mcp.json`
- `user` : disponible pour vous dans tous vos projets

## Layouts

| Layout | Usage | Description |
|--------|-------|-------------|
| `cover-title` | `layout: cover-title` | Image de fond plein ecran + titre centre |
| `chapter` | `layout: chapter` | Image de fond + panneau droit avec titre de chapitre (separation oblique) |
| `content` | `layout: content` | Breadcrumb chapitre/section en haut + zone de contenu |

### Frontmatter

```yaml
---
layout: cover-title
image: /photo.jpg          # image de fond
---

---
layout: chapter
image: /photo.jpg           # image de fond
chapter: Mon Chapitre       # titre du chapitre
section: Ma Section         # sous-titre
---

---
layout: content
chapter: Mon Chapitre       # affiche dans le breadcrumb
section: Ma Section
---
```

## Theme

Editer `styles/index.css` :

```css
:root {
  --theme-primary: #1a365d;       /* couleur principale */
  --theme-accent: #e53e3e;        /* couleur d'accent (separateurs) */
  --theme-font-title: 'Inter';    /* police des titres */
  --theme-font-body: 'Inter';     /* police du corps */
  --theme-cover-overlay: rgba(0, 0, 0, 0.45); /* assombrissement cover */
}
```

## Commandes

```bash
pnpm dev      # serveur de dev → http://localhost:3030
pnpm build    # build statique
pnpm export   # export PDF (necessite playwright-chromium)
```

## Structure

```
mon-projet/
  slides.md             # contenu de la presentation
  styles/index.css      # variables CSS du theme
  layouts/
    cover-title.vue     # slide titre
    chapter.vue         # slide chapitre
    content.vue         # slide contenu
  public/               # images et assets
  .claude/commands/     # commande /init pour Claude Code
  .agents/skills/       # skill Slidev pour Claude Code
```
