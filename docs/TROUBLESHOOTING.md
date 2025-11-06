# Guide de Dépannage - MCP n8n & Context7

## Problèmes de Connexion n8n

### Erreur 403 Forbidden lors de l'accès à l'API

**Symptôme** : Lors du test de connexion à l'API n8n, vous recevez une erreur 403.

**Causes possibles** :

1. **API Key invalide ou expirée**
   - Vérifiez que l'API key est toujours valide dans votre instance n8n
   - Générez une nouvelle API key si nécessaire

2. **Format de header incorrect**
   - n8n accepte généralement : `X-N8N-API-KEY: <votre-clé>`
   - Certaines versions utilisent : `Authorization: Bearer <votre-clé>`

3. **Restrictions d'IP**
   - Vérifiez si votre instance n8n a des restrictions d'IP
   - Sur Hostinger, vérifiez les paramètres de sécurité

4. **API publique désactivée**
   - Dans n8n, allez dans Settings → API
   - Vérifiez que "Public API" est activé

**Solutions** :

### Solution 1 : Vérifier l'API Key dans n8n

1. Connectez-vous à votre instance n8n : https://n8n.srv972813.hstgr.cloud
2. Allez dans **Settings** → **API Keys**
3. Vérifiez que votre clé est bien active
4. Si nécessaire, créez une nouvelle clé :
   - Cliquez sur "Create API Key"
   - Donnez-lui un nom descriptif (ex: "Claude Code MCP")
   - Copiez la clé générée
   - Mettez à jour le fichier `.env` avec la nouvelle clé

### Solution 2 : Activer l'API Publique

1. Dans n8n, allez dans **Settings** → **API**
2. Activez **"Public API"**
3. Sauvegardez les paramètres
4. Redémarrez n8n si nécessaire

### Solution 3 : Tester manuellement l'API

```bash
# Test 1 : Avec X-N8N-API-KEY
curl -X GET "https://n8n.srv972813.hstgr.cloud/api/v1/workflows" \\
  -H "X-N8N-API-KEY: VOTRE_CLE_API"

# Test 2 : Avec Authorization Bearer
curl -X GET "https://n8n.srv972813.hstgr.cloud/api/v1/workflows" \\
  -H "Authorization: Bearer VOTRE_CLE_API"

# Test 3 : Vérifier l'endpoint
curl -X GET "https://n8n.srv972813.hstgr.cloud/api/v1" \\
  -H "X-N8N-API-KEY: VOTRE_CLE_API"
```

### Solution 4 : Vérifier la version de n8n

Certaines versions de n8n ont des configurations d'API différentes.

```bash
# Vérifier la version
curl -s https://n8n.srv972813.hstgr.cloud/api/v1 | grep version
```

### Solution 5 : Configuration Hostinger

Sur Hostinger, vérifiez :
1. Les règles de firewall
2. Les restrictions d'accès à l'application
3. Les paramètres SSL/TLS

---

## Problèmes MCP

### Context7 ne se charge pas

**Symptôme** : Context7 n'apparaît pas dans la liste des serveurs MCP.

**Solutions** :

1. **Vérifier npx** :
```bash
npx --version
```

2. **Tester manuellement** :
```bash
npx -y @upstash/context7-mcp@latest
```

3. **Nettoyer le cache npm** :
```bash
npm cache clean --force
npx clear-npx-cache
```

4. **Vérifier la configuration** :
- Ouvrez `.mcp/mcp_config.json`
- Vérifiez la syntaxe JSON
- Assurez-vous qu'il n'y a pas de virgules en trop

### n8n MCP ne se connecte pas

**Symptôme** : Le serveur MCP n8n ne parvient pas à se connecter à votre instance.

**Solutions** :

1. **Vérifier les credentials** :
```bash
cat .env
# Vérifiez que N8N_URL et N8N_API_KEY sont corrects
```

2. **Tester la connectivité** :
```bash
curl https://n8n.srv972813.hstgr.cloud
```

3. **Vérifier les logs Claude Code** :
- Consultez les logs pour voir les erreurs détaillées
- Recherchez les messages d'erreur du serveur MCP n8n

4. **Réinstaller le package** :
```bash
npm uninstall -g @mcp-tools/n8n-mcp
npm install -g @mcp-tools/n8n-mcp
```

---

## Problèmes de Configuration

### Fichier .mcp/mcp_config.json invalide

