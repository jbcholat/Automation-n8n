# Automation-n8n

Projet d'automatisation utilisant n8n avec intégration MCP (Model Context Protocol) pour Claude Code.

## Vue d'ensemble

Ce projet configure et utilise deux serveurs MCP pour améliorer les capacités d'automatisation :

1. **Context7 MCP** - Accès à la documentation à jour des frameworks et bibliothèques
2. **n8n MCP** - Contrôle et création de workflows n8n directement depuis Claude Code

## Instance n8n

- **URL** : https://n8n.srv972813.hstgr.cloud
- **Hébergement** : Hostinger
- **API** : Configurée avec authentification par clé API

## Démarrage Rapide

### 1. Configuration des variables d'environnement

Copiez le fichier d'exemple et configurez vos credentials :

```bash
cp .env.example .env
# Éditez .env avec vos propres valeurs
```

### 2. Configuration MCP pour Claude Code

Copiez la configuration MCP dans votre fichier de configuration Claude Code :

```bash
cat .mcp/mcp_config.json
# Copiez le contenu dans votre configuration Claude
```

### 3. Redémarrez Claude Code

Après avoir ajouté la configuration, redémarrez Claude Code pour charger les serveurs MCP.

### 4. Vérification

Dans Claude Code, vérifiez que les serveurs sont bien chargés :

```
/mcp list
```

Vous devriez voir :
- context7
- n8n

## Documentation

- **[MCP_SETUP.md](docs/MCP_SETUP.md)** - Guide complet de configuration
- **[USAGE_GUIDE.md](docs/USAGE_GUIDE.md)** - Exemples d'utilisation et cas d'usage
- **[TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)** - Résolution de problèmes

## Structure du Projet

```
Automation-n8n/
├── .mcp/
│   └── mcp_config.json          # Configuration des serveurs MCP
├── docs/
│   ├── MCP_SETUP.md            # Guide de configuration
│   ├── USAGE_GUIDE.md          # Guide d'utilisation
│   └── TROUBLESHOOTING.md      # Dépannage
├── workflows/
│   └── (vos workflows n8n)
├── .env                        # Credentials (non versionné)
├── .env.example                # Template des variables
├── .gitignore
└── README.md
```

## Utilisation

### Avec Context7

Obtenez de la documentation à jour pour n'importe quel framework :

```
use context7 - Comment utiliser les Server Actions dans Next.js ?
```

### Avec n8n MCP

Contrôlez votre instance n8n directement :

```
Liste tous mes workflows n8n
```

```
Crée un workflow qui reçoit un webhook et envoie un email
```

```
Exécute le workflow "Mon Workflow" avec ces données de test
```

## Exemples de Workflows

Consultez le [guide d'utilisation](docs/USAGE_GUIDE.md) pour des exemples détaillés :

- Webhook to Database
- Scheduled Data Sync
- Error Monitoring & Alerts
- API Aggregator
- Content Moderation Pipeline

## Prérequis

- Node.js 18+
- npm ou npx
- Accès à une instance n8n
- Claude Code CLI ou Claude Desktop

## Sécurité

- Ne jamais commiter le fichier `.env` dans Git
- Gardez vos API keys confidentielles
- Utilisez des API keys avec permissions limitées si possible
- Régulièrement, régénérez vos clés API

## Contribution

Ce projet est en développement actif. N'hésitez pas à :

- Documenter vos workflows dans `/workflows`
- Ajouter des exemples dans le guide d'utilisation
- Partager vos découvertes et optimisations

## Support

Pour toute question ou problème :

1. Consultez d'abord [TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md)
2. Vérifiez les logs de Claude Code
3. Testez manuellement les connexions API

## Ressources

- [Documentation n8n](https://docs.n8n.io/)
- [n8n API Reference](https://docs.n8n.io/api/)
- [Context7 GitHub](https://github.com/upstash/context7)
- [Model Context Protocol](https://modelcontextprotocol.io/)

## Licence

MIT

---

**Note** : Ce projet nécessite une instance n8n fonctionnelle et configurée pour l'accès API.