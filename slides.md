---
theme: default
title: Model Context Protocol (MCP)
author: Negus
fonts:
  sans: Inter
  provider: google
---

<div class="mcp-cover">
  <div class="mcp-cover__grid" />
  <div class="mcp-cover__logo">
    <svg fill="currentColor" fill-rule="evenodd" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M15.688 2.343a2.588 2.588 0 00-3.61 0l-9.626 9.44a.863.863 0 01-1.203 0 .823.823 0 010-1.18l9.626-9.44a4.313 4.313 0 016.016 0 4.116 4.116 0 011.204 3.54 4.3 4.3 0 013.609 1.18l.05.05a4.115 4.115 0 010 5.9l-8.706 8.537a.274.274 0 000 .393l1.788 1.754a.823.823 0 010 1.18.863.863 0 01-1.203 0l-1.788-1.753a1.92 1.92 0 010-2.754l8.706-8.538a2.47 2.47 0 000-3.54l-.05-.049a2.588 2.588 0 00-3.607-.003l-7.172 7.034-.002.002-.098.097a.863.863 0 01-1.204 0 .823.823 0 010-1.18l7.273-7.133a2.47 2.47 0 00-.003-3.537z"/><path d="M14.485 4.703a.823.823 0 000-1.18.863.863 0 00-1.204 0l-7.119 6.982a4.115 4.115 0 000 5.9 4.314 4.314 0 006.016 0l7.12-6.982a.823.823 0 000-1.18.863.863 0 00-1.204 0l-7.119 6.982a2.588 2.588 0 01-3.61 0 2.47 2.47 0 010-3.54l7.12-6.982z"/></svg>
  </div>
  <div class="mcp-cover__content">
    <h1>Model Context Protocol</h1>
    <p>Le standard ouvert qui connecte l'IA au monde reel</p>
  </div>
</div>

<!--
Bonjour a tous. Aujourd'hui on va parler du Model Context Protocol, ou MCP.

Vous en avez surement entendu parler ces derniers mois — c'est devenu LE buzzword de l'ecosysteme IA. Mais derriere le buzz, il y a un vrai sujet technique qui nous concerne directement en tant que developpeurs.

MCP, c'est un protocole ouvert, cree par Anthropic fin 2024, qui est en train de devenir le standard pour connecter les modeles de langage a nos outils du quotidien — nos fichiers, nos bases de donnees, notre CI/CD, nos outils metier.

L'objectif de cette presentation, c'est de demystifier MCP. On ne va pas coder un serveur MCP aujourd'hui. On va comprendre ce que c'est, comment c'est architecte, et surtout comment on peut l'utiliser concretement avec Claude Code pour devenir plus productif.

Plan : on va d'abord voir le probleme que MCP resout, puis comprendre l'architecture, et enfin voir des exemples concrets d'utilisation.
-->

<style>
.mcp-cover {
  position: fixed;
  inset: 0;
  background: var(--theme-bg);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  z-index: 0;
}

.mcp-cover__grid {
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32' width='32' height='32' fill='none' stroke='rgb(255 255 255 / 0.05)'%3e%3cpath d='M0 .5H31.5V32'/%3e%3c/svg%3e");
  background-position: 10px 10px;
  mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.6));
  -webkit-mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.6));
}

.mcp-cover__logo {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 115%;
  height: 115%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.04);
  pointer-events: none;
}

.mcp-cover__logo svg {
  width: 100%;
  height: 100%;
}

.mcp-cover__content {
  position: relative;
  z-index: 1;
  text-align: center;
}

.mcp-cover__content h1 {
  font-family: var(--theme-font-title);
  font-size: 3.5rem;
  font-weight: 700;
  color: #f3f3f3;
  margin: 0;
  letter-spacing: -0.03em;
}

.mcp-cover__content p {
  font-family: var(--theme-font-body);
  font-size: 1.4rem;
  color: #9e9e9e;
  margin-top: 1rem;
  font-weight: 400;
}
</style>

---
layout: chapter
chapter: Introduction
section: Le contexte
---

<!--
On va commencer par le constat de depart. Pourquoi est-ce qu'on a besoin de MCP ?
-->

---
layout: content
chapter: Introduction
section: Le contexte
---

<img src="/images/problem.png" class="problem-img" />