**Symptôme** : Claude Code ne charge aucun serveur MCP.

**Solutions** :

1. **Valider le JSON** :
```bash
# Utiliser jq pour valider
cat .mcp/mcp_config.json | jq .
```

2. **Vérifier le format** :
Le fichier doit avoir cette structure :
```json
{
  "mcpServers": {
    "nom-serveur": {
      "command": "commande",
      "args": ["arg1", "arg2"]
    }
  }
}
```

3. **Recréer le fichier** :
Si le fichier est corrompu, supprimez-le et recréez-le :
```bash
rm .mcp/mcp_config.json
# Puis utilisez l'exemple fourni dans MCP_SETUP.md
```

---

## Problèmes de Performance

### Les serveurs MCP sont lents

**Solutions** :

1. **Utiliser des versions spécifiques** :
Au lieu de `@latest`, utilisez une version spécifique :
```json
{
  "context7": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp@1.0.0"]
  }
}
```

2. **Cache npm** :
```bash
# Nettoyer le cache
npm cache verify
```

3. **Vérifier la connexion réseau** :
```bash
ping n8n.srv972813.hstgr.cloud
```

---

## Messages d'Erreur Courants

### "command not found: npx"

**Solution** :
```bash
# Installer Node.js et npm
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs

# Vérifier
npx --version
```

### "EACCES: permission denied"

**Solution** :
```bash
# Option 1 : Utiliser --prefix
npx --prefix=/tmp -y @upstash/context7-mcp@latest

# Option 2 : Corriger les permissions npm
sudo chown -R $USER:$GROUP ~/.npm
sudo chown -R $USER:$GROUP ~/.config
```

### "Cannot find module '@mcp-tools/n8n-mcp'"

**Solution** :
```bash
# Le package officiel pourrait avoir un nom différent
# Essayez :
npx -y n8n-mcp
# ou
npx -y @n8n/mcp-server
```

---

## Checklist de Diagnostic

Avant de demander de l'aide, vérifiez :

- [ ] Node.js est installé (v18+)
- [ ] npm/npx fonctionne
- [ ] L'instance n8n est accessible
- [ ] L'API publique de n8n est activée
- [ ] L'API key n8n est valide
- [ ] Le fichier .env contient les bonnes valeurs
- [ ] Le fichier mcp_config.json est valide (JSON)
- [ ] Les serveurs MCP apparaissent dans `/mcp list`
- [ ] Les logs Claude Code ne montrent pas d'erreurs

---

## Logs et Debug

### Activer les logs détaillés

```bash
# Dans Claude Code, activez le mode debug
export DEBUG=mcp:*

# Ou dans la configuration Claude
{
  "debug": true,
  "logLevel": "verbose"
}
```

### Consulter les logs

```bash
# Logs npm
cat ~/.npm/_logs/*.log

# Logs système (si applicable)
journalctl -u claude-code

# Logs applicatifs
tail -f ~/.claude/logs/*.log
```

---

## Support et Ressources

Si le problème persiste :

1. **Documentation officielle** :
   - [n8n API Docs](https://docs.n8n.io/api/)
   - [MCP Protocol](https://modelcontextprotocol.io/)
   - [Context7 GitHub](https://github.com/upstash/context7)

2. **Communities** :
   - n8n Community Forum
   - MCP Discord
   - Claude Code GitHub Issues

3. **Informations à fournir** :
   - Version de n8n
   - Version de Node.js
   - Système d'exploitation
   - Message d'erreur complet
   - Configuration (sans les credentials)

---

## Configuration Alternative (si problème persistant)

### Utiliser le serveur MCP natif de n8n

Au lieu du package npm, utilisez le trigger MCP intégré à n8n :

1. Dans n8n, créez un nouveau workflow
2. Ajoutez le node "MCP Server Trigger"
3. Configurez l'URL et l'authentification
4. Dans votre configuration MCP Claude, pointez vers cette URL :

```json
{
  "n8n-native": {
    "command": "npx",
    "args": [
      "-y",
      "@modelcontextprotocol/server-http",
      "https://n8n.srv972813.hstgr.cloud/webhook/your-mcp-endpoint"
    ]
  }
}
```

Cette approche peut être plus stable car elle utilise les capacités natives de n8n.

---

**Note** : Ce document sera mis à jour au fur et à mesure que de nouveaux problèmes sont identifiés et résolus.
