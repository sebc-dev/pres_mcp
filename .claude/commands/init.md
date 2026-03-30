---
description: Initialise le projet Slidev apres clonage du template
---

Initialise ce projet Slidev en executant les etapes suivantes dans l'ordre :

1. Installe les dependances avec `pnpm install` (ou `npm install` si pnpm n'est pas disponible)
2. Installe le skill Slidev avec `npx -y skills add slidevjs/slidev`
3. Installe les serveurs MCP necessaires avec le scope `project` pour qu'ils soient partages avec l'equipe via `.mcp.json` :
   ```bash
   claude mcp add --scope project --transport http <nom> <url>
   claude mcp add --scope project --transport stdio <nom> -- <commande> [args...]
   ```
4. Affiche un resume des fichiers cles du projet :
   - `slides.md` : contenu des slides
   - `styles/index.css` : theme et styles
   - `package.json` : scripts disponibles (`dev`, `build`, `export`)