<!--
Les LLM sont incroyablement puissants, mais ils ont un gros defaut : ils sont isoles. Nativement, un LLM ne peut pas acceder a vos fichiers, interroger une base de donnees, ou interagir avec vos outils metier. Tout ce qu'il connait, c'est son training data et ce que vous lui collez dans le prompt.

En pratique, qu'est-ce qu'on fait aujourd'hui ? On copie-colle du contexte manuellement. On prend un bout de log, on le colle dans ChatGPT. On copie un schema de base, on le met dans le prompt. C'est fastidieux et ca ne scale pas.

Et si on veut automatiser ca, chaque integration necessite du code specifique. Si vous avez N applications IA — Claude, ChatGPT, Cursor, Copilot — et M outils externes — Jira, Postgres, Figma, Slack — vous vous retrouvez avec N fois M integrations custom a developper et maintenir. C'est le meme probleme qu'on avait avec les chargeurs de telephone avant l'USB-C : chaque marque avait son propre connecteur.

C'est exactement ce probleme que MCP vient resoudre.
-->

<style>
.problem-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
  margin: 0 auto;
}
</style>

---
layout: content
chapter: Introduction
section: Le contexte
---

<img src="/images/cest-quoi-mcp.png" class="slide-img" />

<!--
MCP, c'est le Model Context Protocol. C'est un protocole ouvert et standardise qui permet a n'importe quelle application IA de se connecter a n'importe quel systeme externe — donnees, outils, workflows — de maniere uniforme.

L'analogie officielle, c'est l'USB-C de l'IA. Avant l'USB-C, chaque fabricant avait son propre connecteur. Maintenant, un seul cable pour tout. MCP, c'est pareil : un seul protocole pour connecter n'importe quel LLM a n'importe quel outil.

Au lieu d'avoir N fois M integrations custom, on a N plus M. Chaque application IA implemente le protocole MCP une fois cote client, chaque outil l'implemente une fois cote serveur, et tout le monde peut parler a tout le monde.

Cree par Anthropic fin 2024, le protocole est desormais open source et maintenu par la Linux Foundation, sous l'egide de l'Agentic AI Foundation. Et ce n'est pas qu'Anthropic : OpenAI, Google, AWS, Cloudflare, Block, Microsoft — tous soutiennent et implementent MCP. C'est un vrai standard industrie, pas un outil proprietaire.

Ce qui est important de retenir a ce stade : en tant que developpeur, vous n'avez pas besoin de creer vos propres serveurs MCP. Il en existe deja des centaines, prets a l'emploi. L'idee c'est de les brancher et d'en profiter.
-->

<style>
.slide-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
  margin: 0 auto;
}
</style>

---
layout: chapter
chapter: Architecture
section: Comment ca marche
---

<!--
Maintenant qu'on a compris le pourquoi, voyons le comment. On va rentrer dans l'architecture de MCP, mais pas de panique — ca reste simple et logique.
-->

---
layout: content
chapter: Architecture
section: Comment ca marche
---

<img src="/images/architecture.png" class="slide-img" />

<!--
L'architecture MCP repose sur trois acteurs. C'est un modele client-serveur classique, avec une subtilite.

Premier acteur : le Host. C'est l'application IA que vous utilisez au quotidien — Claude Code, Claude Desktop, Cursor, VS Code avec Copilot, ChatGPT. C'est le point d'entree pour l'utilisateur. C'est lui qui orchestre tout.

Deuxieme acteur : le Client MCP. C'est un composant interne au Host. Il y a un client MCP par serveur connecte. Si vous branchez trois serveurs MCP dans Claude Code, Claude Code cree trois clients MCP en interne, chacun avec sa propre connexion dediee.

Troisieme acteur : le Serveur MCP. C'est un programme — souvent leger — qui expose des capacites a l'IA via le protocole. Par exemple, un serveur MCP Postgres sait lire le schema de votre base et executer des requetes. Un serveur MCP Angular sait chercher dans la doc officielle.

La communication entre Client et Serveur se fait via JSON-RPC 2.0 — un format de messages structure et standardise. C'est simple : le client envoie des requetes, le serveur repond.

Ce qui est important de comprendre : en tant qu'utilisateur de Claude Code, vous ne touchez jamais directement au Client MCP. Vous configurez juste quels serveurs vous voulez connecter, et Claude Code gere tout le reste. C'est transparent.
-->

<style>
.slide-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
  margin: 0 auto;
}
</style>

