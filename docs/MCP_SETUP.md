# Configuration des Serveurs MCP - n8n & Context7

## Vue d'ensemble

Ce projet utilise deux serveurs MCP (Model Context Protocol) pour améliorer les capacités d'automatisation avec n8n et l'accès à la documentation à jour :

1. **Context7 MCP** - Documentation à jour pour les frameworks et bibliothèques
2. **n8n MCP** - Contrôle et création de workflows n8n directement depuis Claude Code

## Prérequis

- Node.js 18+ installé
- npm ou npx disponible
- Une instance n8n active (dans notre cas : Hostinger)
- Claude Code CLI ou Claude Desktop

## Architecture du Projet

```
Automation-n8n/
├── .mcp/
│   └── mcp_config.json          # Configuration des serveurs MCP
├── docs/
│   ├── MCP_SETUP.md            # Ce fichier
│   └── USAGE_GUIDE.md          # Guide d'utilisation
├── workflows/
│   └── (vos workflows n8n)
├── .env                        # Credentials (ne pas commiter)
├── .env.example                # Template des variables
├── .gitignore
└── README.md
```

## Configuration Détaillée

### 1. Context7 MCP

Context7 fournit de la documentation à jour pour les frameworks populaires (Next.js, React, Vue, etc.).

#### Configuration

Le serveur est configuré dans `.mcp/mcp_config.json` :

```json
{
  "context7": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp@latest"]
  }
}
```

#### Utilisation

Pour utiliser Context7 dans vos prompts Claude Code :

```
use context7 - Comment utiliser Next.js 15 App Router ?
```

ou

```
@context7 Quelle est la syntaxe pour les Server Actions dans Next.js ?
```

#### Frameworks Supportés

- Next.js
- React
- Vue.js
- Nuxt
- Svelte
- Et bien d'autres...

### 2. n8n MCP

Le serveur MCP n8n permet d'interagir avec votre instance n8n directement depuis Claude Code.

#### Configuration

**Instance n8n (Hostinger)**
- URL : `https://n8n.srv972813.hstgr.cloud`
- API Key : Stockée dans `.env`

**Configuration MCP** (`.mcp/mcp_config.json`) :

```json
{
  "n8n": {
    "command": "npx",
    "args": [
      "-y",
      "@mcp-tools/n8n-mcp",
      "https://n8n.srv972813.hstgr.cloud",
      "<votre-api-key>"
    ]
  }
}
```

**Note de sécurité** : Les credentials sont aussi stockés dans `.env` pour référence.

#### Capacités

Avec le serveur MCP n8n, vous pouvez :

- Lister tous les workflows
- Créer de nouveaux workflows
- Modifier des workflows existants
- Exécuter des workflows
- Consulter l'historique des exécutions
- Gérer les credentials
- Créer et modifier des nodes

## Installation et Activation

### Étape 1 : Configuration de Claude Code

1. Localisez votre fichier de configuration Claude Code
2. Ajoutez le contenu de `.mcp/mcp_config.json` à votre configuration
3. Redémarrez Claude Code

### Étape 2 : Vérification

Pour vérifier que les serveurs MCP sont bien configurés :

```bash
# Dans Claude Code, tapez :
/mcp list
```

Vous devriez voir :
- context7
- n8n

### Étape 3 : Test de Connexion

**Test Context7** :
```
use context7 - Qu'est-ce que le streaming dans Next.js 14 ?
```

**Test n8n** :
```
Liste-moi tous mes workflows n8n
```

## Variables d'Environnement

Le fichier `.env` contient les informations sensibles :

```env
N8N_URL=https://n8n.srv972813.hstgr.cloud
N8N_API_KEY=<votre-clé-api>
MCP_ENABLED=true
```

**IMPORTANT** : Ne jamais commiter le fichier `.env` dans Git !

## Dépannage

### Context7 ne répond pas

1. Vérifiez que npm/npx est installé : `npx --version`
2. Testez manuellement : `npx -y @upstash/context7-mcp@latest`
3. Vérifiez les logs de Claude Code

### n8n MCP ne se connecte pas

1. Vérifiez l'URL de votre instance : `curl https://n8n.srv972813.hstgr.cloud/api/v1/workflows -H "X-N8N-API-KEY: <votre-clé>"`
2. Vérifiez que l'API key est valide
3. Vérifiez que l'instance n8n est accessible depuis votre réseau

### Erreur de permissions

Si vous obtenez des erreurs de permissions :
```bash
npm cache clean --force
npx clear-npx-cache
```

## Meilleures Pratiques

1. **Sécurité** : Ne jamais partager votre API key n8n
2. **Versioning** : Toujours utiliser `.env.example` pour documenter les variables requises
3. **Documentation** : Maintenir à jour ce fichier avec vos découvertes
4. **Workflows** : Sauvegarder régulièrement vos workflows dans le dossier `/workflows`

## Ressources

- [Documentation Context7](https://github.com/upstash/context7)
- [Documentation n8n MCP](https://github.com/czlonkowski/n8n-mcp)
- [Documentation n8n API](https://docs.n8n.io/api/)
- [Model Context Protocol](https://modelcontextprotocol.io/)

## Support

Pour toute question ou problème :
1. Consultez les logs de Claude Code
2. Vérifiez la documentation officielle des serveurs MCP
3. Testez manuellement les commandes npx

## Changelog

- **2025-11-06** : Configuration initiale de Context7 et n8n MCP
  - Ajout de Context7 pour documentation à jour
  - Connexion à l'instance n8n Hostinger
  - Création de la structure de projet
