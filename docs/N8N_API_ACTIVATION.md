# Activation de l'API n8n - Guide Rapide

## Problème Actuel

Lors des tests de connexion, nous avons reçu une erreur 403 (Forbidden) en tentant d'accéder à l'API n8n.

## Actions Requises dans n8n

Pour que le serveur MCP puisse se connecter à votre instance n8n, vous devez activer l'API publique.

### Étape 1 : Connexion à n8n

1. Accédez à votre instance : https://n8n.srv972813.hstgr.cloud
2. Connectez-vous avec vos identifiants

### Étape 2 : Activer l'API Publique

1. Cliquez sur **Settings** (Paramètres) dans le menu
2. Allez dans la section **API**
3. Activez l'option **"Public API"** (API Publique)
4. Sauvegardez les modifications

### Étape 3 : Vérifier/Générer une API Key

1. Dans **Settings**, allez dans **API Keys** (Clés API)
2. Vérifiez que votre clé actuelle est listée et active
3. Si vous ne voyez pas de clé ou si vous voulez en créer une nouvelle :
   - Cliquez sur **"Create API Key"** (Créer une clé API)
   - Donnez-lui un nom : `Claude Code MCP`
   - Copiez la clé générée (elle ne sera affichée qu'une seule fois)
   - Mettez à jour votre fichier `.env` avec la nouvelle clé

### Étape 4 : Configuration des Permissions (Optionnel)

Si votre instance n8n le permet, configurez les permissions de l'API key :

- **Workflows** : Read, Write, Execute
- **Credentials** : Read (si nécessaire)
- **Executions** : Read

### Étape 5 : Vérification des Restrictions Réseau

Si vous êtes sur Hostinger, vérifiez :

1. **Firewall** : Assurez-vous que l'API n'est pas bloquée par un firewall
2. **IP Whitelist** : Si activée, ajoutez votre IP ou désactivez cette restriction pour les tests
3. **CORS** : Vérifiez que les requêtes API cross-origin sont autorisées

## Test de Connexion

Après avoir activé l'API, testez la connexion :

### Option 1 : Depuis votre terminal

```bash
# Remplacez VOTRE_CLE_API par votre vraie clé
curl -X GET "https://n8n.srv972813.hstgr.cloud/api/v1/workflows" \
  -H "X-N8N-API-KEY: VOTRE_CLE_API"
```

Vous devriez recevoir une liste de vos workflows en JSON (peut être vide si vous n'en avez pas encore).

### Option 2 : Test via le projet

```bash
cd /home/user/Automation-n8n
source .env
curl -X GET "${N8N_URL}/api/v1/workflows" \
  -H "X-N8N-API-KEY: ${N8N_API_KEY}"
```

## Réponses Attendues

### Succès (200 OK)

```json
{
  "data": [
    {
      "id": "1",
      "name": "Mon Workflow",
      "active": true,
      ...
    }
  ]
}
```

ou

```json
{
  "data": []
}
```

(liste vide si aucun workflow)

### Erreur 403 Forbidden

```json
{
  "message": "API not enabled"
}
```

**Solution** : Activez l'API publique dans les paramètres (voir Étape 2)

### Erreur 401 Unauthorized

```json
{
  "message": "Invalid API key"
}
```

**Solution** : Vérifiez que votre clé API est correcte et active

## Configurations Hostinger Spécifiques

### Si l'instance est derrière un proxy

Certaines configurations Hostinger peuvent nécessiter des headers additionnels :

```bash
curl -X GET "https://n8n.srv972813.hstgr.cloud/api/v1/workflows" \
  -H "X-N8N-API-KEY: VOTRE_CLE_API" \
  -H "X-Forwarded-Host: n8n.srv972813.hstgr.cloud"
```

### Si SSL/TLS pose problème

Pour les tests uniquement (NE PAS UTILISER EN PRODUCTION) :

```bash
curl -k -X GET "https://n8n.srv972813.hstgr.cloud/api/v1/workflows" \
  -H "X-N8N-API-KEY: VOTRE_CLE_API"
```

## Configuration Alternative : Trigger MCP Natif

Si l'API REST continue de poser problème, vous pouvez utiliser le trigger MCP intégré de n8n :

### Étape 1 : Créer un Workflow MCP

1. Dans n8n, créez un nouveau workflow
2. Ajoutez le node **"MCP Server Trigger"**
3. Configurez :
   - **URL Path** : `/mcp` (ou personnalisez)
   - **Authentication** : Bearer Token (recommandé) ou None (pour les tests)
4. Connectez vos nodes d'outils
5. Activez le workflow

### Étape 2 : Mettre à Jour la Configuration MCP

Dans `.mcp/mcp_config.json`, modifiez :

```json
{
  "n8n-native": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-http",
      "https://n8n.srv972813.hstgr.cloud/webhook/mcp"
    ]
  }
}
```

## Prochaines Étapes

Une fois l'API activée et testée :

1. ✅ Vérifiez que l'API retourne des données
2. ✅ Testez le serveur MCP dans Claude Code
3. ✅ Créez votre premier workflow via MCP
4. ✅ Consultez le [guide d'utilisation](USAGE_GUIDE.md) pour des exemples

## Support

Si après avoir suivi ces étapes, l'API ne fonctionne toujours pas :

1. Vérifiez les logs de n8n (dans l'interface web ou via Hostinger)
2. Contactez le support Hostinger pour vérifier les restrictions
3. Consultez le [guide de dépannage](TROUBLESHOOTING.md)
4. Vérifiez la documentation n8n pour votre version spécifique

## Références

- [n8n API Documentation](https://docs.n8n.io/api/)
- [n8n Public API Settings](https://docs.n8n.io/hosting/configuration/environment-variables/public-api/)
- [n8n Authentication](https://docs.n8n.io/api/authentication/)

---

**Important** : Une fois l'API activée, refaites un test de connexion avant d'utiliser le serveur MCP.