---
layout: content
chapter: Architecture
section: Comment ca marche
---

<img src="/images/primitives.png" class="slide-img" />

<!--
Un serveur MCP peut exposer trois types de capacites — on les appelle les "primitives". C'est le coeur du protocole.

Premiere primitive : les Tools, les outils. Ce sont des actions executables. Quand Claude Code appelle un tool, il se passe quelque chose dans le monde reel — une requete API est envoyee, un fichier est cree, une requete SQL est executee, un email est envoye. C'est la primitive la plus utilisee. Exemple concret : le tool "search_documentation" du serveur Angular va chercher dans la doc officielle d'angular.dev et renvoie le resultat a Claude.

Deuxieme primitive : les Resources, les ressources. Ce sont des donnees en lecture seule — pas d'effet de bord. Par exemple, le schema d'une base de donnees, le contenu d'un fichier de configuration, une documentation. Ca donne du contexte a l'IA sans executer d'action.

Troisieme primitive : les Prompts, les modeles. Ce sont des templates de prompts reutilisables — des instructions systeme, des exemples few-shot, des workflows predefinis. Ca permet au serveur de guider l'IA sur comment utiliser au mieux ses capacites.

Ce qui est elegant dans MCP, c'est la decouverte dynamique. Quand Claude Code se connecte a un serveur, il commence par demander "qu'est-ce que tu sais faire ?" via des appels comme tools/list, resources/list. Le serveur repond avec la liste de ses capacites, et Claude decide automatiquement quand et comment les utiliser en fonction de votre conversation. Vous n'avez rien a configurer — Claude decouvre et utilise les outils disponibles de maniere autonome.
-->

<style>
.slide-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
  margin: 0 auto;
}
</style>

---
layout: content
chapter: Architecture
section: Comment ca marche
---

<img src="/images/transports.png" class="slide-img" />

<!--
Derniere piece du puzzle technique : comment le Client et le Serveur communiquent-ils concretement ? Il y a deux modes de transport.

Premier mode : STDIO, pour les serveurs locaux. Le serveur MCP tourne sur votre machine, dans un process a cote de Claude Code. La communication passe par stdin et stdout — entree et sortie standard. C'est ultra simple, pas de reseau, pas de latence. C'est le mode le plus courant quand vous travaillez en local. Par exemple, un serveur MCP filesystem qui lit vos fichiers, ou un serveur Git qui accede a votre historique.

Deuxieme mode : Streamable HTTP, pour les serveurs distants. Le serveur tourne quelque part sur le web — chez Sentry, chez Notion, chez Figma. La communication passe par HTTP classique, avec du Server-Sent Events pour le streaming. Ca permet de se connecter a des services heberges avec de l'authentification OAuth.

En pratique, la plupart des serveurs que vous utiliserez au quotidien avec Claude Code seront en STDIO — ils tournent en local sur votre machine. Mais de plus en plus de services proposent des serveurs MCP distants : Sentry, Atlassian, Cloudflare, etc.

Le point cle : en tant qu'utilisateur, vous ne choisissez pas le transport. C'est defini par le serveur MCP que vous connectez. Vous, vous mettez juste la config dans votre .mcp.json et ca marche.
-->

<style>
.slide-img {
  max-width: 100%;
  max-height: 100%;
  object-fit: contain;
  display: block;
  margin: 0 auto;
}
</style>

---
layout: chapter
chapter: En pratique
section: Utilisation et ecosysteme
---

<!--
On a vu la theorie. Maintenant, voyons ce que ca donne concretement dans notre quotidien de dev.
-->

---
layout: content
chapter: En pratique
section: Utilisation et ecosysteme
---

# Exemples d'utilisation

<div class="examples-grid">
  <div class="example-card">
    <span class="example-icon">📐</span>
    <strong>Figma → Code</strong>
    <p>« Implemente le composant Header a partir de la maquette Figma »</p>
  </div>
  <div class="example-card">
    <span class="example-icon">🗄️</span>
    <strong>Postgres</strong>
    <p>« Quels utilisateurs ont ete actifs cette semaine ? Montre-moi le schema de la table users »</p>
  </div>
  <div class="example-card">
    <span class="example-icon">🔀</span>
    <strong>GitHub</strong>
    <p>« Cree une PR avec ces changements et assigne la review a @lead »</p>
  </div>
  <div class="example-card">
    <span class="example-icon">🅰️</span>
    <strong>Angular</strong>
    <p>« Migre ce composant vers le nouveau control flow en suivant les best practices »</p>
  </div>
</div>

<!--
Voici des exemples concrets de ce que MCP permet au quotidien avec Claude Code.

Avec le serveur Figma, vous pouvez demander a Claude de lire une maquette et de generer directement le code du composant. Plus besoin de faire l'aller-retour entre Figma et votre editeur.

Avec Postgres, Claude interroge directement votre base. Il peut lire le schema, ecrire des requetes, et vous donner des resultats contextualises. Au lieu de copier-coller un schema dans le prompt, il y accede en direct.

Avec GitHub, Claude peut creer des issues, ouvrir des PR, commenter, consulter les checks CI. Vous restez dans le terminal et tout se fait via la conversation.

Et avec Angular par exemple, Claude a acces a toute la doc officielle d'angular.dev — les best practices, les exemples de code, les guides de migration. Il peut appliquer ca directement a votre code.

Le point cle : vous n'ecrivez pas de code d'integration. Vous branchez un serveur MCP, et Claude sait l'utiliser.
-->

<style>
.examples-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1rem;
  margin-top: 1rem;
}

.example-card {
  background: #1a1a1e;
  border: 1px solid #2a2a2e;
  border-radius: 0.5rem;
  padding: 1rem;
}

.example-card .example-icon {
  font-size: 1.5rem;
}

.example-card strong {
  display: block;
  color: #f3f3f3;
  margin: 0.25rem 0;
}

.example-card p {
  color: #9e9e9e;
  font-size: 0.85rem;
  margin: 0;
  font-style: italic;
}
</style>

---
layout: content
chapter: En pratique
section: Utilisation et ecosysteme
---

# Installer un serveur MCP

**Via la commande CLI** avec `--scope` pour choisir la portee :

```bash
# Scope project : partage avec l'equipe via .mcp.json
claude mcp add --scope project angular -- npx -y @anthropic/angular-mcp-server

# Serveur HTTP distant
claude mcp add --scope project --transport http notion https://mcp.notion.com/mcp
```

| Scope | Stockage | Visibilite |
|-------|----------|------------|
| `local` | `~/.claude.json` | Vous seul, ce projet |
| `project` | `.mcp.json` | Toute l'equipe (versionne) |
| `user` | `~/.claude.json` | Vous seul, tous les projets |

<!--
Comment on installe un serveur MCP dans Claude Code ? C'est tres simple, avec la commande "claude mcp add".

Le point important, c'est le flag --scope qui determine ou la config est stockee.

Scope "project" : c'est celui que vous utiliserez le plus souvent en equipe. La config est enregistree dans un fichier .mcp.json a la racine du projet. Ce fichier est versionne avec le code — toute l'equipe en profite automatiquement.

Scope "local" : c'est le scope par defaut si vous ne specifiez rien. La config est privee, stockee dans votre ~/.claude.json, et limitee a ce projet. C'est utile pour des serveurs avec des credentials personnelles ou pour tester.

Scope "user" : la config est aussi dans votre ~/.claude.json mais disponible dans tous vos projets. Pratique pour des outils transversaux que vous utilisez partout.

Pour les serveurs distants, comme Notion ou Sentry, c'est le meme principe mais avec --transport http et une URL au lieu d'une commande locale.

Une fois le serveur ajoute, il n'y a rien d'autre a faire. Claude Code decouvre automatiquement les outils disponibles au demarrage et les utilise quand c'est pertinent dans la conversation.
-->

---
layout: content
chapter: En pratique
section: Utilisation et ecosysteme
---

# Installer via `.mcp.json`

Le fichier `.mcp.json` a la racine du projet — versionne et partage avec l'equipe :

```json
{
  "mcpServers": {
    "angular": {
      "command": "npx",
      "args": ["-y", "@anthropic/angular-mcp-server"]
    },
    "notion": {
      "type": "http",
      "url": "https://mcp.notion.com/mcp"
    }
  }
}
```

Au demarrage, Claude Code detecte le fichier et connecte automatiquement les serveurs.

<!--
L'autre methode, c'est d'editer directement le fichier .mcp.json a la racine de votre projet. C'est ce que la commande "claude mcp add --scope project" fait pour vous en coulisse.

Le format est simple : un objet "mcpServers" qui contient un serveur par cle. Pour un serveur local en stdio, vous donnez la commande et ses arguments. Pour un serveur distant en HTTP, vous donnez le type "http" et l'URL.

L'avantage de cette methode, c'est que le fichier est versionne avec le projet. Quand un nouveau developpeur clone le repo et lance Claude Code, tous les serveurs MCP sont deja configures. Pas besoin de documentation supplementaire ou de setup manuel — c'est plug and play.

C'est la methode recommandee pour les projets d'equipe.
-->

---
layout: content
chapter: En pratique
section: Utilisation et ecosysteme
---

# L'ecosysteme aujourd'hui

**Hosts/Clients compatibles** : Claude Code, Claude Desktop, Claude.ai, ChatGPT, Cursor, Windsurf, Cline, VS Code (Copilot), et beaucoup d'autres.

**Serveurs** : un registre officiel sur modelcontextprotocol.io/registry avec des centaines de serveurs communautaires et officiels.

**SDKs disponibles** : TypeScript, Python, Java, Kotlin, C#, et plus.

<!--
L'ecosysteme MCP a explose en quelques mois.

Cote applications compatibles — ce qu'on appelle les Hosts — la liste est longue : Claude Code, Claude Desktop, Claude.ai, ChatGPT, Cursor, Windsurf, Cline, VS Code avec GitHub Copilot, et beaucoup d'autres. Quasiment tous les outils IA majeurs supportent MCP aujourd'hui.

Cote serveurs, il y a un registre officiel sur modelcontextprotocol.io/registry. On y trouve des centaines de serveurs, communautaires et officiels. Des serveurs pour les bases de donnees, les services cloud, les outils de dev, les services SaaS... Si un service populaire existe, il y a probablement un serveur MCP pour lui.

Et pour ceux qui veulent creer leurs propres serveurs — meme si ce n'est pas l'objet de cette presentation — il y a des SDKs officiels en TypeScript, Python, Java, Kotlin, C#, et d'autres. Ca permet d'exposer vos outils internes via MCP si besoin.

Le point cle : vous n'etes pas enferme dans un ecosysteme. MCP est ouvert, multi-plateforme, multi-langage. Un serveur MCP cree pour Cursor fonctionne aussi dans Claude Code, dans VS Code, dans ChatGPT.
-->

---
layout: content
chapter: En pratique
section: Utilisation et ecosysteme
---

# Ce que MCP n'est PAS

- Ce n'est **pas un framework d'agents** (comme LangChain ou CrewAI). C'est une couche d'integration standardisee.

- Ce n'est **pas du RAG**. RAG enrichit la generation de texte avec de la recherche. MCP permet l'interaction bidirectionnelle et l'execution d'actions.

- Ce n'est **pas du function calling** tout seul. MCP s'appuie sur le function calling mais le standardise et l'enrichit avec la decouverte dynamique, les sessions stateful, et la securite.

<!--
Avant de conclure, je veux clarifier quelques confusions courantes. On me pose souvent ces questions.

Premiere confusion : MCP n'est PAS un framework d'agents. Ce n'est pas LangChain, ce n'est pas CrewAI, ce n'est pas AutoGen. Ces frameworks orchestrent des agents avec des boucles de raisonnement, de la planification, de la memoire. MCP, c'est juste la couche d'integration — le tuyau standardise entre l'IA et les outils. Un framework d'agents peut utiliser MCP en interne pour se connecter a des outils, mais ce sont deux choses differentes.

Deuxieme confusion : MCP n'est PAS du RAG. Le RAG — Retrieval Augmented Generation — c'est quand on enrichit le prompt avec des documents retrouves par une recherche semantique. C'est unidirectionnel : on injecte du contexte, et l'IA genere du texte. MCP, c'est bidirectionnel : l'IA peut lire des donnees MAIS aussi executer des actions dans le monde reel. Envoyer un email, creer un fichier, lancer un build — le RAG ne fait pas ca.

Troisieme confusion : MCP n'est PAS juste du function calling. Le function calling — qu'on retrouve dans les APIs d'OpenAI ou d'Anthropic — permet a un LLM d'appeler des fonctions predefinies. MCP s'appuie dessus, mais il ajoute trois choses essentielles : la decouverte dynamique (le LLM decouvre les outils disponibles au runtime), les sessions stateful (la connexion persiste et le serveur peut notifier le client de changements), et la standardisation (un seul protocole pour tous les outils, pas du code custom pour chaque integration).
-->

---

<div class="mcp-cover">
  <div class="mcp-cover__grid" />
  <div class="mcp-cover__logo">
    <svg fill="currentColor" fill-rule="evenodd" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path d="M15.688 2.343a2.588 2.588 0 00-3.61 0l-9.626 9.44a.863.863 0 01-1.203 0 .823.823 0 010-1.18l9.626-9.44a4.313 4.313 0 016.016 0 4.116 4.116 0 011.204 3.54 4.3 4.3 0 013.609 1.18l.05.05a4.115 4.115 0 010 5.9l-8.706 8.537a.274.274 0 000 .393l1.788 1.754a.823.823 0 010 1.18.863.863 0 01-1.203 0l-1.788-1.753a1.92 1.92 0 010-2.754l8.706-8.538a2.47 2.47 0 000-3.54l-.05-.049a2.588 2.588 0 00-3.607-.003l-7.172 7.034-.002.002-.098.097a.863.863 0 01-1.204 0 .823.823 0 010-1.18l7.273-7.133a2.47 2.47 0 00-.003-3.537z"/><path d="M14.485 4.703a.823.823 0 000-1.18.863.863 0 00-1.204 0l-7.119 6.982a4.115 4.115 0 000 5.9 4.314 4.314 0 006.016 0l7.12-6.982a.823.823 0 000-1.18.863.863 0 00-1.204 0l-7.119 6.982a2.588 2.588 0 01-3.61 0 2.47 2.47 0 010-3.54l7.12-6.982z"/></svg>
  </div>
  <div class="mcp-cover__content">
    <h1>A retenir</h1>
    <p>MCP transforme les LLM d'assistants isoles en agents connectes a vos outils reels.</p>
    <p>Un seul protocole, un ecosysteme qui grandit vite, une adoption massive.</p>
    <p class="mcp-cover__link">modelcontextprotocol.io</p>
  </div>
</div>

<!--
Pour resumer en une phrase : MCP transforme les LLM d'assistants isoles en agents connectes a vos outils reels.

Un seul protocole a connaitre. Un ecosysteme qui grandit tres vite — des centaines de serveurs disponibles, tous les acteurs majeurs a bord. Et une mise en place triviale : un fichier .mcp.json ou une commande "claude mcp add", et c'est parti.

Mon conseil pour commencer : installez Claude Code si ce n'est pas deja fait, ajoutez un ou deux serveurs MCP pertinents pour votre stack — Angular, Postgres, GitHub, Figma, ce que vous utilisez au quotidien — et constatez la difference. Vous allez voir que Claude devient beaucoup plus utile quand il a acces a vos outils reels.

Toute la documentation est sur modelcontextprotocol.io. Le registre des serveurs disponibles est sur modelcontextprotocol.io/registry.

Merci ! Je suis disponible pour vos questions.
-->

<style>
.mcp-cover {
  position: fixed;
  inset: 0;
  background: var(--theme-bg);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  z-index: 0;
}

.mcp-cover__grid {
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 32 32' width='32' height='32' fill='none' stroke='rgb(255 255 255 / 0.05)'%3e%3cpath d='M0 .5H31.5V32'/%3e%3c/svg%3e");
  background-position: 10px 10px;
  mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.6));
  -webkit-mask-image: linear-gradient(0deg, #fff, rgba(255, 255, 255, 0.6));
}

.mcp-cover__logo {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 115%;
  height: 115%;
  display: flex;
  align-items: center;
  justify-content: center;
  color: rgba(255, 255, 255, 0.04);
  pointer-events: none;
}

.mcp-cover__logo svg {
  width: 100%;
  height: 100%;
}

.mcp-cover__content {
  position: relative;
  z-index: 1;
  text-align: center;
}

.mcp-cover__content h1 {
  font-family: var(--theme-font-title);
  font-size: 3.5rem;
  font-weight: 700;
  color: #f3f3f3;
  margin: 0;
  letter-spacing: -0.03em;
}

.mcp-cover__content p {
  font-family: var(--theme-font-body);
  font-size: 1.4rem;
  color: #9e9e9e;
  margin-top: 1rem;
  font-weight: 400;
}

.mcp-cover__link {
  margin-top: 2rem !important;
  color: #f3f3f3 !important;
  font-weight: 600 !important;
  font-size: 1.2rem !important;
}
</style>
